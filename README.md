# muse-tools

Muse 工具统一入口。这个仓库只负责让 AI 根据需求选择并获取工具，各项目源码继续在独立仓库维护，方便单独发布、排错和升级。

## 给 AI 使用

把下面地址交给支持 skill 的 AI：

`https://github.com/caichengle666/muse-tools/blob/main/SKILL.md`

也可以直接描述目标，例如：

- “我想在 Muse 里管理文件和查看系统状态。”
- “我想把自己电脑的 Cookie 同步到另一台设备。”
- “我要把沙盒里的服务通过 Cloudflare 暴露出来。”

AI 会先读取目录，说明匹配的工具、运行要求和外部影响；等用户选定后，再从独立仓库获取最新版并读取该项目自己的 `SKILL.md`。

## 当前工具

| 工具 | 用途 | 仓库 |
| --- | --- | --- |
| muse-file-manager-monitor | 文件管理、系统监控、网页终端和 AI Agent | https://github.com/caichengle666/muse-file-manager-monitor |
| muse-tunnel | Muse 沙盒中的 Cloudflare Tunnel、常驻和自动恢复 | https://github.com/caichengle666/muse-tunnel |
| muse-cookies-sync | 自己的设备之间上传和恢复浏览器 Cookie | https://github.com/caichengle666/muse-cookies-sync |
| muse-wecom-bridge | 企业微信智能机器人 × Muse 双向桥接，在企业微信里聊天、收发文件、卡片交互、主动推送 | https://github.com/caichengle666/muse-wecom-bridge |
| muse-feishu-bridge | 飞书企业自建应用 × Muse 双向桥接，在飞书里聊天、收发文件、卡片回复、主动推送 | https://github.com/caichengle666/muse-feishu-bridge |

详细的选择条件、运行要求、外部影响和组合方式见 `references/catalog.md`。

## 设计原则

- 每个工具保持独立仓库、独立版本和独立 `SKILL.md`。
- 本仓库不复制源码、不引入 Git submodule，只维护目录和路由说明。
- 工具安装、部署、创建隧道或处理 Cookie 前，必须由用户明确选择并授权。
- 新增工具时同时更新 `README.md` 和 `references/catalog.md`，并在目标仓库根目录提供有效 `SKILL.md`。

## 新增工具

见 `CONTRIBUTING.md`。

## License

MIT，见 `LICENSE`。
