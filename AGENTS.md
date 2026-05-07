# AGENTS.md - Issue Tracker Contract Orbit

本仓库使用 Issue Tracker Contract Orbit 管理 issue-driven delivery。

## 不可违反规则

- 先读 `docs/issue-tracker-orbit/tracker-contract.md`。如果它仍是 `pending-bootstrap`，先执行 `BOOTSTRAP.md`。
- 不要假设 issue 存在 GitHub。必须使用 tracker contract YAML block 里选择的 backend 和事实映射。
- 每个 open issue 必须且只能有一个 canonical state role。
- Issue 进入 `ready-for-dev` 前，必须有 exactly one issue type 和完整 `Dev Brief`。
- Issue 进入 `in-progress` 前，必须已经处于 canonical state role `ready-for-dev`。
- `Dev Workpad` 是 issue-scoped execution record，不是外部运行 session。
- `Review Sweep` 只记录 **Review Sweep Producer** 的观察，不决定 `rework` 或 `merge`。
- `to-rework` 和 `to-merge` 必须由 `Human Review Decision` 决定。
- `to-rework` 完成后回到 `in-review`；`to-merge` 只有 Land 成功后才能进入 `merged`。
- 不得在默认分支直接开发，不得本地直接 merge 默认分支。
- 发生规则冲突、模板冲突、状态冲突或事实源不一致时，停止推进并请求人类维护者决策。

## 文档入口

- Runtime contract：`docs/issue-tracker-orbit/tracker-contract.md`
- 文档索引：`docs/issue-tracker-orbit/INDEX.md`

处理状态、metadata、issue sections、review artifact、模板或安全规则时，先读取 tracker contract，再按需读取 core 或 adapter 文档。
