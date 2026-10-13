# Rules

个人维护的代理分流规则，格式参照 [dler-io/Rules](https://github.com/dler-io/Rules)。

## Grok Suite

覆盖 Grok（网页 / App）、xAI API 与控制台、Grok Bot 桌面端（含共享登录域名与托管电脑）。

Surge / Shadowrocket（`[Rule]`）：

```
RULE-SET,https://raw.githubusercontent.com/xingyingyuzhui/Rules/main/Surge/Surge%203/Provider/Grok.list,Grok
```

Loon（添加到现有 `[Remote Rule]`）：

```
https://raw.githubusercontent.com/xingyingyuzhui/Rules/main/Surge/Surge%203/Provider/Grok.list,policy=Grok,tag=Grok,enabled=true
```

`Grok` 需替换为实际存在的策略名称。Loon 也可通过订阅规则界面添加 URL，并选择策略。

注意：

- 放在 Twitter/X 规则之前，否则 `grok.com` 可能被 Twitter 规则抢走；不要把整个 `x.com` 放进 AI 策略组。
- 节点建议美国 / 日本 / 新加坡。`api.x.ai` 在中国大陆被封锁，xAI 未公布可用地区列表。
- 2026-10-30 起 Cursor / Grok Bot 旧登录回退页下线，必须能访问 `accounts.x.ai` 与 `accounts.spacex.ai`。
- 可选：同时使用 Tailscale 时，可加 `IP-CIDR,100.64.0.0/10,DIRECT,no-resolve`。

## Meta AI / Meta Suite

- `MetaAI.list`：Meta AI 网页 / App、Model API 与开发者控制台、Llama、Muse、AI Studio 旧入口，以及共享 Meta 账号登录和 Accounts Center。
- `Meta.list`：包含上述 AI 域名，并补充 Facebook、Messenger、Instagram、Threads、WhatsApp、Meta 账号及 Quest / Oculus 的主要服务与 CDN 域名。

Surge / Shadowrocket（`[Rule]`）：分开设置 AI 与 Meta 策略时，按以下顺序添加；`MetaAI` 和 `Meta` 需替换为配置中实际存在的策略组名称：

```
RULE-SET,https://raw.githubusercontent.com/xingyingyuzhui/Rules/main/Surge/Surge%203/Provider/MetaAI.list,MetaAI
RULE-SET,https://raw.githubusercontent.com/xingyingyuzhui/Rules/main/Surge/Surge%203/Provider/Meta.list,Meta
```

Loon（添加到现有 `[Remote Rule]`，使用相同顺序）：

```
https://raw.githubusercontent.com/xingyingyuzhui/Rules/main/Surge/Surge%203/Provider/MetaAI.list,policy=MetaAI,tag=MetaAI,enabled=true
https://raw.githubusercontent.com/xingyingyuzhui/Rules/main/Surge/Surge%203/Provider/Meta.list,policy=Meta,tag=Meta,enabled=true
```

以上是两种客户端的引用语法；`.list` 文件本身可共用。Loon 在订阅规则界面添加时，只填写对应 URL，分别选择实际存在的策略，并确认订阅已启用、下载成功。语法参照 [Loon 官方配置示例](https://github.com/Loon0x00/LoonExampleConfig/blob/master/example.conf)。

希望全部 Meta 服务使用同一策略时，只需引用 `Meta.list`。

注意：

- `MetaAI.list` 放在 `Meta.list`、其他 Facebook / Instagram 和综合 AI 规则之前，否则 AI 域名可能被更早的规则匹配。
- **Loon 的规则来源优先级为本地规则 > 插件规则 > 订阅规则。** 把订阅移到前面不能覆盖本地或插件中已命中的域名规则，需在请求记录详情中核对命中规则、策略及最终节点。[官方匹配说明](https://nsloon.app/docs/Rule/)
- **共享服务无法按域名区分 AI 与普通社交流量。** Facebook / Instagram 登录、Graph API、图片和附件可能使用 `facebook.com`、`instagram.com`、`fbcdn.net`、`fbsbx.com` 等共享域名，由 `Meta.list` 处理；共享 Meta 登录域名则由优先的 `MetaAI.list` 处理。需要相同出口时，将两个策略组选为同一节点；这也会影响其他 Meta 产品的登录与社交流量。
- 单独引用 `MetaAI.list` 时，仍需已有规则或最终代理策略处理上述共享域名。它不是独立完整的 App 网络白名单，也不能仅靠域名把 WhatsApp / Instagram 内置 AI 单独分流。
- 地区与功能开放情况以官方当前说明为准，网页、App、语音、眼镜、Muse 和 Model API 的范围可能不同。规则只负责网络分流，不保证账号或功能可用；Model API 有独立的[地区使用政策](https://dev.meta.ai/legal/geographic-use-policy)。
- 新规则仅使用域名匹配，未加入整个 Meta ASN、过时 IP 段或整个 Akamai / AWS 网段；对直接连接 IP 的通话等流量不提供完整覆盖。需要保留的细粒度广告拦截规则应放在宽泛的 `Meta.list` 之前。

调查日期：2026-10-13。域名来源、上游版本、纳入理由和验证边界见 [Meta 调查记录](docs/Meta.md)。

### Loon 网页登录后提示地区不可用

先把 MetaAI 与 Meta 绑定到同一手动选择的具体节点，避免不同策略组或自动选择产生不同出口。若仍报错，保持节点不变，临时使用全局代理，以浏览器新的无痕会话重新登录，比较两种模式的结果。

- 全局可用、自动分流不可用：检查订阅下载和启用状态、本地或插件规则覆盖，以及登录前后出现的域名所用最终节点。
- 两种模式都不可用：继续核查出口 IP 的地区识别与账号可用性；仅凭错误发生在登录后，不能认定账号受限或规则遗漏。

在 Loon 请求记录详情中，重点查看 `meta.ai`、`auth.meta.com`、`accountscenter.meta.com`、`facebook.com`、`instagram.com` 和相关共享资源域名。提供诊断信息时仅需域名、命中规则、策略与最终节点，不需要 Cookie、令牌或完整登录 URL。无需为此启用 HTTPS 解密。

## 来源

- Cursor 官方：[sign-in domains](https://cursor.com/help/troubleshooting/sign-in-domains)、[network configuration](https://cursor.com/docs/enterprise/network-configuration)、[Grok Bot proxies](https://cursor.com/docs/grok-bot/proxies.md)
- xAI 官方：[Regions](https://docs.x.ai/developers/advanced-api-usage/regions)
- 社区规则集：dler-io、v2fly/domain-list-community、MetaCubeX/meta-rules-dat、Loyalsoldier/surge-rules、SukkaW、ACL4SSR、viewer12/OverseasAI.list
- Meta：官方 [Meta AI](https://ai.meta.com/meta-ai/)、[Model API](https://dev.meta.ai/docs/overview)、[Muse](https://about.fb.com/news/2026/09/introducing-muse-personal-ai-agent/)、[AI Studio](https://about.fb.com/news/2024/07/create-your-own-custom-ai-with-ai-studio/)、[Threads 域名迁移](https://about.fb.com/news/2025/04/new-features-threads-web-experience/)；社区来源与固定版本详见 [Meta 调查记录](docs/Meta.md)。
