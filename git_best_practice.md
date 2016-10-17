
## 使用 github flow
* 介绍文章: [English](https://guides.github.com/introduction/flow/) [中文](http://gitbeijing.com/flow/)
* 基于 master 创建分支.
* 分为 4 个阶段: 创建分支, 提交 commit, 提交 PR, Code Review, 部署, 合并到 master.

## Commit Message
* 必须用英文描述.
* PR 中可以使用中文进行描述.
* 代码注释必须使用英文.

### Commit 详细格式

```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

* `type` 定义了此次变更的类型, 只能使用下面几种类型.
  - feat: 增加新功能
  - fix: 问题修复
  - docs: 文档变更
  - style: 代码风格变更(不影响功能)
  - refactor: 既不是新功能也不是问题修复的代码变更
  - perf: 改善性能
  - test: 增加测试
  - chore: 开发工具(构建, 脚手架工具等)
* subject 是对变更的简要描述.
* footer: 可以包含 Breaking Changes 和 Closes 信息.
* body: 是更为详细的描述.
