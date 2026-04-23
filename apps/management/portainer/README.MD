# 🐳 Portainer (容器可视化管家)

Portainer 是本集群的专属容器管理面板，提供轻量、直观的图形化界面来管理容器与集群资源。

## ⚙️ 参数适配 (Configuration)

部署前，请务必根据你的局域网网段修改负载均衡的 IP 地址配置：

1. **外部访问 IP (Cilium L2 LB)**
   * **文件**：`portainer-access.yaml`（或你的访问控制清单）
   * **操作**：搜索关键字 `REPLACE_ME`，找到 `io.cilium/lb-ipam-ips` 注解，将示例中的 `10.0.10.55` 替换为你局域网中实际规划的可用 IP。

## 📚 详细施工图纸

关于 Portainer 的完整架构讲解、数据持久化方案以及详细部署流程，请访问官方建设指南：

👉 **[Portainer 部署详解：K3s 集群的二号大管家](https://besthomelab.tech/application/k3s/portainer/)**