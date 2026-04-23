# 💧 NFS Provisioner (外接大容量水库)

NFS Subdir External Provisioner 相当于集群里的“全自动水管工”。它负责将外部 NAS (如 TrueNAS, 群晖) 极低成本的 HDD 存储空间接入集群，为 Jellyfin、Nextcloud 等海量数据应用提供按需分配的持久化存储。

## ⚙️ 参数适配 (Configuration)

在向 ArgoCD 提交本图纸进行部署前，你必须将水管指向你真实的物理 NAS：

1. **NAS 连接情报**
   * **文件**：`values.yaml`
   * **操作**：在编辑器中搜索 `REPLACE_ME`，修改以下两个核心参数：
     * `nfs.server`: 替换为你的 NAS 物理 IP。
     * `nfs.path`: 替换为你事先在 NAS 上建好并开放权限的共享目录路径。

## 📚 详细施工图纸

关于为什么有了 Longhorn 还需要 NFS、如何在 NAS 端配置 Mapall 权限，以及如何造一个“假租客”进行打水压测，请访问官方建设指南：

👉 **[接入 NFS 动态存储池：实现海量冷数据的云原生自由](https://besthomelab.tech/cluster/nfs/)**