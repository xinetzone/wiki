# Markdown

参考：[Markdown — Sphinx documentation (sphinx-doc.org)](https://www.sphinx-doc.org/en/master/usage/markdown.html)

[Markdown](https://daringfireball.net/projects/markdown/) 是一种轻量级的标记语言，具有简单的纯文本格式化语法。它在句法上有许多不同的风格。为了支持基于 Markdown 的文档，Sphinx 可以使用 [MyST-Parser](https://myst-parser.readthedocs.io/en/latest/)。MyST-Parser 是 Docutils 与 [markdown-it-py](https://github.com/executablebooks/markdown-it-py) 的桥梁，这是一个用于解析 [CommonMark](https://commonmark.org/) Markdown 风格 的 Python 包。

## 配置

要配置 Sphinx 项目以获得 Markdown 支持，请执行以下操作：

- 安装 Markdown 分析器 MyST-Parser。

```shell
pip install --upgrade myst-parser
```

- 将 `myst_parser` 添加到[配置的扩展列表](https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-extensions)中。

```python
extensions = ['myst_parser']
```

- 如果你想使用扩展名不是 `.md` 的 Markdown 文件，调整 [source_suffix](https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-source_suffix) 变量。下面的例子将 Sphinx 配置为将所有扩展名为 `.md` 和 `.txt` 的文件解析为Markdown。

```python
source_suffix = {
    '.rst': 'restructuredtext',
    '.txt': 'markdown',
    '.md': 'markdown',
}
```

- 你可以进一步配置 MyST-Parser 以允许标准 CommonMark 不支持的自定义语法。在 [MyST-Parser 文档](https://myst-parser.readthedocs.io/en/latest/using/syntax-optional.html) 中阅读更多内容。
