# join
手順書 4-7（join 前確認）、4-8（トークン発行・join・zone ラベル付け）に対応。
- `/etc/kubernetes/kubelet.conf` があれば参加済みとしてスキップ（冪等）。
- トークンは control-plane で `--ttl 10m` の短命を毎回発行（手で消す必要なし）。
- ラベル `topology.kubernetes.io/zone={{ physical_host }}` は `physical_host` が定義されているノードのみ。
