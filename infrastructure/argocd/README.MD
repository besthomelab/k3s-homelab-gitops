# 🐙 ArgoCD (GitOps 持续交付总监)

ArgoCD 是本赛博堡垒的核心“监工”，负责监听 Git 仓库的图纸变化，并将所有的基础设施与业务应用全自动下发到 K3s 集群中。

## ⚙️ 参数适配 (Configuration)

为了让你能从局域网直接访问 ArgoCD 的可视化面板，我们需要为其分配一个固定的 LoadBalancer IP。

1. **外部访问 IP**
   * **文件**：`service.yaml` (或对应的网络配置清单)
   * **操作**：搜索关键字 `REPLACE_ME`，找到 `loadBalancerIP: 10.0.10.53`，将其修改为你局域网中实际规划的可用 IP。

## 📚 详细施工图纸

关于 ArgoCD 的初次手动浇筑、如何破译初始最高权限密码，以及如何让 ArgoCD 实现“自我管理”和配置防篡改，请前往官方建设指南阅读完整教程：

👉 **[ArgoCD 部署指南：打造全自动驾驶的 GitOps 引擎](https://besthomelab.tech/cluster/argocd)**