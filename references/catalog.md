# Muse Tool Catalog

> 本目录只做选择和获取入口。运行任何工具前，必须读取目标仓库最新的 `SKILL.md`，并以该文件为准。

## file-manager-monitor

- repository: `https://github.com/caichengle666/muse-file-manager-monitor`
- skill: `file-manager-monitor`
- choose when: 用户需要在 Muse 中管理文件、查看系统监控、使用网页终端或运行 AI Agent，或要复刻、二次开发该构件。
- do not choose when: 用户只需要公网暴露服务或跨设备 Cookie；或目标环境不是 Muse 构件平台。
- requirements: React 19 + Bun 构建；依赖 Muse 构件平台接口；需要在 Muse 中部署构件。
- external effects: 部署后可在网页中读写沙盒文件、查看进程和系统状态，并可执行终端命令、调用 AI Agent。
- works with: 与 `tunnel` 组合可把网页服务暴露到公网；与 Cookie 同步无直接依赖。
- important boundary: 网页终端和 AI Agent 具备执行命令的能力，仅适合用户控制的私有环境，不要暴露给不可信用户。

## tunnel

- repository: `https://github.com/caichengle666/muse-tunnel`
- skill: `cf-tunnel-bridge`
- choose when: 用户需要在 Muse 式沙盒中通过 Cloudflare Tunnel 暴露本地网站、API、WebSocket 或 webhook，并希望重启后自动恢复。
- do not choose when: 目标机器是普通 VPS 或已有可用公网入口，可直接使用官方 cloudflared；或用户不想使用 Cloudflare。
- requirements: Python CLI + cloudflared + systemd；Cloudflare 账号、域名和凭据；Muse 式受限沙盒网络。
- external effects: 创建或修改 Cloudflare Tunnel 与 DNS 记录，写入 systemd 服务，并长期运行网络进程。
- works with: 为 `cookies-sync` 接收端提供公网 HTTPS 地址；与 `file-manager-monitor` 组合可暴露网页服务。
- important boundary: 这是针对受限 Muse 沙盒网络的专用方案；普通 VPS 通常直接使用官方 cloudflared。创建隧道和 DNS 前必须由用户选择域名并确认凭据范围。

## cookies-sync

- repository: `https://github.com/caichengle666/muse-cookies-sync`
- skill: `muse-cookies-sync`
- choose when: 用户需要在自己的设备之间上传、保存、拉取和恢复当前网站 Cookie，或部署对应的自建接收端。
- do not choose when: 用户需要同步密码、书签、历史记录或其他非 Cookie 数据；或要把 Cookie 提供给第三方。
- requirements: Tampermonkey/Violentmonkey 用户脚本 + Node.js 接收端；自建服务器或可访问的 HTTPS 地址；用户自己的浏览器和账号。
- external effects: 用户脚本读取当前网站 Cookie 并上传到用户指定的接收端；恢复时写入浏览器 Cookie；接收端保存这些 Cookie。
- works with: 可用 `tunnel` 暴露自建接收端；与 `file-manager-monitor` 无直接依赖。
- important boundary: 只处理用户自己的浏览器、账号和服务器。Cookie 属于登录凭据；恢复动作必须由用户主动触发，并且部分网站会因 IP、设备指纹或其他本地状态而拒绝复用。

## wecom-bridge

- repository: `https://github.com/caichengle666/muse-wecom-bridge`
- skill: `muse-wecom-bridge`
- choose when: 用户想在企业微信里直接跟 Muse 对话（含文本、语音、图片、文件、视频），或需要 Muse 主动推送消息/卡片到企业微信。
- do not choose when: 用户想接管微信个人号（这是企业微信智能机器人方案，不是个人微信）；或只需要公网暴露服务、同步 Cookie。
- requirements: Node.js 18+；企业微信管理后台创建智能机器人（API 模式 + 长连接）并获取 BotID/Secret；配置 `allowedUserIds` 白名单。
- external effects: 本机向 `wss://openws.work.weixin.qq.com` 建长连接；收到的媒体文件下载到本地；同一 BotID 只允许一条连接。
- works with: 可用 `tunnel` 给配套的 AI 后端提供公网地址；与 `file-manager-monitor` 无直接依赖。
- important boundary: 白名单 fail-closed（为空拒绝启动）；`secrets.env` 绝不提交；外发文件仅限 `outgoing/` 与 `/tmp`；语音用企业微信自带转写。

## feishu-bridge

- repository: `https://github.com/caichengle666/muse-feishu-bridge`
- skill: `muse-feishu-bridge`
- choose when: 用户想在飞书里直接跟 Muse 对话（含文本、图片、文件、视频），或需要 Muse 主动推送消息/卡片到飞书；或需要在飞书群聊里 @ 机器人问答。
- do not choose when: 用户想接管微信个人号或用企业微信（这是飞书企业自建应用方案）；或只需要公网暴露服务、同步 Cookie。
- requirements: Node.js 18+；飞书开放平台创建企业自建应用（机器人能力 + 长连接接收事件，订阅 `im.message.receive_v1`）并获取 App ID/Secret；配置 `allowedOpenIds` 白名单。
- external effects: 本机向飞书建 WebSocket 长连接收事件；收到的媒体文件下载到本地；同一应用只应有一条长连接。
- works with: AI 后端由部署者自接 `inbox/`/`outbox/` 文件队列；与 `file-manager-monitor` 无直接依赖。
- important boundary: 白名单 fail-closed（为空拒绝启动）；`secrets.env` 绝不提交；外发文件仅限 `outgoing/` 与 `/tmp`；群聊默认只响应 @；卡片原地更新需要应用开通"更新应用发送的消息"权限。

## auto-approve

- repository: `https://github.com/caichengle666/muse-auto-approve`
- skill: `muse-auto-approve`
- choose when: 用户想让 muse.ai 里 Agent 的外联审批自动通过，不再手动点审批卡片；或希望新域名首次访问就自动永久放行。
- do not choose when: 用户希望每次外联都经过人眼确认（那就别装这个）；或用的不是 muse.ai。
- requirements: Node.js 20.18+；一个 muse.ai 账号（邮箱 + 密码）；出网能访问 muse.ai。
- external effects: 自动登录 muse.ai（服务端会发一封 OTP 邮件，无需读码）；每 10 秒轮询审批并自动 `allow_always`；每 5 分钟续期会话；会在账号下落成 durable 永久放行规则。
- works with: 与其他工具无直接依赖。
- important boundary: 高权限工具——新域名的首次外联以后不再经人眼，只用在自己的账号和机器上；`data/`（凭据/会话）绝不提交；逆向协议，官方改版可能失效。

## Combined Use

- Cookie Sync + Tunnel：先部署 Cookie 接收端，再按需用 Tunnel 给它一个公网 HTTPS 地址；同步仍需用户主动触发。
- File Manager Monitor + Tunnel：用 Monitor 查看文件和服务状态，用 Tunnel 暴露需要公网访问的服务。
- 用户只需要其中一个能力时，不安装另外两个工具；组合使用不代表合并仓库。
