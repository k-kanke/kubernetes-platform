# monitoring

Grafana は k8s の外に移した。k8s 内の Grafana（kube-prometheus-stack 同梱）は無効化済み。

- Grafana: Proxmox (pve-nec) 上の LXC、http://192.168.0.13:3000（Tailscale 経由で IP 直アクセス。Ingress なし）
- データソース: k8s の Prometheus を NodePort 30090（http://192.168.0.20:30090）で参照
  - NodePort は `argocd/applications/kube-prometheus-stack.yaml` の values で固定
  - 認証なし。家の中と Tailscale 経由だけで使う前提

このディレクトリは Argo CD の `monitoring-config` Application の参照先。
マニフェストが 0 件でもパスが消えないよう、この README を置いている
（パスが無いと同期エラーになる）。
