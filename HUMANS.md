# HUMANS.md - Issue Tracker Contract Orbit

这份说明给项目维护者日常使用。它不是安装清单，也不是 orbit 内部维护手册。

## 你需要知道什么

- 当前仓库的 issue workflow 规则在 `docs/issue-tracker-orbit/tracker-contract.md`。
- Issue 可以存在 GitHub、GitLab、本地 markdown 或其他 tracker 中，具体以后端契约为准。
- **Contract Consumer** 是必须先读取并遵守 tracker contract 的运行参与者身份。任何要参与 issue workflow 的运行参与者都必须先作为 **Contract Consumer**。
- **Contract Consumer** 身份本身不授予 triage、开发、创建 review artifact、rework、land 或 merge 权限；具体权限由外部 orbit、运行时参与者、工具或人类流程决定。
- 人类维护者负责澄清需求、批准 waiver、处理冲突，以及写出最终 review decision。

## 什么时候需要你参与

### Triage

当 issue 信息不足时，具备 triage 职责的 **Contract Consumer** 会让 issue 保持在 `needs-info`，并记录 Triage Notes。你需要补充缺失事实或决定取消、拆分、降级。

当 issue 太大、不能作为一个安全开发单元推进时，把它移到 `needs-split`。拆分原因和预期拆分方向应记录在 Triage Notes 或 Dev Brief；解除该状态前，需要记录拆出的更小 issue，或由人类维护者把原 issue 收窄到可继续推进的范围。

### Ready for Dev

Issue 进入 `ready-for-dev` 前必须有完整 Dev Brief。你需要确认：

- 需求表达清楚，
- acceptance criteria 可验证，
- validation plan 真实可执行，
- out-of-scope 明确，
- issue 当前不处于 `blocked`。

### Blocked

`blocked` 是状态。它表示当前 issue 被依赖、外部因素或人类决策阻塞，暂时不能安全推进。

### Duplicate / Cancelled

`duplicate` 表示该 issue 已由另一个 issue 承接，必须记录 superseding issue。`cancelled` 表示维护者决定不再交付该 issue，必须记录取消原因。

这两个都是终止状态。

### Debt Notes

当开发或 review 中发现不阻塞当前交付的技术债，可以按需创建 `## Debt Notes`。它不是隐藏 backlog，也不是 merge blocker。若债务后来被提升为独立 issue，更新原条目的 `Follow-up issue`；若债务会影响当前交付，必须进入 review evidence，并由 Human Review Decision 决定。

### Review

Issue 进入 `in-review` 后，review artifact 已存在。它通常是 PR/MR；local markdown backend 会使用本地 review artifact 文件模拟 review gate。Review Sweep 写入后，issue 进入 `human-review`，等待人类维护者写 Human Review Decision。

Review Sweep 只是观察，不是决定。

### Human Review Decision

当 review 信息足够后，人类维护者需要写 Human Review Decision：

```text
Decision: hold
Decision: rework
Decision: merge
```

- `hold`：继续留在 `human-review`。
- `rework`：允许 issue 进入 `to-rework`，由具备 rework 职责的 **Contract Consumer** 接管。
- `merge`：允许 issue 进入 `to-merge`，由具备 land 职责的 **Contract Consumer** 执行 Land；Land 成功后 issue 进入 `merged`。

不要让 review output 替代 Human Review Decision。

`Human Review Decision` 是授权证据，不是操作触发器；操作触发看 issue 的 canonical state role。Contract Consumer 需要用自己的 claim/running/retry 机制避免重复接管，tracker contract 不记录这些运行时占用事实。

`to-rework` 完成后必须回到 `in-review`，重新产生 Review Sweep。`to-merge` 执行失败时回到 `human-review`，只有人类确认需要改代码时才进入 `to-rework`。

## 不要手动绕过

- 不要直接在默认分支开发。
- 不要本地 merge 默认分支。
- 不要绕过 tracker contract 修改状态。
- 不要把外部运行 session 或 worktree notes 当成事实源。
- 不要在没有 Human Review Decision 的情况下要求任何运行环境 merge。
