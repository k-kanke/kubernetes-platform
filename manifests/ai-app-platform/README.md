# ai-app-platform manifests

Static platform infrastructure, reconciled by Argo CD (`argocd/applications/ai-app-platform.yaml`).

| File | What |
|---|---|
| `namespaces.yaml` | `platform-system` (restricted PSA), `aap-apps` (baseline PSA; generated apps) |
| `rbac.yaml` | Control Plane ServiceAccount + namespaced Role limited to `aap-apps` |
| `control-plane.yaml` | Control Plane Deployment (1 replica, Recreate), SQLite PVC, Service |
| `portal.yaml` | Portal (Next.js) Deployment + Service. Not exposed yet (access path = Phase 0) |
| `networkpolicies.yaml` | default-deny in `aap-apps`; per-role allow rules |

## Created out-of-band (never committed)

```sh
kubectl -n platform-system create secret generic control-plane-secrets \
  --from-literal=token-secret="$(openssl rand -hex 32)"

# optional: real LLM agent (agent image must be built with INSTALL_CLAUDE=true,
# and the Control Plane run with AAP_AGENT=claude in the agent image env)
kubectl -n aap-apps create secret generic agent-credentials \
  --from-literal=ANTHROPIC_API_KEY=...
```

## Prerequisites on the home cluster (Phase 0)

- A **dynamic** StorageClass. The existing `manual` hostPath PVs do not provision
  per-app PVCs; install e.g. local-path-provisioner and set `AAP_STORAGE_CLASS`.
- Backup of the Control Plane SQLite PVC and every `aap-gen-*-data` PVC (plan-2 §9.1).
- `runtime-ingress` NetworkPolicy assumes the ingress controller runs in namespace
  `ingress-nginx`; adjust to the real namespace/gateway.
- Image tags: the manifests use `:latest` placeholders; pin to a commit SHA once CI publishes images.
- Test locally without touching the home cluster: `ai-app-platform/hack/e2e-kind.sh`.
