(sphinx:configuration)=
# 配置

参考：[Configuration — Sphinx documentation (sphinx-doc.org)](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html)

[配置目录](https://www.sphinx-doc.org/zh_CN/master/glossary.html#term-configuration-directory) 必须包含一个名为 `conf.py` 的文件。这个文件（包含 Python 代码）被称为 "构建配置文件"，包含（几乎）所有自定义 Sphinx 输入和输出行为所需的配置。

> 一个可选的文件 [docutils.conf](https://docutils.sourceforge.io/docs/user/config.html) 可以被添加到配置目录，以调整 [Docutils](https://docutils.sourceforge.io/) 配置，如果没有被 Sphinx 覆盖或设置的话。

配置文件在构建时作为 Python 代码执行（使用 `execfile()`，并且当前目录设置为其包含目录），因此可以执行任意复杂的代码。然后，Sphinx 从文件命名空间中读取简单名称作为其配置。

```{important}
* 如果没有其他文件规定，值必须是**字符串**，其默认值是空字符串。
* 术语 “fully-qualified name” 指的是在一个模块中命名可导入的 Python 对象的字符串；例如，FQN "`sphinx.builders.Builder`" 表示 `sphinx.builders` 模块中的 `Builder` 类。
* 请记住，文档名称使用 `/` 作为路径分隔符，并且不包含文件扩展名。
* 由于 `conf.py` 是作为一个 Python 文件被读取的，通常的规则适用于编码和 Unicode 支持。
* 挑选 config 命名空间的内容（以便 Sphinx 可以在配置更改时找到），因此它可能不包含不可删除的值 - 如果合适，可以使用 `del` 从命名空间中删除它们。模块会自动删除，因此您在使用后无需 `del` 导入。
* 配置文件中有一个名为 `tags` 的特殊对象。它可用于查询和更改标记（请参阅 [引用基于标签的内容](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#tags)）。使用 `tags.has('tag')` 来查询，`tags.add('tag')` 和 `tags.remove('tag')` 来改变。只有通过 `-t` 命令行选项或 `tags.add('tag')` 设置的标签可以使用 `tags.has('tag')` 进行查询。请注意，当前构建器标记在 `conf.py` 中不可用，因为它是在初始化构建器 *之后* 创建的。
```

## 项目信息

`project`
:    记录的项目名称。

`author`
:    该文件的作者姓名。默认值为 'unknown'。

`copyright`
:    '2008, Author Name' 风格的版权声明。

`project_copyright`
:   `copyright` 的别名。

`version`
:   主要项目版本, 用作 `|version|` 的替代品。例如, 对于 Python 文档, 这可能类似于 `2.6`。

`release`
:    完整的项目版本, 用作 `|release|` 的替代品, 例如在 HTML 模板中。例如, 对于 Python 文档, 这可能类似于 `2.6.0rc1`。如果你不需要在 `version` 和 `release` 之间提供分隔, 只需将它们设置为相同的值即可。

## 一般配置项

[`extensions`](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/index.html)
:   模块名称为 `extensions` 的字符串列表。这些可以是 Sphinx（名为 `sphinx.ext.*`）或自定义的扩展。

    请注意, 如果扩展名位于另一个目录中, 则可以在 `conf` 文件中扩展 [sys.path](https://docs.python.org/3/library/sys.html#sys.path) - 但请确保使用绝对路径。如果您的扩展路径是相对于 [configuration directory](https://www.sphinx-doc.org/zh_CN/master/glossary.html#term-configuration-directory)，请使用 `os.path.abspath()` 之类的：

    ```python
    import sys, os

    sys.path.append(os.path.abspath('sphinxext'))

    extensions = ['extname']
    ```

    这样, 你可以从子目录 `sphinxext` 加载一个名为 `extname `的扩展名。

    配置文件本身可以是扩展名；为此, 你只需要提供一个 `setup()` 函数。

`source_suffix`
:   源文件的文件扩展名。Sphinx 认为有这个后缀的文件是源文件。这个值可以是一个映射文件扩展名和文件类型的字典。比如：

    ```python
    source_suffix = {
        '.rst': 'restructuredtext',
        '.txt': 'restructuredtext',
        '.md': 'markdown',
    }
    ```

    默认情况下, Sphinx 仅支持 `'restructuredtext'` 文件类型。 您可以使用源解析器扩展添加新文件类型。请阅读扩展文档以了解扩展程序支持的文件类型。

    该值也可以是文件扩展名列表：然后 Sphinx 会认为它们都映射到 'restructuredtext' 文件类型。

    默认是 `{'.rst': 'restructuredtext'}`。

`source_encoding`
:   所有 reST 源文件的编码。推荐的编码和默认值是 `'utf-8-sig'`。

`source_parsers`
:   如果给出, 则不同源的解析器类字典就足够了。键是后缀, 值可以是类或字符串, 给出解析器类的完全限定名称。解析器类可以是 `docutils.parsers.Parser` 或 [`sphinx.parsers.Parser`](https://www.sphinx-doc.org/zh_CN/master/extdev/parserapi.html#sphinx.parsers.Parser)。具有不在字典中的后缀的文件将使用默认的 reStructuredText 解析器进行解析。

    例如:

    ```python
    source_parsers = {'.md': 'recommonmark.parser.CommonMarkParser'}
    ```

    ```{deprecated} 1.8 版后已移除
    现在, Sphinx 提供了一个 API [`Sphinx.add_source_parser()`](https://www.sphinx-doc.org/zh_CN/master/extdev/appapi.html#sphinx.application.Sphinx.add_source_parser) 来注册一个源解析器。请改用它。
    ```

`root_doc`
:   "根"文档的名称，即包含根 `toctree` 指令的文档。默认为 `"index"`。

`exclude_patterns`
:   查找源文件时应排除的 `glob` 样式模式列表[^1]。 它们与源目录相对于源目录进行匹配, 在所有平台上使用斜杠作为目录分隔符。

    示例模式:

    * `'library/xml.rst'` – 忽略 `library/xml.rst` 文件（替换条目 `unused_docs`）
    * `'library/xml'` – 忽略 `library/xml` 目录
    * `'library/xml*'` – 忽略以 `library/xml` 开头的所有文件和目录
    * `'**/.svn'` – 忽略所有 `.svn` 目录

    在 [`html_static_path`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-html_static_path) 和 [`html_extra_path`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-html_extra_path) 中查找静态文件时也会查询 [`exclude_patterns`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-exclude_patterns)

`templates_path`
:   包含额外模板（或覆盖内置/特定主题模板）的路径列表。相对路径被认为是相对于配置目录的。

`template_bridge`
:   一个字符串，包含一个可调用（或简单的类）的全称，它返回一个 [TemplateBridge](https://www.sphinx-doc.org/zh_CN/master/extdev/appapi.html#sphinx.application.TemplateBridge) 的实例。这个实例然后被用来渲染 HTML 文档，也可能是其他构建器的输出（目前是变化构建器）。（注意，如果要使用 HTML 主题，模板桥必须具有主题感知能力。）

`rst_epilog`
:   reStructuredText 字符串，它将包含在每个被读取的源文件的末尾。这是一个可以添加每个文件中都应该有的替换的地方（另一个是 `rst_prolog`）。一个例子：

    ```python
    rst_epilog = """
    .. |psf| replace:: Python Software Foundation
    """
    ```

`rst_prolog`
:   reStructuredText 字符串，它将包含在每个被读取的源文件的开头。这是一个可以添加应该在每个文件中可用的替换的地方（另一个是 `rst_epilog`）。 一个例子：

    ```python
    rst_prolog = """
    .. |psf| replace:: Python Software Foundation
    """
    ```

`primary_domain`
:   默认[域](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html)的名称。也可以用 `None` 来禁用默认域。默认是 `'py'`。那些在其他域中的对象（无论域名是明确给出的，还是由 `default-domain` 指令选择的），在命名时都将明确地预留域名（例如，当默认域是 C 时，Python 函数将被命名为 "Python function"，而不仅仅是 "function"）。

`default_role`
:   一个 reST 角色的名字（内置的或 Sphinx 扩展的），作为默认的角色，也就是说，用于标记为 `` `like this` `` 的文本。这可以设置为 `'py:obj'`，使 `` `filter` `` 成为 Python 函数 "filter" 的交叉引用。默认是 `None`，它不会重新分配默认的角色。

    默认的角色总是可以在个别文件中使用标准的 reST `default-role` 指令来设置。

`keep_warnings`
:   如果为真，在构建的文档中保留警告为 "系统消息" 段落。不管这个设置如何，警告总是在 `sphinx-build` 运行时被写入标准错误流。

`suppress_warnings`
:   一个警告类型的列表，用于抑制任意的警告信息。

    Sphinx 支持以下警告类型：

    * `app.add_node`
    * `app.add_directive`
    * `app.add_role`
    * `app.add_generic_role`
    * `app.add_source_parser`
    * `download.not_readable`
    * `image.not_readable`
    * `ref.term`
    * `ref.ref`
    * `ref.numref`
    * `ref.keyword`
    * `ref.option`
    * `ref.citation`
    * `ref.footnote`
    * `ref.doc`
    * `ref.python`
    * `misc.highlighting_failure`
    * `toc.circular`
    * `toc.secnum`
    * `epub.unknown_project_files`
    * `epub.duplicated_toc_entry`
    * `autosectionlabel.*`


`needs_sphinx`
:   如果设置为 `major.minor` 版本字符串, 如 '1.1' , Sphinx 会将其与版本进行比较, 如果它太旧则拒绝构建。默认是没有要求的。

`needs_extensions`
:   这个值可以是一个字典，指定扩展中对扩展的版本要求，例如，`needs_extensions = {'sphinxcontrib.something': '1.5'}`。版本字符串的形式应该是 `major.minor`。不需要为所有扩展指定要求，只需要为那些你想检查的扩展指定。

```{note}
这要求扩展指定其版本给 Sphinx（参见 [开发扩展](https://www.sphinx-doc.org/zh_CN/master/extdev/index.html#dev-extensions)，了解如何做到这一点）。
```

[^1]: A note on available globbing syntax: you can use the standard shell constructs *, ?, [...] and [!...] with the feature that these all don’t match slashes. A double star ** can be used to match any sequence of characters including slashes.

更多内容请阅读：[配置 — Sphinx documentation (sphinx-doc.org)](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html)。
