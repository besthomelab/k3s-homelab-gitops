# 🚦 Traefik (总网关)

Traefik 是一款云原生边缘路由器，负责整个集群的流量分发、安全卸载（HTTPS）以及负载均衡。

## ⚙️ 参数适配 (Configuration)

在部署前，请根据你的局域网环境修改以下参数：

1. **全局网关 IP 与证书挂载**
   * **文件**：`values.yaml`
   * **操作**：
     * 搜索 `loadBalancerIP`，修改为你局域网中预留给网关的静态 IP。
     * 确保 `defaultCertificate.secretName` 指向刚才由 Cert-manager 生成的 Secret 名称。

2. **控制面板访问域名**
   * **文件**：`dashboard-route.yaml`
   * **操作**：将 `Host` 匹配规则修改为你规划的面板域名（例如 `traefik.k3s.besthomelab.tech`）。

## 💡 核心特性

* **白嫖全局证书**：所有 `IngressRoute` 只需声明 `tls: {}` 即可自动调用泛域名证书，无需重复配置。
* **HTTP/3 加速**：已默认开启 QUIC 支持，提供极致的访问体验。
* **自动跳转**：所有 80 端口的 HTTP 请求将强制重定向至 443 端口。

## 📚 详细施工图纸

关于 Traefik 的详细安装、Dashboard 暴露以及 PVE/OPNsense 等外部设备接入的进阶玩法，请访问：

👉 **[Traefik 部署详解：构建优雅的云原生网关入口](https://besthomelab.tech/cluster/traefik/)**