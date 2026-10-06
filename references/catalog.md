# Muse Tool Catalog

## file-manager-monitor

- repository: `https://github.com/caichengle666/muse-file-manager-monitor`
- skill: `file-manager-monitor`
- choose when: 用户需要 Muse 中的文件管理、系统监控、网页终端、AI Agent，或要复刻和二次开发该构件。
- runtime: React 19 + Bun，依赖 Muse 构件平台接口。
- important boundary: 网页终端和 AI Agent 具备执行命令的能力，仅适合用户控制的私有环境。

## tunnel

- repository: `https://github.com/caichengle666/muse-tunnel`
- skill: `cf-tunnel-bridge`
- choose when: 用户需要在 Muse 式沙盒中通过 Cloudflare Tunnel 暴露本地网站、API、WebSocket 或 webhook，并希望重启后自动恢复。
- runtime: Python CLI + cloudflared + systemd。
- important boundary: 这是针对受限 Muse 沙盒网络的专用方案；普通 VPS 通常直接使用官方 cloudflared。创建隧道和 DNS 前需要用户选择域名并具备 Cloudflare 凭据。

## cookies-sync

- repository: `https://github.com/caichengle666/muse-cookies-sync`
- skill: `muse-cookies-sync`
- choose when: 用户需要在自己的设备之间上传、保存、拉取和恢复当前网站 Cookie，或部署对应的自建接收端。
- runtime: Tampermonkey/Violentmonkey 用户脚本 + Node.js 接收端。
- important boundary: 只处理用户自己的浏览器、账号和服务器。Cookie 属于登录凭据；恢复动作必须由用户主动触发，并且部分网站会因 IP、设备指纹或其他本地状态而拒绝复用。

## Combined Use

- 要给 Cookie 接收端提供公网 HTTPS 地址：先部署 `cookies-sync` 接收端，再按需使用 `tunnel` 暴露服务。
- 要在 Muse 中查看文件、进程和服务状态：使用 `file-manager-monitor`；它不替代 `tunnel` 或 Cookie 客户端。
- 用户只需要其中一个能力时，不安装另外两个工具。

