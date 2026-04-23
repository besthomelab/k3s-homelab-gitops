# 💾 Longhorn (分布式存储大坝)

Longhorn 是本赛博堡垒的底层企业级块存储基建。它将所有物理节点的零散硬盘池化，为上层数据库和核心业务提供高可用（3副本）的 NVMe 级存储卷。

## ⚙️ 参数适配 (Configuration)

在向 ArgoCD 提交本图纸之前，请确保完成以下网络配置的适配：

1. **管理面板访问 IP**
   * **文件**：`values.yaml`
   * **操作**：搜索关键字 `REPLACE_ME`，找到 `io.cilium/lb-ipam-ips` 注解。将示例中的 `10.0.10.52` 替换为你局域网中实际规划的可用 IP。

*(💡 提示：Longhorn 底层依赖极其重要，请确保宿主机已安装 `open-iscsi` 等核心依赖组件。)*

## 📚 详细施工图纸

关于 Longhorn 的物理盘挂载、节点标签调度以及如何将其设为集群默认 StorageClass，请访问官方建设指南：

👉 **[Longhorn 部署详解：为 K3s 打造企业级分布式存储底座](https://besthomelab.tech/cluster/longhorn/)**