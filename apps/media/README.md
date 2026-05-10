# 🎬 Servarr 自动化影音全家桶 (GitOps 部署图纸)

欢迎来到赛博堡垒的影音核心调度区。本目录包含了在 K3s 集群中通过 GitOps (ArgoCD) 自动化部署 Prowlarr、Radarr、Sonarr、Seerr 等核心组件的全部声明式 YAML 图纸。

📖 **图纸解析与完整搭建教程，请务必跳转阅读博客原文：**
👉 [**赛博堡垒的私人影院：基于 GitOps 部署 Servarr 自动化全家桶**](https://besthomelab.tech/application/k3s/servarr/)

---

> **⚠️ 抄作业必看：图纸本土化修改指南**
> 
> 直接 Copy 本目录下的赛博图纸前，请务必在你的代码编辑器中全局确认并替换以下核心参数，否则你的流水线将全部建在别人的工地上：
> 
> 1. **`IngressRoute` 域名替换**：将图纸中的 `*.k3s.besthomelab.tech` 全部替换为你自己规划的泛域名。
> 2. **`NFS` 存储锚点替换（绝对重点！）**：全量排查所有的 `deployment.yaml`，把示例中的 NFS 服务器 IP `10.0.10.10` 和物理路径 `/mnt/nfs/media`，精准替换为你局域网内真实 NAS 的地址与路径。
> 3. **`PVC` 存储配额按需定夺**：对于存放在 Longhorn 上的 `/config` 卷容量（如 `storage: 3Gi`），请根据你实际的追剧规模来填。**高阶建议：初始给最小，后续按需扩容。** 依托 Kubernetes + Longhorn 的云原生特性，以后空间红了，只需改大这个值并 Push，集群会在底层无缝热扩容，连 Pod 都不用重启！
> 4. **⚠️ 跨环境挂载避坑（Docker 独立部署必看）**：如果你没有将 qBittorrent 和 Jellyfin 部署在 K3s 集群内，而是直接在 NAS 上使用 Docker 独立部署，**必须保证容器内的路径映射与 K3s 内部完全一致**。请务必将宿主机的物理路径（如 `/mnt/nfs/media`）映射到容器内部的 `/media` 目录。
> 
>    **Docker Compose 磁盘映射示例：**
>    ```yaml
>    volumes:
>      # 宿主机实际物理路径 : 容器内部标准路径
>      - /mnt/nfs/media:/media
>    ```
>    *原理解析：只有保持内外路径绝对一致，当 K3s 里的 Radarr 告诉外面的 qB “去 `/media/downloads/movies` 下载”时，qB 才能找对地方，后续的跨容器硬链接也才能完美触发。*

---
**Build by CyberBuilder** 🏗️