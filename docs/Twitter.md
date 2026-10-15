# X / Twitter 规则调查记录

调查日期：2026-10-16。`Twitter.list` 共 15 条域名规则，用于 X 社交服务分流；Grok 独立服务继续由现有 `Grok.list` 处理。

## 官方资料与公开页面

| 来源 | 核查结果与用途 |
| --- | --- |
| [X OAuth 2.0 授权流程](https://docs.x.com/fundamentals/authentication/oauth-2-0/authorization-code) | 当前文档给出 `x.com/i/oauth2/authorize` 和 `api.x.com/2/oauth2/token`，均由 `x.com` 后缀覆盖。无需额外添加重复的登录或 API 子域。 |
| [X API 数据字典](https://docs.x.com/x-api/fundamentals/data-dictionary) | 官方示例包含 `api.x.com`、头像 / 图片 `pbs.twimg.com` 和视频 `video.twimg.com`，采用 `x.com`、`twimg.com` 后缀。文档示例是域名用途证据，不是完整 App 网络白名单。 |
| [X 短链接说明](https://help.x.com/en/using-x/url-shortener) | 官方说明帖子与私信中的链接使用 `t.co`。只匹配短链接本身，跳转后的外部目标由其他规则处理。 |
| [X Pro 使用说明](https://help.x.com/en/using-x/how-to-use-x-pro) | 当前产品使用 X Pro 名称；社区中的 `tweetdeck.com` 作为旧入口保留，不据此宣称旧版 TweetDeck 仍运营。 |
| [X 公开首页](https://x.com) | 未登录 GET 返回 HTTP 200；HTML 引用 `abs.twimg.com`、`pbs.twimg.com`、`video.twimg.com`、`ton.twimg.com`、`cdn.syndication.twimg.com`、`api.x.com`、`t.co`。所有这些主机已由三个后缀覆盖。页面链接到 `grok.com` 不代表应将它归入 Twitter 策略。 |
| [X Chat 说明](https://help.x.com/en/using-x/about-chat) | 官方确认存在 Chat 与独立 App，但未在该说明中提供完整网络域名白名单；未将普通网页域名覆盖描述成 Chat / 通话完整支持。 |

## 固定社区来源

- [v2fly/domain-list-community：`dd4e3c0`](https://github.com/v2fly/domain-list-community/blob/dd4e3c080cee602512768e8109f89fafaa94b8ca/data/twitter)：读取调查时 `master` 的 `data/twitter`。
- [blackmatrix7/ios_rule_script：`d8363ca`](https://github.com/blackmatrix7/ios_rule_script/blob/d8363caa2c1e47fb735615316032bed3f475ee57/rule/Surge/Twitter/Twitter.list)：读取调查时 `master` 的 Surge Twitter 清单。文件自身更新时间为 2025-09-28，不能把本次调查日期当作其中每个域名的新验证日期。

以上两个清单可能共享上游数据，不视为两个独立的实机验证。此次规则是按服务范围挑选的域名集合，没有整体复制任一来源。原计划核查的 dler-io `main` 下 Twitter Provider 地址返回 404，因此不将该路径作为 X 域名来源。

| 纳入分组 | 域名 | 依据及边界 |
| --- | --- | --- |
| 当前与旧主站 | `x.com`、`twitter.com` | 官方当前入口及两个社区清单；包含各自的登录、API、帮助和开发者子域，保留旧域名兼容路径。 |
| 短链接与旧入口 | `t.co`、`tweetdeck.com`、`twttr.com` | `t.co` 有官方说明；其余为两个社区清单中的旧入口，不保证每个根域名都提供可访问网页。 |
| 媒体与静态资源 | `twimg.com` | 官方数据字典和公开首页证据；覆盖图片、视频、静态资源与嵌入资源子域。 |
| 直播 / 历史视频基础设施 | `periscope.tv`、`pscp.tv`、`twvid.com` | 两个社区清单均收录，保留相关域名匹配，不宣称旧 Periscope 产品仍在运营，也不保证直播完整覆盖。 |
| 辅助 / 历史基础设施 | `twitteroauth.com`、`twitterstat.us`、`twtrdns.net`、`twttr.net`、`cms-twdigitalassets.com` | 两个社区清单均收录；未找到公开官方完整用途清单，不能仅凭名称认定当前具体功能。DNS 基础设施域名的匹配也不等于代理了系统全部 DNS 流量。 |
| 特定 CDN 主机 | `twitter.map.fastly.net` | v2fly 收录；采用精确 `DOMAIN`，不扩展到整个 `fastly.net`。未据此证明客户端一定直接请求该 CNAME 主机。 |

未纳入：

- `grok.com`、`x.ai`、Cursor 等独立 AI 服务域名：保持现有 Grok 策略划分。
- `DOMAIN-KEYWORD,twitter`、整个 CDN / 云服务商后缀、ASN 与历史 IP 网段：本次没有足够依据将这些广泛匹配加入 X 服务列表。
- `ads-twitter.com`、`tellapart.com`：广告 / 营销范围不作为本次核心社交访问集合；这也不使列表成为广告拦截规则。
- `twimg.co`、`twimg.org`：仅在所查 blackmatrix7 清单出现，未找到当前官方媒体用途依据。
- `twitter.biz`、`twitter.jp`、`twittercommunity.com`、`twitterflightschool.com`、`twitterinc.com`：企业、社区、培训或地区旧入口，不扩展本次主要访问范围。
- `twitpic.com`、`vine.co`：历史服务，未找到本次核心 X 网络功能依赖的证据。
- 第三方镜像、帖子修复 / 预览服务、验证码、外部身份提供商和链接目标：不因为出现在 X 使用流程中就整体归入 Twitter 策略。

## 优先顺序与共享域名

同时使用时，将 `Grok.list` 放在 `Twitter.list` 前面。`Grok.list` 的 `DOMAIN,grok.x.com` 应先命中，其他 `x.com` 请求交给 Twitter。`Twitter.list` 自身不包含 `grok.com` 或 `x.ai`。

X 内置 Grok 使用共享 API / CDN 时，单靠主机名无法识别其用途；这部分仍会随 Twitter 策略。若登录或功能要求一致出口，可手动将两个策略组选为同一节点。不要将整个 `x.com` 加入 Grok 列表来解决共享流量问题。

客户端引用格式沿用仓库现有风格：Surge / Shadowrocket 使用 `[Rule]` 内 `RULE-SET`；Loon 使用 `[Remote Rule]` 内 URL、`policy=`、`tag=` 和 `enabled=true`。参照 [Surge RULE-SET](https://manual.nssurge.com/rules/ruleset.html)、[域名匹配语义](https://manual.nssurge.com/rules/domain.html)、[Loon 示例](https://github.com/Loon0x00/LoonExampleConfig/blob/master/example.conf)与[规则优先级](https://nsloon.app/docs/Rule/)。Loon 本地 / 插件规则仍可能先于订阅命中。

## 验证与限制

- 静态检查 UTF-8、字段、域名格式、重复规则、单文件后缀冗余；核对全部 15 个域名均在固定 v2fly 来源中。
- 检查当前 / 旧主站、API、登录、图片、视频、短链接、直播基础设施及 CDN 主机的匹配；检查相似第三方域名不被误匹配。
- 检查 Grok 优先时 `grok.x.com` 命中 Grok，其他 X 域名命中 Twitter，且独立 Grok / xAI / Cursor 域名不被 Twitter 列表捕获。
- README 中订阅 URL 与仓库文件路径一致，并提供两种客户端语法。
- 公开页面获取与文档核查未使用用户账号；未执行登录、发帖、私信、API 付费调用或修改账号设置。
- 未进行 Surge / Loon / Shadowrocket 实机导入、App 抓包或登录后图片 / 视频 / Chat / Spaces / 通话测试；静态检查无法证明全部网络请求均被覆盖。后续补充应以客户端域名、命中规则、策略与最终节点日志为依据。
