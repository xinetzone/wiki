(wiki:epb/contributing)=
# EPB 开发规范

参考：[Development Conventions — The Executable Book Project (executablebooks.org)](https://executablebooks.org/en/latest/contributing.html)

**EPB 开发规范**旨在帮助您尽可能高效地作出贡献，而不是制定必须始终遵守的严格规则。认为这些都是可以遵循的合理模式，EBP 也试图尽可能地遵循它们。但如果你喜欢做其他的事情，这通常是好的👍。

## 行为准则

本项目及其所有参与人员均受 [EBP 行为准则](https://github.com/executablebooks/.github/blob/master/CODE_OF_CONDUCT.md) 管辖。通过参与，你将被期望维护这一准则。请向 executablebooks@gmail.com 报告不可接受的行为。

## 问题或反馈

可执行书籍项目使用一个 [GitHub 讨论板](https://github.com/executablebooks/meta/discussions) 来提供社区问题、讨论和帮助。请在这里加入：<https://github.com/executablebooks/meta/discussions>。

此外，如果你想看到一个新功能的实现，查看 EPB 的 [功能投票](https://executablebooks.org/en/latest/feature-vote.html) 页面。

## EBP 存储库的结构

EBP 是一个大型开源项目；它由许多包组成，以保持单个组件的模块化和可重用性。当您考虑为 EBP 做出贡献时，您可能不确定哪些存储库实现了您想要更改的功能或报告错误。这部分应该可以帮助你。

以下是一些大的列表：

- [markdown-it-py](https://github.com/executablebooks/markdown-it-py) 是 EPB 的 Markdown 解析器。它是非常流行的 [markdown-it](https://github.com/markdown-it/markdown-it) 包的 Python 端口，该包与 CommonMark 兼容、快速且可扩展。
- [MyST-Parser](https://github.com/executablebooks/MyST-Parser) 是 markdown-it-py 和 [sphinx](https://github.com/sphinx-doc/sphinx) 之间的桥梁。它在 Markdown 文件上调用 markdown-it-py，并将创建的解析令牌（tokens）转换为 sphinx 内部使用的 docutils 抽象语法树（Abstact Syntax Tree，简称AST）。
- [MyST-NB](https://github.com/executablebooks/MyST-NB) 构建于 MyST-Parser 之上，允许解析和执行 [Jupyter Notebooks](https://jupyter.org/) 及其[基于文本的表示](https://myst-nb.readthedocs.io/en/latest/use/markdown.html)。
- MyST-NB 使用 [jupyter-cache](https://github.com/executablebooks/jupyter-cache) 来执行笔记本并缓存它们的结果，这样，只有当代码单元发生变化时，它们才会在文档构建期间重新执行。
- [sphinx-book-theme](https://github.com/executablebooks/sphinx-book-theme) 是一个 sphinx HTML 主题，为可执行书籍的表示而设计。
- [sphinx-copybutton](https://github.com/executablebooks/sphinx-copybutton), [sphinx-togglebutton](https://github.com/executablebooks/sphinx-togglebutton), [sphinx-panels](https://github.com/executablebooks/sphinx-panels) 和 [sphinx-thebe](https://github.com/executablebooks/sphinx-thebe) 提供 sphinx 扩展以允许在文档中包含特殊特性。
- [jupyter-book](https://github.com/executablebooks/jupyter-book) 利用上述组件，提供了一个用户友好的界面，以建立美丽的，出版质量的书籍和文件。
- [myst-language-support](https://github.com/executablebooks/myst-language-support) 提供了 aTextmate 语法和 VS Code 扩展，用于编辑 MyST 标记。

下面是适用于所有存储库的约定文档，但单个存储库也可能包含针对特定代码库的额外贡献指南。

## 设计哲学

在做出技术和社区决策时，EPB 项目尝试遵循一些高级原则。他们的目标是要争取的，并且可能不是所有的时间都被完美地遵循。以下是其中一些原则：

文档第一
:    当决定是否需要一个新特性时，首先要考虑改进文档是否可以为其他人解决同样的问题。新特性（尤其是新 API）的开发和维护成本很高。有时候，通过文档向人们展示如何手动完成复杂的事情，比扩展 API 更好。如果需要新的特性/API，首先要确保这条途径已经用完，这样做是有意的。

规范开发实践
:   我们应该在所有的 EBP 知识库中保持开发者/发布/社区实践的一致性。在可能的情况下，在他们之间共享基础架构和文档在所有核心存储库中保持相同的质量控制级别，无论它们有多小。

保持语义文档模型的一致性
:   有一些地方 Markdown 语法/文件结构映射到一本书的结构上。发生这种情况的两个最明显的地方是 Jupyter Book `_toc.yml` 文件和底层的 Sphinx `toctree` 结构。这两个模型应该非常相似。尽量不要包含面向用户的结构（例如，在 `_toc.yml`，必须在 Sphinx 中转换为不同的文档结构）。

尽早并经常发布
:   我们应该强调更小、更迭代的发布，而不是更大、更复杂的发布。这将使我们的文档与最新版本保持一致，并将与大更改相关的中断（以及随后的维护负担）降至最低。[创建一个发布版本的过程](https://executablebooks.org/en/latest/contributing.html#releases-and-change-logs) 是相对简单和快速的，所以发布适当的补丁版本（或小版本）时不要犹豫。

## 编程风格

编码风格在很大程度上是自动执行的，使用 [pre-commit hooks](https://pre-commit.com/)。对于 Python 包，预提交应该包括通过 [Black](https://black.readthedocs.io/) 实现的自动代码格式化和通过 [flake8](https://flake8.pycqa.org/) 实现的代码检测。

## 命名规范

应该使用以下命名约定。

### 指令

指令的名称应该是：

- 简短、简明和描述性
- 使用 `-` 将单词连接在一起

例如，对 `rst` 语法求值的指令可以命名为 `eval-rst`，它将在使用该 [指令语法](https://myst-parser.readthedocs.io/en/latest/using/syntax.html#directives-a-block-level-extension-point) 的文档中使用。

````
```{eval-rst}
<rst>
```
````

## 测试

所有的包都应该包含一个测试基础设施，通常是通过 [GitHub Actions 工作流](https://docs.github.com/en/actions/configuring-and-managing-workflows/configuring-a-workflow) 自动化的 PRs 和提交。

测试哲学：

- 当您遇到一个错误时，为它添加一个 (failing) 测试。然后修复错误。
- 当你修改或添加一个特性时，为它编写一个测试！[在编写实际实现之前](http://en.wikipedia.org/wiki/Test_Driven_Development) 编写测试有助于保持 API 的整洁。
- 使 [单元测试](https://en.wikipedia.org/wiki/Software_testing#Testing_levels) 尽可能的原子化。单元测试应该在眨眼之间运行。
- 记录编写测试的原因：一旦测试失败，开发人员会感谢你的。

对于 Python 包，测试基础设施应该通过 [pytest](https://docs.pytest.org/) 实现。

## 文档

项目的最小描述应该包含在 [README.md](http://readme.md/) 中，那么大多数文档通常应该包含在 `docs` 文件夹中，使用 {term}`Sphinx`（直接或通过 jupyter book）作为文档生成器。

Markdown 风格通常应该遵循 [markdownlint](https://github.com/markdownlint/markdownlint) 中列出的规则，rST 也应该类似地遵循 [rstcheck](https://github.com/myint/rstcheck)，这两者都可以添加到 pre-commit 配置中。

此外，在编写文档时，作者应该遵循以下准则：

- **每行写一个句子**，否则**不需要手动换行**，以方便创建和检查差异。所有标准编辑器都允许动态换行，并且行长与呈现的文档（例如 HTML 或 PDF 格式）无关。
- **文件和目录名应该由字母和数字组成**，并且所有的小写字母和下划线作为单词分隔符。例如：`entry_points.md`。
- **标题必须以句子为单位设置**。例如，“Entry points”。
- 用空行分隔段落，但不要多。
- 对逐项列出的清单使用 `-` 符号。
- 正确地在代码栏/代码块指令前加上前缀以指示它们的用法，例如：

- Bash shell 脚本应该使用 `bash`：

````
```bash
echo "hi"
```
````

- Bash shell 会话（即交互式）应该使用 `console`：

````
```console
$ echo "hi"
```
````

使用 `#` 而不是 `$` 来表示根提示。

- Python 脚本应该使用 `python`：

````
```python
print("hi")
```
````

- Python 会话（例如通过 ipython）应该使用 `ipython`：

````
```ipython
In  [1]: print("hi")
Out [1]: "hi"
```
````

## Git 分支

存储库应该使用 `master`（或者 `main`） 作为它们的主分支。这个分支在 GitHub 上应该是“受保护的”，并且在合并之前需要 PR 审查和状态检查（参见 [GitHub 分支配置](https://docs.github.com/en/github/administering-a-repository/configuring-protected-branches)）。

添加到 `master` 应该遵循以下简单的概念：

- 对所有新特性和 bug 修复使用 features 分支。
- 使用拉请求将特性分支合并到主分支中。
- 保持一个高质量，最新的主分支。

## 打开拉请求

Pull 请求应该尽早和经常提交给 `master`！

为了提高对拉请求“一眼就能看出来”的理解，鼓励使用几个标准化的标题前缀：

- `[BRK]` for changes which break existing builds or tests
- `[DOC]` for new or updated documentation
- `[ENH]` for enhancements
- `[FIX]` for bug fixes
- `[REF]` for refactoring existing code
- `[STY]` for stylistic changes
- `[TST]` for new or updated tests, and

您还可以组合上面的标记，例如，如果您同时更新一个测试和文档：`[TST, DOC]`。

PRs 通常还应该对一个或多个未决问题作出回应。你可以将 pull 请求链接到某个议题，以显示修复正在进行中，并在 pull 请求合并时自动关闭该问题；参见将 [pull 请求链接到一个议题](https://docs.github.com/en/github/managing-your-work-on-github/linking-a-pull-request-to-an-issue)。

如果您的 pull 请求还没有准备好合并，请打开您的 pull 请求作为 draft。更多信息可以在[ GitHub 的文档中找到](https://help.github.com/articles/about-pull-requests/#draft-pull-requests)。这告诉开发团队拉请求是一个“work-in-progress”，并且您计划继续处理它。

当您的 pull 请求为“Ready for Review”时，您可以在 PR 页面上选择此选项，该选项将通知项目维护者审查您提出的更改。

## Pull Request Reviews

### 源

https://github.com/aiidateam/aiida-core/wiki/Best-practises-for-code-review

https://google.github.io/eng-practices/review/reviewer/standard.html

https://www.ibm.com/developerworks/rational/library/11-proven-practices-for-peer-review/index.html

https://phauer.com/2018/code-review-guidelines/

### 批准变更

- 技术事实和数据优于个人偏好
- 如果 PR 确实能够改善代码的整体健康状况，那么就应该批准 PR，即使它并不完美

### 警惕

- 对评审请求做出反应。那些付出努力并做出回馈的用户最值得我们关注，而 PRs 的及时审核便是一大激励因素。
- 查看正在修改的每一行代码。
- 使用清单，特别是如果您是代码审查新手。

### 交流

- 对做得好的事情给予鼓励，不要只是指出错误。
- 可以提到哪些地方需要改进，但不是强制性的（在注释前加上 “Nit:” 代替 “nitpick”）。
- 分享知识有助于提交者提高他们对代码的理解（(明确你希望/不希望在什么地方采取行动）

## Check-list - What to look for

### Scope[¶](https://executablebooks.org/en/latest/contributing.html#scope "Permalink to this headline")

*   Are you being asked to review more than 200 lines of code? Then don’t be shy to ask the submitter to split the PR - review effectiveness [drops substantially beyond 200 lines of code](https://www.ibm.com/developerworks/rational/library/11-proven-practices-for-peer-review/index.html).

*   Are there parts of the codebase that have not been modified, but *should* be adapted to the changes? Does the code change require an update of the documentation?

### Design[¶](https://executablebooks.org/en/latest/contributing.html#design "Permalink to this headline")

*   Does this change belong in the codebase?

*   Is it integrated well?

### Functionality[¶](https://executablebooks.org/en/latest/contributing.html#functionality "Permalink to this headline")

*   Does the code do what the developer intended?

*   Are there edge cases, where it could break?

*   For UI changes: give it a try yourself! (difficult to grasp from reading code)

### Complexity[¶](https://executablebooks.org/en/latest/contributing.html#complexity "Permalink to this headline")

*   Any complex lines, functions, classes that are not easy to understand?

*   Over-engineering: is the code too complex for the problem at hand?

### Tests[¶](https://executablebooks.org/en/latest/contributing.html#tests "Permalink to this headline")

*   Are there tests for new functionality?
    Are bugs covered by a test that breaks if the bug resurfaces?

*   Are the tests correct and useful?
    Do they make simple and useful assertions?

### Naming[¶](https://executablebooks.org/en/latest/contributing.html#naming "Permalink to this headline")

*   A good name is long enough to communicate what the item does, without being so long that it becomes hard to read

### Comments[¶](https://executablebooks.org/en/latest/contributing.html#comments "Permalink to this headline")

*   Do comments explain *why* code exists (rather than *what* it is doing)?

### Style & Consistency[¶](https://executablebooks.org/en/latest/contributing.html#style-consistency "Permalink to this headline")

*   Does the contribution follow generic coding style (mostly enforced automatically)?

## Merging Pull Requests[¶](https://executablebooks.org/en/latest/contributing.html#merging-pull-requests "Permalink to this headline")

A pull request should be opened only once you consider it ready for review. Each time you push a commit to a branch with an open PR, it triggers a CI build, eating up the quota of the organization.

There are three ways of ‘merging’ pull requests on GitHub. **Squash and merge** is the favoured method, applicable to the majority of PRs, but there are some use cases where the other two apply:

*   **Squash and merge**: take all commits, squash them into a single one and put it on top of the base branch.

    *   Choose this by default for pull requests that address a single issue and are well represented by a single commit.

    *   The person merging the PR should choose a [clear commit message](https://executablebooks.org/en/latest/contributing.html#dev-commits) when merging (via the GitHub UI)

*   **Rebase and merge**: take all commits and ‘recreate’ them on top of the base branch. All commits will be recreated with new hashes.

    *   Choose this for pull requests that require more than a single commit.

    *   Make sure [the commits have clear commit messages](https://executablebooks.org/en/latest/contributing.html#dev-commits).

    *   Examples: PRs that contain multiple commits with individually significant changes; PRs that have commits from different authors (squashing commits would remove attribution)

*   **Merge with merge commit**: put all commits as they are on the base branch, with a merge commit on top

    *   Choose for collaborative PRs with many commits. Here, the merge commit provides actual benefits.

One drawback of squash-merging is that it combines multiple commits into a single one. This is usually fine, as PRs have many commits like “fixing typo”, and “addressing comments”. Squashing these into one message allows the PR merger to create [a commit message that is clear and concise](https://executablebooks.org/en/latest/contributing.html#dev-commits). However, sometimes a PR is best-represented by *multiple* commits. In this case, it’s fine to rebase-merge or merge-commit.

> **How can I rename my commits locally?**
> 
> If you’d like to rename commits locally (e.g., if you’d like to make a rebase-commit in GitHub, but wish to clean up the commit history first to use [commit messages that are clear and concise](https://executablebooks.org/en/latest/contributing.html#dev-commits)), you can try an [**interactive rebase**](https://thoughtbot.com/blog/git-interactive-rebase-squash-amend-rewriting-history). This allows you to convert a series of commits into a smaller number of commits, and you can choose the commit message for each one. However, this is an advanced git technique so only do this if you know what you’re doing! If you just want to merge in your commits without interactively rebasing, it is not the end of the world.

## 提交信息

提交信息：

- 应该理想地解决一个问题吗
- 应该是对代码库的自包含更改吗
- 一定不能把不相关的变化混为一谈吗

````
<EMOJI> <KEYWORD>: Summarize changes in 72 characters or less (#<PR number>)

More detailed explanatory text, if necessary.
The blank line separating the summary from the body is
critical (unless you omit the body entirely); various tools like `log`,
`shortlog` and `rebase` can get confused if you run the two together.

Explain the problem that this commit is solving. Focus on why you are
making this change as opposed to how (the code explains that).  Are
there side effects or other unintuitive consequences of this change?
Here's the place to explain them to someone else reading your message in
the future. Make sure to include some necessary context for difficult
concepts that might change in the future.

There is no need to mention you also added unit tests when adding a new feature. The code diff already makes this clear.
````

关键词/表情符号改编自  [Emoji-Log](https://github.com/ahmadawais/Emoji-Log) 和 [gitmoji](https://github.com/carloscuesta/gitmoji)，应该是以下其中之一（括号中包含 [GitHub 表情符号标记](https://gist.github.com/rxaviers/7360908) 供参考）：

- `‼️ BREAKING:` (`:bangbang:`) — to introduce a back-incompatible change(s) (and/or remove deprecated code).
- ✨ `NEW:` (`:sparkles:`) — to introduce a new feature(s).
- 👌 `IMPROVE:` (`:ok_hand:`) — to improve an existing code/feature (with no breaking changes).
- 🐛 `FIX:` (`:bug:`) — to fix a code bug.
- 📚 `DOCS:` (`:books:`) — to add new documentation.
- 🔧 `MAINTAIN:` (`:wrench:`) — to make minor changes (like fixing typos) which should not appear in a changelog.
- 🧪 `TEST:` (`:test_tube:`) — to add additional testing only.
- 🚀 `RELEASE:` (`:rocket:`) — to bump the package version for release.
- ⬆️ `UPGRADE:` (`:arrow_up:`) — for upgrading a dependency pinning.
- ♻️ `REFACTOR:` (`:recycle:`) — for refactoring existing code (with no specific improvements).
- 🗑️ `DEPRECATE:` (`:wastebasket:`) — mark some code as deprecated (for removal in a later release). The future version when it will be removed should also be specified, and (if applicable) what will replace it.
- 🔀` MERGE:` (`:twisted_rightwards_arrows:`) — for a merge commit (then all commits within the merge should be categorised)
- ❓ `OTHER:` (`:question:`) — anything not covered above (use as a last resort!).

这个列表是松散的优先级顺序，例如，一个既修复了 bug 又不兼容的提交应该被归类为 `BREAKING` 而不是 `FIX`。

## 发布和变更日志

发布应该通过 [GitHub releases](https://docs.github.com/en/github/administering-a-repository/managing-releases-in-a-repository)，从主分支和使用语义版本标记，例如 `v1.2.1`，对于 `1.0.0` 以上的版本。对于低于 `1.0.0` 的版本，可以理解为破坏性的更改更频繁（即 repo 处于 beta 版），因此放宽了语义版本控制，因此 MINOR 版本更改也意味着向后不兼容的版本。

变更日志应该便于用户和开发人员理解关键的变更，并且应该以以下格式反映上面描述的提交类别：

````
## 1.1.0 - 2020-06-25

### Added
- List of `NEW` commits

### Improved
- List of `IMPROVE` commits

### Fixed
- List of `FIX` commits

### Breaking
- List of `BREAKING` and `UPGRADE` commits

### Deprecated
- List of `DEPRECATE` commits

### Documented
- List of `DOCS` commits
````

没有内容的副标题可以被跳过，贡献者的提交应该被忽略（例如 “, thanks to @chrisjsewell”）

包发布应该通过 GitHub Action 工作流自动化，在创建标签时触发。例如：

- https://github.com/pypa/gh-action-pypi-publish
- https://github.com/pascalgn/npm-publish-action

使用[`needs` 键](https://docs.github.com/en/actions/reference/workflow-syntax-for-github-actions#jobsjob_idneeds) 来确保这些操作只在 pre-commit 和单元测试成功通过后才运行。

### 创建一个版本的过程

下面是创建一个版本的完整工作流：

- 进行发布提交 🚀 `RELEASE: ...` 在 master 上（通过 PR）将版本转换为 M.m.p（例如更改 Python 包的 `__version__`），并在 `CHANGELOG.md` 中添加一节在上面的格式。
- 为该提交创建一个 GitHub 版本，标签为 vM.m.p 版本（可以在正文中包括 changelog 部分）。
- 检查自动发布工作流是否成功完成。

## Deprecations

在存储库退出 beta 开发之后（即 `>= 1.0.0`），API（函数，类等）的弃用应该在更改日志和代码中显示出来，例如在 Python 包中使用 [Sphinx deprecated 指令](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#directive-deprecated) 和/或 [DeprecationWarning](https://docs.python.org/3/library/exceptions.html#DeprecationWarning)：

```python
import warnings

def deprecated(message):
    warnings.warn(message, DeprecationWarning, stacklevel=2)
```

理想情况下，应该在实际从代码库中删除它们之前添加一个或两个主要版本。还应该指定将在何时删除它们的未来版本，以及（如果适用）将被替换的内容。

在可能的情况下，还应该维护弃用项列表，例如：<https://www.sphinx-doc.org/en/master/extdev/deprecated.html>。