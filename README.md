# Issue Tracker Contract Orbit

这是安装到代码仓库内的 issue tracker 契约 orbit。

它定义该仓库的 issue 存放位置、状态表示、issue sections、review artifact、validation 和安全规则。它不负责跨项目调度，也不负责实现代码。

```text
AGENTS.md
BOOTSTRAP.md
HUMANS.md
README.md
docs/issue-tracker-orbit/
```

核心入口：

```text
docs/issue-tracker-orbit/tracker-contract.md
```

Bootstrap 按 `BOOTSTRAP.md` 自动解析 tracker backend。

选择完成后，**Contract Consumer** 只读取 `tracker-contract.md`，不从模板目录猜测当前仓库使用哪个 backend。
