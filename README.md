# muse-tools

Muse 工具统一入口。这个仓库只负责让 AI 根据需求选择并获取工具，各项目源码继续在独立仓库维护。

把下面地址交给支持 skill 的 AI 即可：

`https://github.com/caichengle666/muse-tools/blob/main/SKILL.md`

当前目录包含：

- `muse-file-manager-monitor`：文件管理、系统监控、网页终端和 AI Agent。
- `muse-tunnel`：Muse 沙盒中的 Cloudflare Tunnel、常驻和自动恢复。
- `muse-cookies-sync`：自己的设备之间上传和恢复浏览器 Cookie。

AI 会先说明匹配的工具；用户选定后，再从独立仓库获取最新版并读取该项目自己的 `SKILL.md`。

