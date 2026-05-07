# BOOTSTRAP.md - Issue Tracker Contract Orbit

本 orbit 已安装在当前代码仓库中。Bootstrap 的目标是选择 issue tracker backend，并生成可运行的 `tracker-contract.md`。

Bootstrap 不创建开发 worktree，不实现代码，不创建业务 review artifact，不 merge。

## 1. 确认仓库上下文

- 确认当前目录是目标代码仓库根目录。
- 执行 `git remote -v`，判断是否明显对应 GitHub 或 GitLab。
- 执行 `git branch --show-current` 与 `git status --short`，确认当前分支和工作区状态。
- 读取 `docs/issue-tracker-orbit/INDEX.md`。
- 读取 `docs/issue-tracker-orbit/tracker-contract.md`。

## 2. 自动解析 tracker backend

解析结果必须是一个 backend：

```text
github
gitlab
local-markdown
other
```

自动选择规则：

- 如果 `tracker-contract.md` 已经是 `status: active`，保留其中的 `backend`，不要重新选择。
- 如果 git remote、`.git/config` 或已有 tracker 模板明确指向 GitHub，选择 `github`。
- 如果 git remote、`.git/config` 或已有 tracker 模板明确指向 GitLab，选择 `gitlab`。
- 如果 GitHub 和 GitLab 证据同时存在且无法判断当前项目事实源，停止并请求人类维护者选择。
- 如果项目明确要求 Jira、Linear 或自定义系统，选择 `other`，并要求人类提供事实表示、review artifact 和安全规则。
- 只有在没有 GitHub/GitLab/other tracker 证据，或项目明确要求本地管理时，才选择 `local-markdown`。

不要因为本 orbit 带有 local markdown 模板就默认选择 `local-markdown`。

读取对应 adapter：

```text
docs/issue-tracker-orbit/adapters/github.md
docs/issue-tracker-orbit/adapters/gitlab.md
docs/issue-tracker-orbit/adapters/local-markdown.md
docs/issue-tracker-orbit/adapters/other.md
```

## 3. 生成 tracker contract

编辑：

```text
docs/issue-tracker-orbit/tracker-contract.md
```

必须完成：

- 将 `status` 改为 `active`。
- 填写 `backend`。
- 填写 repository id 与 default branch。
- 在 `issue` 中填写 issue id/path/URL、state、type、metadata 的 backend 表示。
- 填写 canonical issue sections 的 storage 和 heading。
- 在 `review_artifact` 中保留 `required: true`，并填写所选 backend 的 review artifact storage、link rule 或 path pattern。
- 不要新增 `backend_mapping`；把机器事实直接写入 `issue`、`sections`、`review_artifact`，并只在有真实值时添加 backend-specific 字段。
- 配置 validation commands。
- 配置 land/merge policy，除非所选 backend 没有对应 policy。

若项目暂时没有自动验证命令，必须写明 waiver，并将 `required_before_review` / `required_before_land` 设为符合现实的值。

## 4. 安装或合并 backend 模板

模板来源：

```text
docs/issue-tracker-orbit/templates/backends/<backend>/
```

GitHub backend 常见目标：

```text
.github/ISSUE_TEMPLATE/
.github/pull_request_template.md
```

Local markdown backend 常见目标：

```text
.scratch/issues/
.scratch/reviews/
```

执行规则：

- 如果目标文件不存在，可以创建。
- 如果目标文件已存在，必须先展示 diff 和合并计划。
- 不得静默覆盖已有 tracker 模板、CODEOWNERS 或其他项目配置。
- 新 issue 应默认进入 canonical state role `needs-triage`，并带有合适 issue type。

## 5. 同步 backend metadata

对支持 label 的 backend，先生成 dry-run：

```text
create: ...
update: ...
unchanged: ...
conflict: ...
```

经人类维护者确认后，再同步。

GitHub / GitLab 默认 label：

```text
state:needs-triage
state:needs-info
state:ready-for-dev
state:in-progress
state:in-review
state:human-review
state:to-rework
state:to-merge
state:merged

type:bug
type:feature
type:task
type:docs
type:chore
```

Local markdown backend 不需要创建 labels，但必须确认 frontmatter 字段与 tracker contract 一致。

## 6. 检查运行环境

确认：

- `git` 可用。
- backend 可访问，或 local markdown 路径可写。
- 当前仓库能读取 default branch。
- validation commands 能运行，或已有明确 waiver。
- 外部 **Contract Consumer** 后续能读取本仓库的 `docs/issue-tracker-orbit/tracker-contract.md`。

## 7. 验证初始化结果

完成后报告：

- tracker contract 是否 active。
- tracker contract YAML 字段是否完整。
- backend templates 是否已安装或已合并。
- backend metadata 是否已同步。
- validation 配置是否可运行。
- 是否存在需要人类处理的冲突或未决事项。

## 禁止事项

- 不要创建 worktree。
- 不要启动外部开发运行环境。
- 不要创建业务 review artifact。
- 不要修改业务代码。
- 不要 merge。
- 不要把 review output 当成人类 review decision。
