# K3s HomeLab GitOps 自动化部署指南

本项目使用 ArgoCD 管理 K3s 集群的基础设施与业务应用。通过声明式配置，你可以快速在全新的 K3s 集群中恢复所有的应用服务。

## 1. 环境准备 (Prerequisites)

在执行一键部署之前，请确保你的底层环境已经就绪：

1. **安装 K3s 高可用集群**：
   请参考博客文档完成底层集群的搭建与网络配置：[K3s HA Setup](https://besthomelab.tech/cluster/ha-setup/)
2. **安装 ArgoCD**：
   请参考文档在集群中部署 ArgoCD 控制器：[ArgoCD 部署指南](https://besthomelab.tech/cluster/argocd)

## 2. 环境参数适配 (Configuration)

由于每个人的局域网网段、NAS IP 和存储规划都不尽相同，在下发部署指令前，你必须将本仓库中的环境参数修改为你自己的真实数据。

为了方便大家快速定位需要修改的地方，本仓库采用了标准化的标识：

### 🔍 赛博工地移交指南：全局搜索与替换 (克隆后必做)

本仓库是“赛博堡垒”的专属图纸。如果你 Fork 或 Clone 了本仓库准备建设你自己的集群，请务必在你的代码编辑器（如 VS Code）中执行**全局搜索与替换**。

如果不做这一步，你的总监工（ArgoCD）将会一直跑来我的仓库拉图纸，你的集群将彻底脱离你的控制！

| 全局搜索关键字 (Search) | 替换为你的数据 (Replace With) | 涉及核心组件与作用说明 |
| :--- | :--- | :--- |
| `REPLACE_ME` | 你本地规划的具体参数 | ⚠️ **底层硬装**：主要涉及局域网 LoadBalancer 靓号 IP、物理机器网卡名称、NFS 真实挂载路径等。 |
| `https://github.com/besthomelab/k3s-homelab-gitops.git` | **你的** Git 仓库 URL | 🐙 **总监工图纸源**：**极其重要！** 决定了 ArgoCD 从哪里拉取配置。必须全仓库替换，否则无法实现自我托管。 |
| `besthomelab.tech` | **你的** 专属域名 | 🌐 **七层迎宾大门**：主要涉及 Ingress 路由规则、TLS 证书签发、以及 Homepage 导航页等对外网暴露的配置。 |


**📖 深度阅读：应用专属指南**
随着集群中接入的组件越来越多，我们将具体的配置说明分散到了各个应用目录下。
当你对某个组件（如 Portainer, Longhorn）的配置有疑问时，请直接进入该应用的文件夹，阅读其专属的 `README.md`。

更详细的架构原理、避坑指南和视频演示，请访问建设指南：[赛博工地](https://besthomelab.tech)

## 3. 🔐 敏感信息与密码管理规范 (Sealed Secrets)

*(⚠️ 重要提醒：本仓库中所有的 `secret.yaml` 文件，均已针对博主的私有 K3s 集群进行了绑定加密。如果你直接使用这些文件，在你的集群中是绝对无法解密的！请务必按照以下步骤，生成属于你自己的加密凭证。)*

当你克隆、下载或参考本仓库，并准备在自己的集群中部署需要密码的应用（如数据库、管理面板）时，请执行以下“换牌”操作：

### 3.1 前置环境检查
1. 确保你的 K3s 集群中已成功部署 [sealed-secrets-controller](https://besthomelab.tech/cluster/sealed-secrets/)。
2. 确保你操作的本地电脑已安装 `kubeseal` 命令行工具，并且能够连接到你的集群。

### 3.2 替换密码工作流 (操作指南)

在每个需要凭证的应用目录（例如 `apps/management/xxx/`）下，你会看到一对文件：
* `secret.template.yaml`: 明文模版（这是给你的参考答案，留了填空题）。
* `secret.yaml`: 密封文件（这是给机器读的，目前是博主的版本，你需要把它替换掉）。

**第一步：在模版中填入你自己的密码**
```bash
# 1. 找到对应的应用目录，使用代码编辑器打开 secret.template.yaml
# 2. 将里面的占位符（如 "REPLACE_ME"）替换为你真实的密码并保存
```
*(🚫 警告：为了你自己的安全，绝对不要将填了真实密码的 `secret.template.yaml` 提交到公开网络！)*

**第二步：生成属于你集群的加密文件**
在当前目录下运行 `kubeseal`。它会自动联络你的 K8s 集群获取公钥，并生成一个只有你家集群能解开的 `secret.yaml`：
```bash
# 执行此命令，将直接覆盖掉仓库中原有的、属于博主的 secret.yaml
kubeseal --format yaml < secret.template.yaml > secret.yaml
```

**第三步：提交并部署**
现在，目录下的 `secret.yaml` 已经是专门为你的赛博堡垒定制的加密版本了。
```bash
# 将属于你的加密文件提交到你自己的版本库中
git add secret.yaml
git commit -m "chore: 🔐 替换为我自己集群的加密凭证"
git push
```

当你通过 ArgoCD 或 `kubectl apply` 部署该应用时，你的集群控制器会自动剥开这层“密封壳”，将密码安全地还原给容器使用。


## 4. 执行一键部署 (Deployment)

在 K8s 的架构中，业务应用严重依赖底层基础设施（如存储卷、负载均衡器等）。因此，我们必须分批次下发图纸，并严格等待地基稳固。

请在控制节点依次执行以下操作：

### 第一阶段：浇筑基础设施大坝
提交包含网络、存储（Longhorn / NFS）等核心底座的母应用图纸：
```bash
kubectl apply -f bootstrap/root-infra.yaml
```
**⏳ 等待验收**：
请立刻登录你的 ArgoCD 网页控制台。你会看到 `longhorn`、`nfs-provisioner` 等应用正在自动部署。
**⚠️ 必须等待**：直到这些基础应用的健康状态全部变成绿色的 `Healthy` 和 `Synced`（这可能需要 3-5 分钟），才能进行下一步！

### 第二阶段：拉起上层业务应用
当地基完全稳固后，提交业务应用的图纸：
```bash
kubectl apply -f bootstrap/root-apps.yaml
```
此时切回 ArgoCD 面板，你会看到 `headlamp`、`portainer` 等监控与管理工具开始全自动落成。

### 💡 关于应用的自动与手动状态说明
本项目采用成熟的 GitOps 权限分级策略：
* **基础设施** 开启了 `selfHeal`（自我修复）。任何脱离 Git 在服务器上的手动修改，都会被瞬间强行覆盖。
* **部分业务应用 (如 Portainer)** 开启了自动部署，但**关闭了自我修复**。如果你为了节省服务器内存，手动将该容器缩容至 0，ArgoCD 会允许这种状态存在（亮起黄色的 OutOfSync 提示），方便你在家庭实验室环境中灵活调度资源。