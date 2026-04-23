# 🕸️ Cilium 网络发牌员 (IPAM & L2 Announcement)

这是本赛博堡垒底层的核心网络基建，用于替代 MetalLB，接管集群的 LoadBalancer IP 分配与二层局域网广播。

## ⚙️ 参数适配 (Configuration)

在应用此配置前，你**必须**将其修改为你物理机的真实环境参数，否则会导致整个集群网络无法被外部访问！

1. **IP 地址池范围**
   * 操作：搜索 `REPLACE_ME`，将 `start` 和 `stop` 修改为你路由器上预留出来的、不会和 DHCP 冲突的 IP 段。
2. **物理网卡绑定 (致命考点)**
   * 操作：使用 `ip a` 命令查看你 K3s 宿主机的真实网卡名（例如 `eth0`, `enp3s0` 等）。将 `interfaces` 下的 `^enp6s18` 替换为你的真实网卡名称。

## 📚 详细施工图纸

关于 Cilium 的 L2 模式原理解析、高可用配置及避坑指南，请参阅：

👉 **[K3s HA 终极指南：Cilium 组网与底层发牌](https://besthomelab.tech/cluster/ha-setup/)**