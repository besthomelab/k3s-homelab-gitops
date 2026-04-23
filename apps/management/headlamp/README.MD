# ☸️ Headlamp (K8s 可视化面板)

Headlamp 是一款轻量、美观的 Kubernetes 集群管理面板，用于直观查看集群资源状态。

## ⚙️ 参数适配 (Configuration)

在部署前，请根据你的局域网环境修改以下参数：

1. **外部访问 IP**
   * **文件**：`headlamp-access.yaml`
   * **操作**：搜索关键字 `REPLACE_ME`，将 `loadBalancerIP: 10.0.10.50` 修改为你局域网中规划的可用 IP。

## 📚 详细施工图纸

关于 Headlamp 的详细安装步骤、权限配置以及 Token 获取方法，请访问官方建设指南：

👉 **[Headlamp 部署详解：让 K3s 集群管理更直观](https://besthomelab.tech/application/k3s/headlamp/)**