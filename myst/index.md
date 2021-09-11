(wiki:myst)=
# MyST

````{div} w3-pale-green w3-padding
```{glossary}
[MyST Markdown](https://myst-parser.readthedocs.io/en/latest/using/syntax.html)
[MyST](https://myst-parser.readthedocs.io/en/latest/using/syntax.html)
    MyST（全称 Markedly Structured Text）专为 {term}`Sphinx` 项目使用而设计的 Markdown 风格。它是 {term}`CommonMark Markdown <CommonMark>` 和一些额外的语法片段的组合，以支持 Sphinx 的功能，因此您可以用纯 Markdown 编写 Sphinx 文档。它是 {term}`Jupyter Book` 使用的核心技术之一。

[MyST-Parser](https://myst-parser.readthedocs.io/en/latest/)
    MyST-Parser 是 {term}`Sphinx` 的解析器，它可以读取以 MyST Markdown 编写的内容。{term}`MyST-NB` 也使用它来解析 Jupyter 笔记本内部的 MyST Markdown。
    
    它包含一个使用 [markdown-it-py](https://markdown-it-py.readthedocs.io/) 的符合 [CommonMark](https://commonmark.org/) 标准的扩展解析器，以及一个允许你在 Sphinx 中编写 MyST Markdown 的 Sphinx 扩展。
```
````

:::{panels}
:container: +full-width text-center
:header: w3-pale-yellow
:column: col-lg-4 px-2 py-2
:card: w3-pale-red

**[符合 CommonMark 标准](commonmark-block-tokens)** ✔
^^^
MyST 是 [CommonMark Markdown][commonmark] 的一个超集。任何 CommonMark 文档也都是符合 MyST 的。
---

**[写作的额外语法](extended-block-tokens)** ✍
^^^
MyST 扩展了 CommonMark 的语法，[旨在用于学术写作和技术文档](extended-block-tokens)。

---
**[可扩展的语法](syntax/directives)** 🚀
^^^
MyST 提供了 [角色](syntax/roles) 和 [指令](syntax/directives)，使你可以扩展 MyST 的功能。

---
**[与 Sphinx 兼容 ](sphinx/index)** 📄
^^^
MyST 的灵感来自于 Sphinx，并[配有自己的 Sphinx 分析器](sphinx/index)。[用 Markdown 编写你的 Sphinx 文档](https://www.sphinx-doc.org/en/master/usage/quickstart.html)，或将现有的 [RST 转换为 Markdown][rst-to-myst]！

---
**[可以用 Python 破解](api/index)** 🐍
^^^
这个 MyST 解析器是建立在 [`markdown-it-py` package][markdown-it-py] 包之上的，这是一个可拔插的 Markdown 的 Python 解析器。

---
**[可以用 Javascript 破解 ][markdown-it-myst]** 🌍
^^^
该 [JavaScript 解析器][markdown-it-myst] 建立在 [markdown-it][markdown-it] 的基础上，允许你在网站中解析 MyST。
:::

[commonmark]: https://commonmark.org/
[github-ci]: https://github.com/executablebooks/MyST-Parser/workflows/continuous-integration/badge.svg?branch=master
[github-link]: https://github.com/executablebooks/MyST-Parser
[codecov-badge]: https://codecov.io/gh/executablebooks/MyST-Parser/branch/master/graph/badge.svg
[codecov-link]: https://codecov.io/gh/executablebooks/MyST-Parser
[rtd-badge]: https://readthedocs.org/projects/myst-parser/badge/?version=latest
[rtd-link]: https://myst-parser.readthedocs.io/en/latest/?badge=latest
[black-badge]: https://img.shields.io/badge/code%20style-black-000000.svg
[pypi-badge]: https://img.shields.io/pypi/v/myst-parser.svg
[pypi-link]: https://pypi.org/project/myst-parser
[conda-badge]: https://anaconda.org/conda-forge/myst-parser/badges/version.svg
[conda-link]: https://anaconda.org/conda-forge/myst-parser
[black-link]: https://github.com/ambv/black
[github-badge]: https://img.shields.io/github/stars/executablebooks/myst-parser?label=github
[markdown-it-py]: https://markdown-it-py.readthedocs.io/
[markdown-it-myst]: https://github.com/executablebooks/markdown-it-myst
[markdown-it]: https://markdown-it.github.io/
[rst-to-myst]: https://rst-to-myst.readthedocs.io
