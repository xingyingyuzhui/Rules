# Rules

个人维护的代理分流规则，格式参照 [dler-io/Rules](https://github.com/dler-io/Rules)。

## Grok Suite

覆盖 Grok（网页 / App）、xAI API 与控制台、Grok Bot 桌面端（含共享登录域名与托管电脑）。

Surge / Loon / Shadowrocket：

```
RULE-SET,https://raw.githubusercontent.com/xingyingyuzhui/Rules/main/Surge/Surge%203/Provider/Grok.list,Grok
```

注意：

- 放在 Twitter/X 规则之前，否则 `grok.com` 可能被 Twitter 规则抢走；不要把整个 `x.com` 放进 AI 策略组。
- 节点建议美国 / 日本 / 新加坡。`api.x.ai` 在中国大陆被封锁，xAI 未公布可用地区列表。
- 2026-10-30 起 Cursor / Grok Bot 旧登录回退页下线，必须能访问 `accounts.x.ai` 与 `accounts.spacex.ai`。
- 可选：同时使用 Tailscale 时，可加 `IP-CIDR,100.64.0.0/10,DIRECT,no-resolve`。

## 来源

- Cursor 官方：[sign-in domains](https://cursor.com/help/troubleshooting/sign-in-domains)、[network configuration](https://cursor.com/docs/enterprise/network-configuration)、[Grok Bot proxies](https://cursor.com/docs/grok-bot/proxies.md)
- xAI 官方：[Regions](https://docs.x.ai/developers/advanced-api-usage/regions)
- 社区规则集：dler-io、v2fly/domain-list-community、MetaCubeX/meta-rules-dat、Loyalsoldier/surge-rules、SukkaW、ACL4SSR、viewer12/OverseasAI.list
