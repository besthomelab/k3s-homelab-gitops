# 👁️ Hubble UI (网络流量天眼)

Hubble UI 是 Cilium 官方配套的可视化面板，能够以极其震撼的星系图形式，实时展示集群内部微服务之间的网络流量与拦截状态。

## ⚙️ 参数适配 (Configuration)

1. **外部访问 IP**
   * **文件**：`hubble-ui-lb.yaml`
   * **操作**：搜索关键字 `REPLACE_ME`，找到 `io.cilium/lb-ipam-ips` 注解，将示例 IP `10.0.10.51` 替换为你从 Cilium 地址池中划拨出来的可用 IP。

## 📚 详细施工图纸

关于 Hubble 观测组件的开启方法、数据持久化及面板使用指南，请参阅：

👉 **[Cilium Hubble 部署详解：点亮赛博堡垒的上帝视角](https://besthomelab.tech/application/k3s/cilium-hubble/)**