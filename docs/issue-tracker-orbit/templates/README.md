# 模板

本目录保存 backend 初始化和 issue section 使用的模板参考。

## Backend 模板

`backends/` 下按 backend 保存初始化模板。Bootstrap 选择 backend 后，只读取对应子目录。

```text
backends/github/
backends/local-markdown/
```

`backends/github/` 下的文件用于选择 GitHub backend 时安全安装或合并到真实 `.github/` 目录。

`backends/local-markdown/` 下的文件用于本地 markdown backend 创建 issue 文件和 local review artifact。

## Issue Section 模板

`issue-sections/` 下的文件用于具备相应职责的 **Contract Consumer** 在 issue tracker 中创建或更新 canonical issue sections。

可选 issue section（例如 `debt-notes`）只在确实有内容需要记录时创建。`human-review-decision` 也是条件性 section：只有 issue 进入 `human-review`，或 delivery mode 为 `hitl` 且需要 post-review human decision 时才创建。
