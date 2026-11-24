# containerd
手順書 4-4（Docker リポジトリ・containerd.io・hold）、4-5（config.toml 配布）、5 章（公式 containerd からの入れ替え）に対応。
`files/containerd-config.toml` は ubuntu-26 の `/etc/containerd/config.toml` そのもの
（`disabled_plugins = []`, `SystemdCgroup = true`）。`[ERROR CRI]` を防ぐ。
入れ替え時は kubelet が一時的に CRI を失うため、本番ノードでは事前に `kubectl drain` を推奨。
