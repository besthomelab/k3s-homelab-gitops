# 🔐 Cert-manager (全自动 TLS 签证官)

Cert-manager 是整个赛博堡垒的安全基石，负责通过 Cloudflare DNS 挑战，全自动申请并续期 Let's Encrypt 泛域名证书，为集群内外网的穿透访问提供加密保障。

## ⚙️ 参数适配 (Configuration)

在将图纸推送到 GitHub 前，请根据你的环境修改以下三个文件的 `REPLACE_ME` 部分：

1. **单向加密处理 (kubeseal)**：
   * *💡 前置条件：如果你还没在集群中部署加解密控制器，请务必先完成 [纯正 GitOps 密码管理：Sealed Secrets](/cluster/sealed-secrets/) 的前置基建。*
   * 找到图纸库中的 `01-cloudflare-secret.template.yaml` 文件，将刚才获取的 Token 填入 `api-token` 字段。
   * 
   * 打开你的终端，进入你克隆到本地的工程目录，并执行加密指令：

     ```bash
     # 1. 深入你本地克隆的基础设施目录 (请替换为你自己的实际路径)
     cd <你的本地仓库根目录>/k3s-homelab-gitops/infrastructure/cert-manager

     # 2. 召唤 kubeseal，将明文模版单向加密为安全的生产图纸
     kubeseal --format=yaml < 01-cloudflare-secret.template.yaml > 01-cloudflare-secret.yaml
     ```
   * `01-cloudflare-secret.template.yaml` 文件还原（不要提交到github上）
  
2. **ACME 注册邮箱**
   * **文件**：`02-cluster-issuer-and-cert.yaml`
   * **操作**：将 `spec.acme.email` 修改为你自己的真实邮箱。
   * **作用**：当证书续期出现异常时，Let's Encrypt 会通过此邮箱向你发送紧急通知。

3. **泛域名信息**
   * **文件**：`02-cluster-issuer-and-cert.yaml`
   * **操作**：将 `commonName` 和 `dnsNames` 修改为你自己的域名（例如 `*.k3s.besthomelab.tech`）。

## 🛠️ 深度避坑指南 (Advanced Tips)

* **状态盯盘**：
  部署完成后，使用 `kubectl get certificate -n traefik -w` 实时观察。只有 READY 状态变为 **True**，才代表你的赛博通行证正式生效。

## 📚 详细施工图纸

关于 CF Token 的权限设置细节、DNS 挑战的底层排错逻辑，请访问官方建设指南：

👉 **[Cert-manager 部署详解：拿下赛博堡垒最高权限 TLS 通行证](https://besthomelab.tech/cluster/cert-manager/)**