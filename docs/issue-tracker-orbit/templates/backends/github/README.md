# GitHub Backend Templates

这些文件是 GitHub backend 的 issue / PR 模板参考，不直接安装到仓库根目录。

Bootstrap 时将它们安全安装或合并到：

```text
.github/ISSUE_TEMPLATE/
.github/pull_request_template.md
```

如果目标文件已存在，必须展示 diff 并等待人类维护者确认。

这些模板使用 GitHub label 表示 core 中的 canonical state role 和 issue type：

```text
state:<role>
type:<type>
```

`out-of-scope-catalog.yml` is a special catalog issue template. It creates an issue with `state:cancelled` and `type:out-of-scope` and uses the issue body for `## Out-of-Scope Catalog` instead of delivery sections. Install the template during bootstrap, but create the actual catalog issue lazily when the maintainer enables the catalog or the first out-of-scope decision needs to be recorded.
