# Issue Tracker Contract Workflow

Issue Tracker Contract Workflow 用于在需要时管理 issue-driven delivery 的 tracker contract 和状态安全规则。

## 按需读取

- 处理 issue state、metadata、issue sections、review artifact、模板、tracker backend 或安全规则时，先读 `docs/issue-tracker-orbit/tracker-contract.md`。如果它仍是 `pending-bootstrap`，先执行 `BOOTSTRAP.md`。
- 读取 tracker contract 后，再按需读取 `docs/issue-tracker-orbit/INDEX.md`、core 文档或 backend adapter 文档。
- 使用 `triage` skill 时，由 skill 和 tracker contract 承担具体状态机流程。

## 常驻边界

- 不要假设 issue 在 GitHub；必须使用 tracker contract 里选择的 backend 和事实映射。
- 不得在默认分支直接开发，不得本地直接 merge 默认分支。
- 发生规则冲突、模板冲突、状态冲突或事实源不一致时，停止推进并请求人类维护者决策。
