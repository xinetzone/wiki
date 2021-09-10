(sphinx)=
# Sphinx

````{div} w3-pale-green w3-padding
```{glossary}
[Sphinx](https://github.com/sphinx-doc/sphinx)
    Sphinx 是一个由 Georg Brandl 编写的工具，它可以方便地为 Python 项目（或包含多个 {term}`reStructuredText` 源的其他文档）创建智能而漂亮的文档。它最初是为新的 Python 文档创建的，并且为 Python 项目文档提供了极好的工具，但是它也支持 C/C++，并且计划支持更多的语言。

    Sphinx 使用 {term}`reStructuredText` 作为它的标记语言，它的许多优势来自于 {term}`reStructuredText` 及其解析和翻译套件 Docutils 的强大和直接性。
```
````

Sphinx 特点如下：
    
- **输出格式**：HTML（包括派生格式如 Windows HTML Help），[Epub](https://en.wikipedia.org/wiki/EPub) 和 Qt Help，纯文本，[手册](https://en.wikipedia.org/wiki/Man_page)和 LaTeX（或直接使用 rst2pdf 输出 [PDF](https://en.wikipedia.org/wiki/PDF)），[Texinfo](https://en.wikipedia.org/wiki/Texinfo)
- **广泛的交叉引用**：函数、类、术语表及类似信息的**语义标记**和**自动链接**
- **层次结构**：文档树的简单定义，自动链接到兄弟结点、父结点和子结点
- **自动目录**：通用目录和模块目录（index）
- **代码处理**：使用 [Pygments](https://pygments.org/) 自动高亮显示
- 内置[**扩展**](https://www.sphinx-doc.org/en/master/usage/extensions/index.html#builtin-sphinx-extensions)：例如自动测试代码片段和包含适当格式的文档字符串
- **三方扩展**：[用户贡献](https://www.sphinx-doc.org/en/master/usage/extensions/index.html#third-party-extensions)的几十个扩展；大多数都可以从 PyPI 安装

```{admonition} 参考：
- 官方文档：[Sphinx documentation](https://www.sphinx-doc.org/en/master/)
- 官方中文文档：[Sphinx 中文](https://www.sphinx-doc.org/zh_CN/master/)
- [sphinx-doc/sphinx-doc-translations](https://github.com/sphinx-doc/sphinx-doc-translations)：一个在 Read The Docs 网站上提供多版本和多语言的 Sphinx 官方文档的项目。
```