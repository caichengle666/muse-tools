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

## Combined Use

- Cookie Sync + Tunnel：先部署 Cookie 接收端，再按需用 Tunnel 给它一个公网 HTTPS 地址；同步仍需用户主动触发。
- File Manager Monitor + Tunnel：用 Monitor 查看文件和服务状态，用 Tunnel 暴露需要公网访问的服务。
- 用户只需要其中一个能力时，不安装另外两个工具；组合使用不代表合并仓库。
