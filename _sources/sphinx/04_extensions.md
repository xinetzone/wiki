(sphinx:extensions)=
# 插件

参考：[Extensions — Sphinx documentation (sphinx-doc.org)](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/index.html)

由于许多项目在文件中需要特殊功能，Sphinx 允许在构建过程中添加 "扩展"，每个扩展都可以修改文件处理的几乎任何方面。

本章描述了与 Sphinx 捆绑的扩展。关于编写你自己的扩展的 API 文档，请参考为 [Sphinx 开发扩展](https://www.sphinx-doc.org/zh_CN/master/extdev/index.html#dev-extensions)。

## 内置扩展

这些扩展是内置的，可以通过扩展配置值中各自的条目激活：

*   [`sphinx.ext.autodoc` – Include documentation from docstrings](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/autodoc.html)
    *   [Directives](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/autodoc.html#directives)
    *   [Configuration](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/autodoc.html#configuration)
    *   [Docstring preprocessing](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/autodoc.html#docstring-preprocessing)
    *   [Skipping members](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/autodoc.html#skipping-members)
*   [`sphinx.ext.autosectionlabel` – Allow reference sections using its title](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/autosectionlabel.html)
    *   [Configuration](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/autosectionlabel.html#configuration)
*   [`sphinx.ext.autosummary` – Generate autodoc summaries](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/autosummary.html)
    *   [**sphinx-autogen** – generate autodoc stub pages](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/autosummary.html#sphinx-autogen-generate-autodoc-stub-pages)
    *   [Generating stub pages automatically](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/autosummary.html#generating-stub-pages-automatically)
    *   [Customizing templates](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/autosummary.html#customizing-templates)
*   [`sphinx.ext.coverage` – Collect doc coverage stats](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/coverage.html)
*   [`sphinx.ext.doctest` – Test snippets in the documentation](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/doctest.html)
    *   [Directives](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/doctest.html#directives)
    *   [Skipping tests conditionally](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/doctest.html#skipping-tests-conditionally)
    *   [Configuration](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/doctest.html#configuration)
*   [`sphinx.ext.duration` – Measure durations of Sphinx processing](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/duration.html)
*   [`sphinx.ext.extlinks` – Markup to shorten external links](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/extlinks.html)
*   [`sphinx.ext.githubpages` – Publish HTML docs in GitHub Pages](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/githubpages.html)
*   [`sphinx.ext.graphviz` – Add Graphviz graphs](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/graphviz.html)
*   [`sphinx.ext.ifconfig` – Include content based on configuration](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/ifconfig.html)
*   [`sphinx.ext.imgconverter` – A reference image converter using Imagemagick](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/imgconverter.html)
    *   [Configuration](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/imgconverter.html#configuration)
*   [`sphinx.ext.inheritance_diagram` – Include inheritance diagrams](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/inheritance.html)
    *   [Examples](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/inheritance.html#examples)
    *   [Configuration](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/inheritance.html#configuration)
*   [`sphinx.ext.intersphinx` – Link to other projects’ documentation](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/intersphinx.html)
    *   [Configuration](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/intersphinx.html#configuration)
    *   [Showing all links of an Intersphinx mapping file](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/intersphinx.html#showing-all-links-of-an-intersphinx-mapping-file)
    *   [Using Intersphinx with inventory file under Basic Authorization](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/intersphinx.html#using-intersphinx-with-inventory-file-under-basic-authorization)
*   [`sphinx.ext.linkcode` – Add external links to source code](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/linkcode.html)
    *   [Configuration](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/linkcode.html#configuration)
*   [Math support for HTML outputs in Sphinx](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/math.html)
    *   [`sphinx.ext.imgmath` – Render math as images](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/math.html#module-sphinx.ext.imgmath)
    *   [`sphinx.ext.mathjax` – Render math via JavaScript](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/math.html#module-sphinx.ext.mathjax)
    *   [`sphinx.ext.jsmath` – Render math via JavaScript](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/math.html#module-sphinx.ext.jsmath)
*   [`sphinx.ext.napoleon` – Support for NumPy and Google style docstrings](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/napoleon.html)
    *   [Overview](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/napoleon.html#overview)
        *   [Getting Started](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/napoleon.html#getting-started)
        *   [Docstrings](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/napoleon.html#id1)
        *   [Docstring Sections](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/napoleon.html#docstring-sections)
        *   [Google vs NumPy](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/napoleon.html#google-vs-numpy)
        *   [Type Annotations](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/napoleon.html#type-annotations)
    *   [Configuration](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/napoleon.html#configuration)
*   [`sphinx.ext.todo` – Support for todo items](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/todo.html)
    *   [Configuration](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/todo.html#configuration)
*   [`sphinx.ext.viewcode` – Add links to highlighted source code](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/viewcode.html)
    *   [Configuration](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/viewcode.html#configuration)

## 三方扩展

你可以在 [sphinx-contrib](https://github.com/sphinx-contrib/) 组织中找到几个由用户贡献的扩展。如果你想把你的扩展加入这个组织，只需按照 [github-administration](https://github.com/sphinx-contrib/github-administration) 项目中提供的说明即可。这是可选的，有几个扩展托管在其他地方。[awesome-sphinxdoc](https://github.com/yoloseem/awesome-sphinxdoc) 项目包含了一个精心策划的 Sphinx 软件包列表，许多软件包使用了 [Framework :: Sphinx :: Extension](https://pypi.org/search/?c=Framework+%3A%3A+Sphinx+%3A%3A+Extension) 扩展和 [Framework :: Sphinx :: Theme](https://pypi.org/search/?c=Framework+%3A%3A+Sphinx+%3A%3A+Theme) 分类器，分别用于 Sphinx 扩展和主题。

## 把自己的扩展放在哪里？

一个项目的本地扩展应该放在项目的目录结构中。相应地设置 Python 的模块搜索路径，`sys.path`，以便 Sphinx 能找到它们。例如，如果你的扩展 `foo.py` 在项目根目录的 `exts` 子目录下，就在 `conf.py` 中写入：

```python
import sys, os

sys.path.append(os.path.abspath('exts'))

extensions = ['foo']
```

你也可以在 `sys.path` 的其他地方安装扩展，例如在 site-packages 目录下。