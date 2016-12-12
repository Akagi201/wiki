# GIT 最佳实践

## 使用 github flow
* gitflow 太麻烦了, 而且对于几个人开发的小项目, 完全没有必要. 而且, 大多数的开源项目, 都是使用 github flow.
* 介绍文章: [English](https://guides.github.com/introduction/flow/) [中文](http://gitbeijing.com/flow/)
* 基于 master 创建分支.
* 分为 4 个阶段: 创建分支, 提交 commit, 提交 PR, Code Review, 部署(我们方式是这里是测试, 部署放到最后.), 合并到 master.

## Commit Message
* PR 中使用中文进行描述.
* commit 跟代码注释使用英文和中文均可.

### Commit Message Style

遵循 [Angular’s commit message convention](https://github.com/angular/angular/blob/master/CONTRIBUTING.md#-commit-message-guidelines)

[详细文档](https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit)

辅助工具 [commitizen/cz-cli](https://github.com/commitizen/cz-cli)

好处:

* 自动生成 `CHANGELOG.md`.
* 识别不重要的提交.
* 为浏览提交历史时提供更好的信息.

```
<type>(<scope>): <subject>
<BLANK LINE>
<body>
<BLANK LINE>
<footer>
```

* `type`: 必填, 定义了此次变更的类型, 只能使用下面几种类型.
  - `feat`: 增加新功能.
  - `fix`: 问题修复.
  - `docs`: 文档变更.
  - `style`: 代码风格变更(不影响功能).
  - `refactor`: 既不是新功能也不是问题修复的代码变更.
  - `perf`: 改善性能.
  - `test`: 增加测试.
  - `build`: 与构建系统或者依赖相关.(如 scope: Makefile, gulp, npm)
  - `ci`: CI 配置文件或者脚本.
  - `chore`: 其他修改, 不涉及 `src`, `test` 的修改.
* `scope`: 可选, 改动范围, 如: 模块名.
* `subject`: 必填, 是对变更的简要描述.
* `body`: 可选, 是更为详细的描述.
* `footer`: 可选, 包含Breaking Changes, 以 `BREAKING CHANGE:`开头, 或者 Closes issue, [closing reference to an issue](https://help.github.com/articles/closing-issues-via-commit-messages/).

## Commitizen

### 提交代码
* `git cz`

### 检查提交记录是否符合约定
* [validate-commit-msg](https://github.com/kentcdodds/validate-commit-msg)

### 生成 CHANGELOG.md
* [conventional-changelog-cli](https://github.com/conventional-changelog/conventional-changelog-cli)

## 参考项目
* [Angular](https://github.com/angular/angular/commits/master)

## Refs
* [Git 提交记录和分支模型](https://cattail.me/tech/2016/06/06/git-commit-message-and-branching-model.html)
