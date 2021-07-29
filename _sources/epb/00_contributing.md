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

### Git 分支

存储库应该使用 `master`（或者 `main`） 作为它们的主分支。这个分支在 GitHub 上应该是“受保护的”，并且在合并之前需要 PR 审查和状态检查（参见 [GitHub 分支配置](https://docs.github.com/en/github/administering-a-repository/configuring-protected-branches)）。

添加到 `master` 应该遵循以下简单的概念：

- 对所有新特性和 bug 修复使用 features 分支。
- 使用拉请求将特性分支合并到主分支中。
- 保持一个高质量，最新的主分支。