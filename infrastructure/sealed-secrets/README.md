# 🔐 Sealed Secrets (加密守卫)

Sealed Secrets 是本赛博堡垒的终极密码防线。它允许我们将极度敏感的密码、API Token 等明文数据，通过非对称加密转换成极其安全的乱码（SealedSecret），然后放心地存入公开的 Git 仓库中进行 GitOps 管理。

## ⚙️ 参数适配 (Configuration)

本组件为核心加密基础设施，目前已配置为最佳实践状态，**一般情况下不需要修改任何参数即可直接应用**。

**⚠️ 架构避坑说明：**
在 `values.yaml` 中，我们强制注入了 `fullnameOverride: sealed-secrets-controller`。
这是因为官方 Helm Chart 默认会将服务命名为 `sealed-secrets`，而你本地电脑上的 `kubeseal` 命令行工具却默认去寻找名为 `sealed-secrets-controller` 的服务。如果没有这行代码进行强行纠正，你本地在执行加密指令时将无法联通集群。

## 📚 详细施工图纸

关于如何配置本地加密环境、如何使用 `kubeseal` 封装你的第一个密码，以及如何对解密守卫进行“试压测”，请访问官方建设指南：

👉 **[Sealed Secrets 部署指南：实现 100% 纯正的 GitOps 密码管理](https://besthomelab.tech/cluster/sealed-secrets/)**