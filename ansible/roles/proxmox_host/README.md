# proxmox_host
Proxmox ホスト（pve-nec）の設定。

## e1000e Hardware Unit Hang 対策（必須）
NEC Mate の Intel I219-V (e1000e) は、リンクランプは点いたまま通信だけ止まることがある。
ホストと上の VM が**同時に**見えなくなる。TSO/GSO/GRO を切ると再発しない。

- 確認: `journalctl -k -b | grep -iE "e1000e|hang"`（`Detected Hardware Unit Hang`）
- 即時復旧: `ethtool -K nic0 tso off gso off gro off` → `ip link set nic0 down; ip link set nic0 up`
- 恒久対策: `/etc/network/interfaces` の `iface nic0 inet manual` 直下に `post-up ethtool -K ...`（この role が入れる）

## 今後ここに足すもの（未コード化）
no-subscription リポジトリへの切り替え、固定 IP 192.168.0.11 化（vmbr0 と /etc/hosts）。
