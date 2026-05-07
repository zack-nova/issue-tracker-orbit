# Issue Tracker Contract Orbit 文档索引

本 orbit 安装到代码仓库后，定义该仓库的 issue tracker 契约。

## Runtime 入口

`tracker-contract.md` 是 **Contract Consumer** 的固定读取入口。

**Contract Consumer** 是任何必须读取并遵守本仓库 issues tracker contract 的外部 Agent、运行参与者或工具。

如果 `tracker-contract.md` 仍是 `pending-bootstrap`，先执行 `BOOTSTRAP.md`。正常运行时，不要从 adapter 模板猜测当前仓库使用哪个 issue tracker。

## Core

`core/` 定义平台无关的契约语言：

1. `core/01-core-model.md`：issue tracker、backend、状态、类型、metadata、section、review artifact 的边界。
2. `core/02-state-machine.md`：canonical state roles 与 gate。
3. `core/03-issue-sections.md`：Dev Brief、Dev Workpad、Debt Notes、Review Sweep、Human Review Decision 等 issue sections。
4. `core/04-safety-rules.md`：dry-run、hard stop、禁止事项。

## Adapters

`adapters/` 定义 core 到具体 backend 的映射模板：

- `adapters/github.md`
- `adapters/gitlab.md`
- `adapters/local-markdown.md`
- `adapters/other.md`

Bootstrap 自动解析一个 backend adapter，并生成当前仓库的 `tracker-contract.md`。真实项目已有 GitHub 或 GitLab tracker 时，优先使用已有 tracker；local markdown 只用于没有 hosted tracker 证据或项目明确要求本地管理的仓库。

## Templates

`templates/` 保存 backend 或 issue section 的参考模板。Backend 初始化模板位于 `templates/backends/<backend>/`。Bootstrap 可以按需安装、合并或复制这些模板，但正常运行以 `tracker-contract.md` 为准。
