# Devin 分流规则调查

调查日期为 2026-11-09。

## 域名依据

| 域名后缀 | 官方依据与用途 |
| --- | --- |
| `devin.ai` | [Devin 介绍](https://docs.devin.ai/get-started/devin-intro)链接到 `app.devin.ai`、`cli.devin.ai`；[CLI 快速开始](https://docs.devin.ai/cli/index)使用 `cli.devin.ai` 安装入口与 `static.devin.ai` 下载地址；[SSO 设置](https://docs.devin.ai/enterprise/security-access/sso/okta)使用 `auth.devin.ai`。 |
| `devinapps.com` | [Enterprise Deployment](https://docs.devin.ai/enterprise/deployment/overview)明确要求用户设备通过 HTTPS/443 访问 `*.devinapps.com`，用于 iframe 内的 IDE 和 Desktop 浏览器查看器。 |
| `devinenterprise.com` | [CLI System Configuration](https://docs.devin.ai/cli/enterprise/system-config)以 `acme.devinenterprise.com` 说明企业租户登录主机；[官方中文 AWS VPC 文档](https://docs.devin.ai/zh/enterprise/vpc/aws-setup)也列出此通配域名。 |

## 核验与范围

- 按仓库现有 Surge / Shadowrocket / Loon 共用格式生成 `Devin.list`，每行仅含规则类型与域名，由引用配置指定策略。
- 使用 `DOMAIN-SUFFIX` 同时匹配根域名和各级子域名。`api.devin.ai`、`auth.devin.ai`、`app.devin.ai`、`cli.devin.ai`、`static.devin.ai` 均由 `devin.ai` 覆盖。
- 当前英文 AWS VPC 旧入口会跳转到 Enterprise Deployment，交互式工具域名已在跳转后的现行文档中复核。企业租户后缀另有现行 CLI 配置文档佐证。
- 读取了官方 [CLI 安装脚本](https://cli.devin.ai/install.sh)，其默认下载基址为 `https://static.devin.ai/cli`。本次仅读取脚本文本。
- 第三方身份提供商、GitHub / GitLab、外部 MCP 服务、Devin 托管应用的外部资源，以及 Legacy Windsurf 登录和后端依赖仍需对应规则处理。本列表针对上表中的 Devin 服务域名，本次仅补充下面两项实测 Desktop 依赖，完整 Desktop / Legacy Windsurf 网络依赖尚未核验。
- 本机分流影响用户设备发出的请求。Devin 云端工作区访问项目依赖或内网资源所用的网络由云端配置决定。
- 已做规则格式、去重和代表性域名匹配检查，尚未在 Surge、Shadowrocket 或 Loon 中导入，也未登录 Devin 验证完整会话。

## 本机 Desktop 连接复核

2026-11-09 在运行中的 Devin Desktop 进行只读检查。

- Desktop 的 `language_server_macos_arm` 和三个内置 `devin` 进程均有连接到 `server.codeium.com:443` 的活跃 TCP 连接。通过进程套接字地址与本机 Loon DNS 映射交叉核对。
- Loon 请求记录显示内置 `devin` 和 Devin Helper (Plugin) 访问 `unleash.codeium.com:443`，命中已有 `DOMAIN-SUFFIX,codeium.com` 代理规则。
- 为这两个已观察到的主机添加精确 `DOMAIN` 规则。它们属于共享 Codeium 基础设施，使用本列表也会影响其他客户端对相同主机的访问。
- 此次复核确认了已建立的连接及部分代理命中记录。完整登录、模型调用成功与所有 Desktop 依赖仍需进一步实测。
