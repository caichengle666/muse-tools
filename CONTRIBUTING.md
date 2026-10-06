# Contributing

本仓库是 Muse 工具的统一目录，不接收工具源码。

## 新增工具

1. 工具必须是独立仓库，并保持自己的版本、测试和发布流程。
2. 仓库根目录必须有有效的 `SKILL.md`，其中说明适用场景、运行要求、外部影响和边界。
3. 在本仓库 `references/catalog.md` 中新增条目，至少包含：`repository`、`skill`、`choose when`、`do not choose when`、`requirements`、`external effects`、`works with`、`important boundary`。
4. 在 `README.md` 的工具表格中新增一行。
5. 提交前确认仓库地址可访问、默认分支存在，并且没有把源码或密钥复制进本仓库。

## 维护

- 工具行为变化时，优先更新对应仓库；本仓库只同步选择信息。
- 移除工具时同时删除 `README.md` 和 `references/catalog.md` 中的条目。
- 不使用 Git submodule。
