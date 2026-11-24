# ansible — ノード準備と join

手作業でのワーカー追加（ubuntu-worker-03）を再現する。バージョンは `group_vars/all.yml` のみ。

| role | 内容 | 元の手順 |
|---|---|---|
| common | swap / モジュール / sysctl | 4-2, 4-3 |
| containerd | Docker repo, containerd.io, config, hold | 4-4, 4-5, 5 |
| kubernetes | pkgs.k8s.io, kubeadm/kubelet/kubectl, hold | 4-6 |
| join | トークン発行, join, zone ラベル | 4-7, 4-8 |

```bash
cd ansible
ansible-galaxy collection install -r requirements.yml
# 既存ノードに差分がないか確認（まず common/containerd/kubernetes）
ansible-playbook site.yml --check --diff -K   # -K: sudo パスワード入力
# 新ノード追加: inventory に追記して
ansible-playbook site.yml --limit ubuntu-worker-04 -K
```

## 注意
- sudo はパスワード必須のため `-K` が必要（未検証: 実ノードへの `--check --diff` はまだ流していない。構文チェックのみ通過）。
- apt リポジトリは既存ノードの `.list` と重複しないよう `apt_repository` を使用（ansible-core 2.25 で削除予定、その時 `deb822_repository` へ移行）。
- 未確認: ubuntu-26 / worker-01 / worker-02 の `physical_host`（未設定の間は zone ラベルをスキップ）。
- containerd の入れ替えは再起動を伴う。稼働中ノードは先に `kubectl drain`。
- `--check` では `containerd.io` 未導入ノードの join preflight は失敗しうる（新ノードは実行で確認）。
