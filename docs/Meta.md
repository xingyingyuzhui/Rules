# Meta 规则调查记录

调查日期：2026-10-13。本记录用于解释 `MetaAI.list` 与 `Meta.list` 的域名选择，区分官方服务入口、社区维护的基础设施域名和未做实机验证的部分。

## 官方资料核查

| 资料 | 核查结果及规则用途 |
| --- | --- |
| [Meta AI 产品说明](https://ai.meta.com/meta-ai/) | 官方列出网页 `meta.ai`、独立 App，以及 WhatsApp、Instagram、Messenger、Facebook 内的入口；`ai.meta.com` 是产品说明站。采用 `meta.ai` 和 `ai.meta.com` 后缀。 |
| [Meta AI App 公告](https://about.fb.com/news/2025/04/introducing-meta-ai-app-new-way-access-ai-assistant/) | 确认 App、网页与眼镜的关联，以及 Facebook / Instagram Accounts Center 关联。公告中的初始地区范围有日期背景，不当作当前完整地区名单。 |
| [Meta Model API 文档](https://dev.meta.ai/docs/overview) | 当前 API 地址为 `https://api.meta.ai/v1`，开发者入口为 `dev.meta.ai`。二者已由 `meta.ai` 后缀覆盖，不再添加重复精确规则。 |
| [Model API 地区说明](https://dev.meta.ai/help/accounts-and-login/supported-countries)、[地区使用政策](https://dev.meta.ai/legal/geographic-use-policy) | API 与消费端有不同的可用范围。调查时政策更新日期为 2026-10-12，明确列出中国（含香港、澳门）等受限地区；不能把 API 政策当作消费端地区名单，也不能把代理分流当作服务开放承诺。 |
| [Llama 官网](https://www.llama.com/)及 Meta 官方站点的 Llama 导航 | 将 Meta 的 Llama 官方入口纳入 AI 规则，使用 `llama.com` 后缀；第三方模型托管商不属于这一域名范围。 |
| [Muse 官方公告](https://about.fb.com/news/2026/09/introducing-muse-personal-ai-agent/)、[安全设计说明](https://security.muse.ai/) | 2026 年新增服务入口 `muse.ai`，官方公告还链接其子域。采用后缀规则；安全说明目前重定向至 `research.meta.ai`，已被 `meta.ai` 覆盖。 |
| [AI Studio 官方公告](https://about.fb.com/news/2024/07/create-your-own-custom-ai-with-ai-studio/) | 官方历史入口包含 `aistudio.instagram.com`，按精确域名保留。公告在 2026-08-10 更新为停止新建与编辑角色、保留现有角色和聊天，不把它描述为仍可创建新角色的服务。 |
| [Threads 官方公告](https://about.fb.com/news/2025/04/new-features-threads-web-experience/) | 确认由 `threads.net` 迁移至 `threads.com`。保留新域名与旧入口。 |
| [Meta 登录页](https://auth.meta.com/login)、[Accounts Center](https://accountscenter.meta.com/) | 公开页面和 HTTP 重定向核查：前者返回登录页 HTML，后者进入 `auth.meta.com` 登录流程；登录页引用 `static.xx.fbcdn.net`。AI 列表精确匹配两个共享账号主机，CDN 由 Meta 列表处理。没有登录账号或操作账号设置。 |

## 社区规则交叉核查

以下链接固定到调查时的上游版本，避免后续更新改变本次选择的依据：

| 来源 / 固定版本 | 用途与局限 |
| --- | --- |
| [v2fly/domain-list-community：`c6adacf`](https://github.com/v2fly/domain-list-community/tree/c6adacf29cc4fb05e04651fcf03e00989ef9f797/data) | 核查 `meta`、`facebook`、`instagram`、`messenger`、`threads`、`whatsapp`、`oculus`。`meta` 是包含多个子清单的组合分类，不能只读顶层几个域名。 |
| [MetaCubeX/meta-rules-dat：`83c8208`](https://github.com/MetaCubeX/meta-rules-dat/blob/83c8208bdfae92b1caaa29745f51f233b6f1847e/geo/geosite/meta.list) | 核查生成的 Meta 集合，确认服务与 CDN 域名存在。生成集合可能复用其他社区数据，不把它与 v2fly 当作完全独立的实测证据。 |
| [blackmatrix7/ios_rule_script：`19c8468`](https://github.com/blackmatrix7/ios_rule_script/tree/19c84682407580056d0ae2f27e406f70db679de1/rule/Surge) | 核查 Facebook、Instagram、Threads、Whatsapp 的 Surge 格式。Threads 清单仍只列旧域名，需要结合官方公告补充；Whatsapp 包含共享服务域名、关键词及历史 IP 段，不照单复制。 |
| [dler-io/Rules：`456b268`](https://github.com/dler-io/Rules/blob/456b26844509482d49e0b7b1ed68023aaa506dcd/Surge/Surge%203/Provider/AI%20Suite.list) | 参照仓库已有规则的 `DOMAIN` / `DOMAIN-SUFFIX` 风格；AI Suite 已收录 `meta.ai`。当前默认分支为 `main`，没有单独的 Facebook / Instagram / WhatsApp Provider 文件；未将不存在的路径作为来源。 |

### 域名选择

| 分组 | 保留域名 / 匹配范围 | 依据 |
| --- | --- | --- |
| AI 服务 | `meta.ai`、`ai.meta.com`、`llama.com`、`muse.ai` | 官方入口与文档；社区 Meta 集合也收录主要根域名。 |
| AI 基础设施 | `metaaivm.com` | v2fly 的 `meta` 源清单及 MetaCubeX 集合收录。归入 Muse VM 基础设施是社区证据支持的分类；未找到公开官方网络白名单，不承诺所有子域的具体用途或实际可用性。 |
| AI Studio / 共享账号 | 精确匹配 `aistudio.instagram.com`、`auth.meta.com`、`accountscenter.meta.com` | 官方公告、公开登录页面及重定向。后两个也服务其他 Meta 产品。 |
| Meta 主站 / 社区 | `meta.com`、`atmeta.com` | 官方站点与社区 Meta 分类；`meta.com` 同时覆盖账号、企业及商店等服务。 |
| Facebook / 短链接 | `facebook.com`、`facebook.net`、`fb.com`、`fb.me`、`fb.gg`、`fb.watch`、`fbwat.ch` | v2fly Facebook 与 MetaCubeX 集合。 |
| 共享静态资源 / 附件 / 网络基础设施 | `fbcdn.com`、`fbcdn.net`、`fbsbx.com`、`fbsbx.net`、`tfbnw.net`；精确匹配 `fbcdn-a.akamaihd.net` | 社区清单；`static.xx.fbcdn.net` 另有登录 HTML 佐证。只匹配特定 Akamai 主机。 |
| Messenger | `messenger.com`、`fbmessenger.com`、`m.me` | v2fly Messenger 与 MetaCubeX。 |
| Instagram | `instagram.com`、`cdninstagram.com`、`ig.me`、`igcdn.com`、`igsonar.com`、`instagr.am` | v2fly、MetaCubeX、blackmatrix7 交叉核查；不同来源收录范围不完全一致。 |
| Threads | `threads.com`、`threads.net` | 官方迁移公告及社区 Threads 分类。 |
| WhatsApp | `whatsapp.com`、`whatsapp.net`、`whatsappbrand.com`、`wa.me` | v2fly WhatsApp、MetaCubeX；blackmatrix7 也收录两个主要服务域名。 |
| Quest / Oculus | `oculus.com`、`oculuscdn.com`、`oculusvr.com`、`ocul.us` | v2fly Oculus 与 MetaCubeX；仅选主要服务与 CDN 域名。 |

所有新规则仅使用 `DOMAIN` / `DOMAIN-SUFFIX`，不含策略名称。格式与匹配语义参照 [Surge 域名规则](https://manual.nssurge.com/rules/domain.html)及[外部 RULE-SET 文档](https://manual.nssurge.com/rules/ruleset.html)。

客户端引用语法需要分别配置：Loon 使用 `[Remote Rule]` 中的订阅 URL 与 `policy=`，参照 [Loon 官方配置示例](https://github.com/Loon0x00/LoonExampleConfig/blob/master/example.conf)。[Loon 官方优先级说明](https://nsloon.app/docs/Rule/)明确本地规则 > 插件规则 > 订阅规则，因此仅调整订阅之间的顺序不能覆盖本地和插件中的域名匹配。

## 分流设计与边界

`MetaAI.list` 含 8 条规则，`Meta.list` 含 38 条规则。后者可以单独使用，覆盖前者所有匹配范围；两个列表一起使用时，AI 列表必须先于 Meta 列表。跨文件的覆盖是为了支持不同策略组，单个文件内不重复添加已被后缀覆盖的子域。

AI 列表未加入整个 `facebook.com`、`instagram.com` 或 `fbcdn.net`，这些共享服务由 Meta 列表处理。`graph.facebook.com` 已被 `facebook.com` 覆盖，不能仅凭这个主机认定请求是 AI、WhatsApp 或广告。App 内 AI 与社交功能共用域名时，域名规则无法区分请求用途。

未采用宽泛关键词、整个 Akamai / AWS 域名或网段、整个 Meta ASN、历史 WhatsApp IP 段，以及大量拼写变体、营销、防御注册和非核心开发工具域名。清单是主要服务的维护集合，不是全部 Meta 资产清单；直接连接 IP 的实时通话等流量需要客户端日志另行确认。

## 验证记录

- 静态校验：UTF-8、每行字段、域名格式、重复规则、单文件后缀冗余；核对每个域名的官方或固定上游来源。
- 匹配校验：API / 控制台、共享登录、AI Studio、Muse VM 子域、Threads 新旧入口、共享 CDN；确认第三方相似域名不匹配、AI 优先顺序正确，以及 Meta 单文件覆盖 AI 列表。
- 保持原 `Grok.list` 不变。
- 未进行 Surge / Loon / Shadowrocket 实机导入，未进行登录后的聊天、图片、语音、API 推理或眼镜测试；`meta.ai` 首页抓取遇到 HTTP 403 客户端挑战，没有将抓取失败解释成服务不可用。
- 静态规则与公开来源核查无法证明完整 App 流量覆盖；后续补充应以客户端实际请求日志及最新官方说明为依据。
