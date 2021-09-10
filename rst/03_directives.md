(rst:directives)=
# 指令

[如前所述](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/basics.html#rst-directives)，指令是显式标记的通用块。虽然 Docutils 提供了许多指令, 但 Sphinx 提供了更多指令, 并使用指令作为主要的扩展机制之一。

请参阅 [域](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html)，了解域添加的角色。

```{seealso}
有关 Docutils 提供的指令的概述, 请参阅 [reStructuredText Primer](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/basics.html#rst-directives) 。
```

```{panels}
:container: +full-width
:column: col-lg-9 px-2 py-2
---
:header: w3-light-blue
reStructuredText 指令
^^^
指令可以有**参数**（arguments）、**选项**（options）和**内容**（content）。

- **参数**在指令名称后面的双冒号后直接给出。每个指令决定它是否可以有参数，以及参数的数量。
- **选项**是在参数之后以 "字段列表" 的形式给出的。
- **内容**跟在选项或参数之后的空行。每个指令都决定是否允许内容，以及如何处理它。

指令的一个常见问题是，**内容**的第一行必须缩进到与**选项**相同的水平。
```

## 目录

由于 reST 没有连接多个文档或将文档拆分为多个输出文件的工具, 因此 Sphinx 使用自定义指令来添加文档所包含的单个文件之间的关系, 以及目录。`toctree` 指令是中心元素。

```{note}
可以使用 [include](http://docutils.sourceforge.net/docs/ref/rst/directives.html#include) 指令将一个文件简单地”包含”在另一个文件中。

要为当前文件（`.rst` 文件）创建目录，请使用标准的 [reST 内容指令](http://docutils.sourceforge.net/docs/ref/rst/directives.html#table-of-contents)。
```

`` .. toctree:: ``
:  该指令使用指令体中给出的文档的各个 TOC（包括 “sub-TOC tree”）在当前位置插入 “TOC tree”。相对文档名称（不以斜杠开头）与指令所在的文档相关, 绝对名称相对于源目录。可以给出数字”maxdepth”选项以指示树的深度；默认情况下, 包括所有级别。[^1]

   考虑这个例子（取自 Python docs 的库引用索引）：

   ```rst
   .. toctree::
      :maxdepth: 2

      intro
      strings
      datatypes
      numeric
      (many more documents listed here)
   ```

   这完成了两件事:

   * 插入所有这些文档的目录, 最大深度为2, 表示一个嵌套标题。这些文件中的 `toctree` 指令也被考虑在内。
   * Sphinx 知道文档 `intro`，`strings` 等的相对顺序, 并且它知道它们是所显示文档的子项, 即库索引。根据这些信息, 它会生成 “next chapter”，“previous chapter” 和 “parent chapter” 链接。

   **Entries**

   第一个 [`toctree`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-toctree "toctree directive") 中的文档标题将自动从引用文档的标题中读取。如果这不是您想要的, 您可以使用与 reST 超链接类似的语法指定显式标题和目标（和 Sphinx 的 [cross-referencing syntax](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html#xref-syntax)）。这看起来像：

   ```rst
   .. toctree::

      intro
      All about strings <strings>
      datatypes
   ```

   上面的第二行将链接到 `strings` 文档, 但是将使用标题 “All about strings” 而不是 `strings` 文档的标题。

   您还可以通过提供 HTTP URL 而不是文档名称来添加外部链接。

   **章节编号**

   如果你想在HTML输出中有节号, 请给 **toplevel** toctree 一个 `numbered` 选项。例如:

   ```rst
   .. toctree::
      :numbered:

      foo
      bar
   ```

   编号然后从 `foo` 的标题开始。子 toctrees 是自动编号的（不要给那些 `numbered` flag）。

   通过将深度作为 `numbered` 的数字参数, 也可以编号到特定深度。

   **其他选项**

   你可以使用 `caption` 选项提供一个 toctree 标题, 你可以使用 `name` 选项来提供可以通过使用引用的隐式目标名 [`ref`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html#role-ref "ref role")

   ```rst
   .. toctree::
      :caption: Table of Contents
      :name: mastertoc

      foo
   ```

   如果只想显示树中文档的标题, 而不是同一级别的其他标题, 则可以使用 `titlesonly` 选项:

   ```rst
   .. toctree::
      :titlesonly:

      foo
      bar
   ```

   你可以在toctree指令中使用 “globbing ”，给出 `glob` 标志选项。然后将所有条目与可用文档列表进行匹配, 并按字母顺序将匹配项插入到列表中。例:

   ```rst
   .. toctree::
      :glob:

      intro*
      recipe/*
      *
   ```

   这首先包括所有名称以 `intro` 开头的文件, 然后是 `recipe` 文件夹中的所有文件, 然后是所有剩余的文件(当然包含该指令的文件除外)。[^2]

   特殊条目名称 `self` 代表包含toctree指令的文档。如果要从toctree生成”站点地图”, 这非常有用。

   您可以使用 `reversed` 标志选项来反转列表中条目的顺序。当使用 `glob` 标志选项来反转文件的顺序时, 这非常有用。例:

   ```rst
   .. toctree::
      :glob:
      :reversed:

      recipe/*
   ```

   你也可以给指令一个”隐藏”选项, 比如:

   ```rst
   .. toctree::
      :hidden:

      doc_1
      doc_2
   ```

   这仍将通知 Sphinx 文档层次结构, 但不会在指令的位置插入文档中的链接 - 如果您打算自己, 以不同的样式或 HTML 侧边栏插入这些链接, 这是有意义的。

   如果您只想拥有一个顶级toctree并隐藏所有其他较低级别的 toctree, 则可以将”includehidden”选项添加到顶级 `toctree` 条目:

   ```rst
   .. toctree::
      :includehidden:

      doc_1
      doc_2
   ```

   然后可以通过 “hidden” 选项消除所有其他toctree条目。

   最后, [source directory](https://www.sphinx-doc.org/zh_CN/master/glossary.html#term-source-directory) (或子目录)中的所有文档必须出现在某些 `toctree` 指令中;如果Sphinx找到未包含的文件, 它将发出警告, 因为这意味着无法通过标准导航访问此文件。

   使用 [`exclude_patterns`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-exclude_patterns) 可以完全排除文档或目录。使用 [“orphan” 元数据](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/field-lists.html#metadata) 来构建文档, 但通知Sphinx它是无法通过toctree访问的。

   “master document” (由 [`master_doc`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-master_doc) 选择)是TOC树层次结构的 “root” 。如果你没有给出 `maxdepth` 选项, 它可以用作文档的主页面, 也可以用作 “完整的目录”。

   ```{versionchanged} 0.3
   添加了 “globbing” 选项。
   ```

   ```{versionchanged} 0.6
   添加了 “numbered” 和 “hidden” 选项以及外部链接和对 “self” 引用的支持。
   ```

   ```{versionchanged} 1.0
   添加了 “titlesonly” 选项。
   ```

   ```{versionchanged} 1.1
   在 “numbered” 中添加了数字参数。
   ```

   ```{versionchanged} 1.2
   添加了 “includehidden” 选项。
   ```

   ```{versionchanged} 1.3
   添加了 “caption” 和 “name” 选项。
   ```

### 特别的名字

Sphinx 保留一些文件名供自己使用;您不应该尝试使用这些名称创建文档 - 这将导致问题。

特殊文档名称（以及为它们生成的页面）是：

`genindex`, `modindex`, `search`
:  它们分别用于通用索引, Python 模块索引和搜索页面。

   通用索引用模块中的项填充, 所有索引生成 [对象描述](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#basic-domain-markup)，以及来自 [`index`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-index "index directive") 指令。

  Python 模块索引包含一个条目 [`py:module`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-py:module "py:module directive") 指令。

  搜索页面包含一个表单, 该表单使用生成的 JSON 搜索索引和 JavaScript 对搜索词的生成文档进行全文搜索;它应该适用于支持现代 JavaScript 的每个主流浏览器。

以 `_` 开头的每个名字
:  虽然 Sphinx 目前只使用了很少这样的名称, 但您不应该创建包含此类名称的文档或包含文档的目录。（使用 `_` 作为自定义模板目录的前缀是可以的。）

```{warning}
小心文件名中的异常字符。某些格式可能会以意外的方式解释这些字符：

* 对于基于 HTML 的格式, 不要使用冒号 `:`。与其他部分的链接可能无效。
* 不要使用加号 `+` 作为 ePub 格式。可能找不到某些资源。
```

## 段落级标记

这些指令创建短段落, 可以在内部信息单元和普通文本中使用。

`` .. note:: ``
:  关于 API 的一个特别重要的信息, 用户在使用该注释所涉及的任何 API 时应该注意这些信息。指令的内容应以完整的句子写成, 并包括所有适当的标点符号。

   例:

   ````{panels}
   :container: w3-card-4 w3-pale-green w3-padding
   :column: col-lg-12 px-2 py-2
   ---
   :header: w3-pale-blue
   RST
   ^^^
   ```rst
   .. note::

      This function is not suitable for sending spam e-mails.
   ```
   ---
   :header: w3-pale-red
   渲染：
   ^^^
   ```{eval-rst}
   .. note::

      This function is not suitable for sending spam e-mails.
   ```
   ````

`` .. warning:: ``
:  关于 API 的一些重要信息, 用户在使用警告所涉及的任何API时都应该非常清楚。指令的内容应以完整的句子写成, 并包括所有适当的标点符号。这不同于 [`note`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-note "note directive")，因为建议使用 [`note`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-note "note directive") 以获取有关安全性的信息。

`` .. versionadded:: version ``
:  该指令记录了将所描述的功能添加到库或 C API 的项目版本。当这适用于整个模块时, 应该在任何散文之前将其放置在模块部分的顶部。

   第一个参数必须给定，是有关的版本；你可以添加第二个参数，包括对变更的简要解释。

   例：

   ````{panels}
   :container: w3-card-4 w3-pale-green w3-padding
   :column: col-lg-12 px-2 py-2
   ---
   :header: w3-pale-blue
   RST
   ^^^
   ```rst
   .. versionadded:: 2.5
      The *spam* parameter.
   ```
   ---
   :header: w3-pale-red
   渲染：
   ^^^
   ```{eval-rst}
   .. versionadded:: 2.5
      The *spam* parameter.
   ```
   ````

   请注意, 指令头和说明之间必须没有空行；这是为了使这些块在标记中在视觉上连续。

`` .. versionchanged:: version ``
:  类似于 [`versionadded`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-versionadded "versionadded directive")，但以某种方式描述命名特征中的更改时间和内容(新参数, 更改的副作用等)。

`` .. deprecated:: version ``
:  类似于 [`versionchanged`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-versionchanged "versionchanged directive")，但描述了该功能何时被弃用。还可以给出解释, 例如通知读者应该使用什么。例：

   ````{panels}
   :container: w3-card-4 w3-pale-green w3-padding
   :column: col-lg-6 px-2 py-2
   ---
   :header: w3-pale-blue
   RST
   ^^^
   ```rst
   .. deprecated:: 3.1
      Use :func:`spam` instead.
   ```
   ---
   :header: w3-pale-red
   渲染：
   ^^^
   ```{eval-rst}
   .. deprecated:: 3.1
      Use :func:`spam` instead.
   ```
   ````

`` .. seealso:: ``
:  许多部分包括对模块文档或外部文档的引用列表。这些列表使用 [`seealso`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-seealso "seealso directive") 指令创建。

   [`seealso`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-seealso "seealso directive") 指令通常放在任何子部分之前的部分中。对于HTML输出, 它显示为从文本的主流中框出来。

   以下内容 [`seealso`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-seealso "seealso directive") 指令应该是reST定义列表。例：

   ````{panels}
   :container: w3-card-4 w3-pale-green w3-padding
   :column: col-lg-12 px-2 py-2
   ---
   :header: w3-pale-blue
   RST
   ^^^
   ```rst
   .. seealso::

      Module :py:mod:`zipfile`
         Documentation of the :py:mod:`zipfile` standard module.

      `GNU tar manual, Basic Tar Format <http://link>`_
         Documentation for tar archive files, including GNU tar extensions.
   ```
   ---
   :header: w3-pale-red
   渲染：
   ^^^
   ```{eval-rst}
   .. seealso::

      Module :py:mod:`zipfile`
         Documentation of the :py:mod:`zipfile` standard module.

      `GNU tar manual, Basic Tar Format <http://link>`_
         Documentation for tar archive files, including GNU tar extensions.
   ```
   ````

   还有一个 “短式” 允许看起来像这样：

   ````{panels}
   :container: w3-card-4 w3-pale-green w3-padding
   :column: col-lg-12 px-2 py-2
   ---
   :header: w3-pale-blue
   RST
   ^^^
   ```rst
   .. seealso:: modules :py:mod:`zipfile`, :py:mod:`tarfile`
   ```
   ---
   :header: w3-pale-red
   渲染：
   ^^^
   ```{eval-rst}
   .. seealso:: modules :py:mod:`zipfile`, :py:mod:`tarfile`
   ```
   ````

   **0.5 新版功能:** 短式。

`` .. rubric:: title ``
:  该指令创建一个段落标题, 不用于创建目录节点。

   ```{note}
   如果标题的 *title* 是 “Footnotes ” (或所选语言的等价物), 则LaTeX编写器会忽略此标题, 因为假定它仅包含脚注定义, 因此会创建一个空标题。
   ```

`` .. centered:: ``
:  该指令创建一个居中的粗体文本行。使用方法如下:

   ```rst
   .. centered:: LICENSE AGREEMENT
   ```

   **1.1 版后已移除:** 此演示文稿指令是旧版本的遗留代码。使用 `rst-class` 指令代替并添加适当的样式。

`` .. hlist:: ``
:  该指令必须包含项目符号列表。它会通过水平分布多个项目或减少项目间距来将其转换为更紧凑的列表, 具体取决于构建器。

   对于支持水平分布的构建器, 有一个 `columns` 选项, 用于指定列数；它默认为2。示例：

   ````{panels}
   :container: w3-card-4 w3-pale-green w3-padding
   :column: col-lg-12 px-2 py-2
   ---
   :header: w3-pale-blue
   RST
   ^^^
   ```rst
   .. hlist::
      :columns: 3

      * A list of
      * short items
      * that should be
      * displayed
      * horizontally
   ```
   ---
   :header: w3-pale-red
   渲染：
   ^^^
   ```{eval-rst}
   .. hlist::
      :columns: 3

      * A list of
      * short items
      * that should be
      * displayed
      * horizontally
   ```
   ````

   **0.6 新版功能.**

## 代码高亮

有多种方法可以在 Sphinx 中显示语法高亮的文字代码块：

-  使用 [reST doctest blocks](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/basics.html#rst-doctest-blocks)
-  使用 [reST literal blocks](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/basics.html#rst-literal-blocks)，可选择与 [`highlight`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-highlight "highlight directive") 指令结合使用
-  使用 [`code-block`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-code-block "code-block directive") 指令
-  并使用 [`literalinclude`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-literalinclude "literalinclude directive") 指令。

[Doctest blocks](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/basics.html#rst-doctest-blocks) 只能用于显示交互式Python会话, 而其余三个可用于其他语言。在这三个文件中, 当整个文档或至少大部分文档使用具有相同语法的代码块并且应该以相同的方式设置样式时, 文字块是有用的。另一方面, 当你想要对每个块的样式进行更精细的控制, 或者当你有一个包含使用多种不同语法的代码块的文档时, 第一个 [`code-block`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-code-block "code-block directive") 指令更有意义。最后, [`literalinclude`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-literalinclude "literalinclude directive") 指令对于在文档中包含整个代码文件非常有用。

在所有情况下, 语法高亮由 [Pygments](http://pygments.org/) 提供。使用文字块时, 使用源文件中的任何 [`highlight`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-highlight "highlight directive") 指令进行配置。当遇到 `highlight` 指令时, 它会被使用, 直到遇到下一个 `highlight` 指令。如果文件中没有 `highlight` 指令, 则使用全局突出显示语言。这默认为 `python`，但可以使用 [`highlight_language`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-highlight_language) 配置值进行配置。支持以下值:

* `none` (没有突出显示)
* `default` (类似于 `python3` 但是回退到 `none` 没有警告突出显示失败;默认情况下 [`highlight_language`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-highlight_language) 未设置)
* `guess` (让Pygments根据内容猜测词法分析器, 只适用于某些识别良好的语言)
* `python`
* `rest`
* `c`
* … 以及Pygments支持的任何其他 [词法分析器别名](http://pygments.org/docs/lexers/)

如果使用所选语言突出显示失败(即Pygments发出 “错误” 标记), 则不会以任何方式突出显示该块。

```{important}
支持的词法分类别名列表与Pygment版本相关联。如果要确保一致突出显示, 则应修复Pygments的版本。
```

`` .. highlight:: language ``
:  例：

   ```rst
   .. highlight:: c
   ```

   这个语言会一直使用到遇到下一个高亮指令。如前所述，语言可以是 Pygments 支持的任何词法别名。

   **选项**

   `:linenothreshold:` threshold (number (optional))
   :  启用为代码块生成行号。

      这个选项需要一个可选的数字作为阈值参数。如果给出任何阈值，指令将只为超过 N 行的代码块生成行号。如果不给定，将为所有的代码块产生行号。

      例如：

      ```rst
      .. highlight:: python
         :linenothreshold: 5
      ```

      这将为超过五行的所有代码块生成行号。

   `:force:` (no value)
   :  如果给定，高亮显示的小错误会被忽略。

`` .. code-block:: [language] ``
:  例：

   ```rst
   .. code-block:: ruby

      Some Ruby code.
   ```

   该指令的别名 `sourcecode` 也可以。该指令将语言名称作为参数。它可以是Pygments支持的任何词法分析别名。如果没有给出, 将使用 [`highlight`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-highlight "highlight directive") 指令的设置。如果没有设置, 将使用 [`highlight_language`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-highlight_language) 。

   **选项**

   Pygments可以为代码块生成行号。要启用此功能, 请使用 `linenos` 标志选项。

   ````{panels}
   :container: w3-card-4 w3-pale-green w3-padding
   :column: col-lg-6 px-2 py-2
   ---
   :header: w3-pale-blue
   RST
   ^^^
   ```rst
   .. code-block:: ruby
      :linenos:

      Some more Ruby code.
   ```
   ---
   :header: w3-pale-red
   渲染：
   ^^^
   ```{eval-rst}
   .. code-block:: ruby
      :linenos:

      Some more Ruby code.
   ```
   ````

   可以使用 `lineno-start` 选项选择第一个行号。如果存在, `linenos` 标志会自动激活：

   ````{panels}
   :container: w3-card-4 w3-pale-green w3-padding
   :column: col-lg-12 px-2 py-2
   ---
   :header: w3-pale-blue
   RST
   ^^^
   ```rst
   .. code-block:: ruby
      :lineno-start: 10

      Some more Ruby code, with line numbering starting at 10.
   ```
   ---
   :header: w3-pale-red
   渲染：
   ^^^
   ```{eval-rst}
   .. code-block:: ruby
      :lineno-start: 10

      Some more Ruby code, with line numbering starting at 10.
   ```
   ````

   另外, 可以给出一个 `emphasisize-lines` 选项让 Pygments 强调特定的行：

   ````{panels}
   :container: w3-card-4 w3-pale-green w3-padding
   :column: col-lg-6 px-2 py-2
   ---
   :header: w3-pale-blue
   RST
   ^^^
   ```rst
   .. code-block:: python
      :emphasize-lines: 3,5

      def some_function():
         interesting = False
         print 'This line is highlighted.'
         print 'This one is not...'
         print '...but this one is.'
   ```
   ---
   :header: w3-pale-red
   渲染：
   ^^^
   ```{eval-rst}
   .. code-block:: python
      :emphasize-lines: 3,5

      def some_function():
         interesting = False
         print 'This line is highlighted.'
         print 'This one is not...'
         print '...but this one is.'
   ```
   ````

   可以给出 `caption` 选项以在代码块之前显示该名称。可以使用 `name` 选项提供隐式目标名称, 可以使用以下命令引用 [`ref`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html#role-ref "ref role") 。例如：

   ````{panels}
   :container: w3-card-4 w3-pale-green w3-padding
   :column: col-lg-12 px-2 py-2
   ---
   :header: w3-pale-blue
   RST
   ^^^
   ```rst
   .. code-block:: python
      :caption: this.py
      :name: this-py

      print 'Explicit is better than implicit.'
   ```
   ---
   :header: w3-pale-red
   渲染：
   ^^^
   ```{eval-rst}
   .. code-block:: python
      :caption: this.py
      :name: this-py

      print 'Explicit is better than implicit.'
   ```
   ````

   可以使用 `dedent` 选项从代码块中去除缩进字符。例如：

   ```rst
   .. code-block:: ruby
      :dedent: 4

         some ruby code
   ```

   **在 1.1 版更改:** 添加了 “强调线” 选项。

   **在 1.3 版更改:** 添加了 `lineno-start`, `caption`, `name` 和 `dedent` 选项。

   **在 1.6.6 版更改:** LaTeX支持 `emphasisize-lines` 选项。

   **在 2.0 版更改:** `language` 参数变为可选参数。

`` .. literalinclude:: filename ``
:  通过将示例文本存储在仅包含纯文本的外部文件中, 可以包括更长的逐字文本显示。可以使用 `literalinclude` 指令包含该文件。 [^3]

   例如, 要包含Python源文件 `example.py`，使用:

   ```rst
   .. literalinclude:: example.py
   ```

   文件名通常相对于当前文件的路径。但是, 如果它是绝对的(以 `/` 开头), 它相对于顶级源目录。

   **选项**

   像 [`code-block`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-code-block "code-block directive")，该指令支持 `linenos` 标志选项来打开行号, `lineno-start` 选项来选择第一行号, `强调<span> -<span> lines` 选项用于强调特定行, `name` 选项用于提供隐式目标名称, `dedent` 选项用于删除代码块的缩进字符, 还有 `language` 选项用于选择a语言与当前文件的标准语言不同。另外, 它支持 `caption` 选项;但是, 这可以提供没有参数使用文件名作为标题。选项示例:

   ```rst
   .. literalinclude:: example.rb
      :language: ruby
      :emphasize-lines: 12,15-18
      :linenos:
   ```

   如果给出带有所需制表符宽度的 `tab-width` 选项, 则会扩展输入中的制表符。

   假设包含文件在 [`source_encoding`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-source_encoding) 中编码。如果文件具有不同的编码, 则可以使用 `encoding` 选项指定它:

   ```rst
   .. literalinclude:: example.py
      :encoding: latin-1
   ```

   该指令还支持仅包含文件的一部分。如果它是一个Python模块, 你可以选择一个类, 函数或方法来包含使用 `pyobject` 选项:

   ```rst
   .. literalinclude:: example.py
      :pyobject: Timer.start
   ```

   这只包括属于文件中 `Timer` 类中 `start()` 方法的代码行。

   或者, 您可以通过给出 `lines` 选项来准确指定要包含的行:

   ```rst
   .. literalinclude:: example.py
      :lines: 1,3,5-10,20-
   ```

   这包括第1,3,5到10行和第20行到最后一行。

   另一种控制文件的哪一部分被包括的方法是使用 `start-after` 和 `end-before` 选项（或只使用其中一个）。如果 `start-after` 是一个字符串选项，那么只有包含该字符串的第一行之后的行被包括在内。如果 `end-before` 是一个字符串选项，那么只有包含该字符串的第一行之前的行被包括在内。`start-at` 和 `end-at` 选项的行为方式类似，但包含匹配的字符串的行会被包括在内。

   `start-after/start-at` 和 `end-before/end-at` 可以有相同的字符串。`start-after/start-at` 过滤包含选项字符串的行之前的行（`start-at` 将保留该行）。然后，`end-before/end-at` 过滤包含选项字符串的行之后的行（`end-at` 将保留该行，`end-before` 跳过第一行）。

   ````{note}
   如果你想只选择 ini 文件的 `[second-section]`，像下面这样，你可以使用 `:start-at: [second-section]` 和 `end-before: [third-section]`：

   ```rst
   [first-section]

   var_in_first=true

   [second-section]

   var_in_second=true

   [third-section]

   var_in_third=true
   ```

   这些选项的有用情况是与标签注释一起工作。`:start-after: [initialized]` 和 `:end-before: [initialized]` 选项在注释之间保留行：

   ```rst
   if __name__ == "__main__":
      # [initialize]
      app.start(":8000")
      # [initialize]
   ```
   ````

   使用 `start-after` 选择的行仍然可以使用 `lines`，第一个允许的行按照惯例使用行号 `1` 。

   当以上述任何方式选择行时, “强调行” 中的行号指的是从 “1” 开始连续计数的所选行。

   指定要显示的文件的特定部分时, 显示原始行号可能很有用。这可以使用 `lineno-match` 选项来完成, 但是只有当选择包含连续的行时才允许这样做。

   您可以分别使用 `prepend` 和 `append` 选项为所包含的代码添加和/或附加一行。这很有用, 例如用于突出显示不包含 `<？php`/`？>` 标记的PHP代码。

   如果要显示代码的差异, 可以通过给出 `diff` 选项来指定旧文件:

   ```rst
   .. literalinclude:: example.py
      :diff: example.py.orig
   ```

   这显示了 `example.py` 和 `example.py.orig` 之间的差异, 统一的diff格式。

   **在 0.4.3 版更改:** 添加了 `encoding` 选项。

   **在 0.6 版更改:** 添加了 `pyobject`, `lines`, `start-after` 和 `end-before` 选项, 以及对绝对文件名的支持。

   **在 1.0 版更改:** 添加了 `prepend`, `append` 和 `tab-width` 选项。

   **在 1.3 版更改:** 添加了 `diff`, `lineno-match`, `caption`, `name` 和 `dedent` 选项。

   **在 1.5 版更改:** 添加了 `start-at` 和 `end-at` 选项。

   **在 1.6 版更改:** 使用 `start-after` 和 `lines` 时, 第一行按照 `start-after` 被认为是 `lines` 的行号 `1` 。

## 术语

`` .. glossary:: ``
:  该指令必须包含带有术语和定义的 reST 定义列表标记。然后, 这些定义可以引用 [`term`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html#role-term "term role") 角色。例：

   ````{panels}
   :container: w3-card-4 w3-pale-green w3-padding
   :column: col-lg-6 px-2 py-2
   ---
   :header: w3-pale-blue
   RST
   ^^^
   ```rst
   .. glossary::

      environment
         A structure where information about all documents under the root is
         saved, and used for cross-referencing.  The environment is pickled
         after the parsing stage, so that successive runs only need to read
         and parse new and changed documents.

      source directory
         The directory which, including its subdirectories, contains all
         source files for one Sphinx project.
   ```
   ---
   :header: w3-pale-red
   渲染：
   ^^^
   ```{eval-rst}
   .. glossary::

      environment
         A structure where information about all documents under the root is
         saved, and used for cross-referencing.  The environment is pickled
         after the parsing stage, so that successive runs only need to read
         and parse new and changed documents.

      source directory
         The directory which, including its subdirectories, contains all
         source files for one Sphinx project.
   ```
   ````

   与常规定义列表相比, 每个条目允许 *multiple* 术语, 并且允许使用内联标记。您可以链接到所有条款。例如：

   ````{panels}
   :container: w3-card-4 w3-pale-green w3-padding
   :column: col-lg-6 px-2 py-2
   ---
   :header: w3-pale-blue
   RST
   ^^^
   ```rst
   .. glossary::

      term 1
      term 2
         Definition of both terms.
   ```
   ---
   :header: w3-pale-red
   渲染：
   ^^^
   ```{eval-rst}
   .. glossary::

      term 1
      term 2
         Definition of both terms.
   ```
   ````

   (术语表排序时, 第一个术语确定排序顺序。)

   如果要为一般索引条目指定 “分组键”, 可以将 “键” 设置为 “term : key”。例如：

   ```rst
   .. glossary::

      term 1 : A
      term 2 : B
         Definition of both terms.
   ```

   请注意, “key” 用于按键分组键。 “key” 未正常化; 键 “A” 和 “a” 成为不同的组。使用 “key” 中的整个字符代替第一个字符;它用于 “组合字符序列” 和 “代理对” 分组键。

   在i18n情况下, 即使原始文本只有 “term” 部分, 也可以指定 “本地化 term : key”。在这种情况下, 翻译的 “本地化术语” 将分类在 “密钥” 组中。

   **0.6 新版功能:** 您现在可以为glossary指令提供一个 `:sorted:` 标志, 该标志将按字母顺序自动对条目进行排序。

   **在 1.1 版更改:** 现在支持多个术语和内联标记。

   **在 1.4 版更改:** 词汇表术语的索引键应视为 *experimental*。

## 元信息标记

`` .. sectionauthor:: name <email> ``
:  标识当前部分的作者。该论点应包括作者的姓名, 以便它可用于演示文稿和电子邮件地址。地址的域名部分应为小写。例：

   ```rst
   .. sectionauthor:: Guido van Rossum <guido@python.org>
   ```

   默认情况下, 此标记不会以任何方式反映在输出中(它有助于跟踪贡献), 但您可以将配置值 [`show_authors`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-show_authors) 设置为 `True` 以使它们生成一个段落输出。

`` .. codeauthor:: name <email> ``
:  [`codeauthor`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-codeauthor "codeauthor directive") 指令, 可以多次出现, 命名所描述代码的作者, 就像 [`sectionauthor`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-sectionauthor "sectionauthor directive") 命名一篇文档的作者。如果 [`show_authors`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-show_authors) 配置值为 “True”，它也只产生输出。

## 索引生成标记

Sphinx 自动从所有对象描述（如函数, 类或属性）创建索引条目, 如 [域](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html) 中所述。

但是, 还有明确的标记可用, 以使索引更加全面, 并在文档中启用索引条目, 其中信息不主要包含在信息单元中, 例如语言参考。

`` .. index:: <entries> ``
:  该指令包含一个或多个索引条目。每个条目由一个类型和一个值组成, 用冒号分隔。

   例如:

   ```rst
   .. index::
      single: execution; context
      module: __main__
      module: sys
      triple: module; search; path

   The execution context
   ---------------------

   ...
   ```

   该指令包含五个条目, 这些条目将转换为生成的索引中的条目, 该条目链接到索引语句的确切位置（或者, 如果是脱机介质, 则是相应的页码）。

   由于索引指令在源中的位置生成交叉引用目标, 因此将它们*放在它们引用的东西之前是有意义的 - 例如标题, 如上例所示。

   可能的条目类型是:

   `single`
   :  创建单个索引条目。可以通过用分号分隔子条目文本来创建子条目（下面也使用此符号来描述创建的条目）。

   `pair`
   :  `pair: loop; statement` 是一个创建两个索引条目的快捷方式, 即 `loop; statement` 和 `statement; loop`。

   `triple`
   :  同样, `triple: module; search; path` 是一个创建三个索引条目的快捷方式, 它们是 `module; search path`，`search; path, module`  和 `path; module search`。

   `see` 
   :  `see: entry; other` 创建一个索引条目, 从 `entry` 引用到 `other`。

   `seealso`
   :  就像 `see`，但是插入 “see also” 而不是 “see”。

   `module, keyword, operator, object, exception, statement, builtin`
   :  这些都创建了两个索引条目。例如, `module:hashlib` 创建条目 `module; hashlib` 和 `hashlib; module.` 。（这些是特定于 Python 的, 因此已弃用。）

   您可以通过在前面添加感叹号来标记 “main” 索引条目。生成的索引中强调了对 “main” 条目的引用。例如, 如果两个页面包含：

   ```rst
   .. index:: Python
   ```

   和一页包含:

   ```rst
   .. index:: ! Python
   ```

   然后在三个反向链接中强调后一页面的反向链接。

   对于仅包含 “single” 条目的索引指令, 有一种简写符号:

   ```rst
   .. index:: BNF, grammar, syntax, notation
   ```

   这将创建四个索引条目。

   **在 1.1 版更改:** 添加了 `see` 和 `seealso` 类型, 以及标记主条目。

   **选项**

   `:name:` a label for hyperlink (text)
   :  定义隐含的目标名称，可以通过使用 [ref](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html#role-ref) 来引用。比如说：

   ```rst
   .. index:: Python
   :name: py-index
   ```

`` :index: ``
:  虽然 [`index`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-index "index directive") 指令是块级标记并链接到下一段的开头, 但还有一个相应的角色可以直接设置链接目标的使用位置。

   角色的内容可以是一个简单的短语, 然后保存在文本中并用作索引条目。它也可以是文本和索引条目的组合, 其样式与交叉引用的显式目标类似。在这种情况下, “target ” 部分可以是完整条目, 如上面的指令所述。例如：

   ```rst
   This is a normal reST :index:`paragraph` that contains several
   :index:`index entries <pair: index; entry>`.
   ```

   **1.1 新版功能.**

## 引用基于标签的内容

`` .. only:: <expression> ``
:  仅当 *expression* 为 true 时才包含指令的内容。表达式应该包含标签, 例如：

   ```rst
   .. only:: html and draft
   ```

   未定义的标签是假的, 定义的标签(通过 `-t` 命令行选项或在 `conf.py` 中, 参见 [here](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#conf-tags))是真的。支持布尔表达式, 也使用括号(如 `html` 和 (`latex` or `draft`))。

   *format* 和当前构建器的 *name* (`html`, `latex` 或 `text`)始终设置为标签 [^4]。为了明确区分格式和名称, 它们还添加了前缀 `format_` 和 `builder_`，例如: epub 构建器定义了标签 `html`，`epub`，`format_html` 和 `builder_epub` 。

   读取配置文件 *后*，将设置这些标准标记, 因此它们不可用。

   所有标记必须遵循标识符语法, 如 [标识符和关键字](https://docs.python.org/3/reference/lexical_analysis.html#identifiers) 文档中所述。即, 标记表达式可能只包含标记符合Python变量的语法。在ASCII中, 它由大写和小写字母 `A` 到 `Z` 和下划线 `_` 组成, 除了第一个字符可以使用数字 `0` 到 `9`。

   **0.6 新版功能.**

   **在 1.2 版更改:** 添加了构建器的名称和前缀。

```{warning}
该指令旨在仅控制文档的内容。它无法控制部分, 标签等。
```

## 表格

使用 [reStructuredText tables](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/basics.html#rst-tables)，即

* 网格表语法 ([ref](http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#grid-tables)),
* 简单的表格语法 ([ref](http://docutils.sourceforge.net/docs/ref/rst/restructuredtext.html#simple-tables)),
* [csv-table](http://docutils.sourceforge.net/docs/ref/rst/directives.html#csv-table) 句法,
* 或者 [list-table](http://docutils.sourceforge.net/docs/ref/rst/directives.html#list-table) 语法。

[table](http://docutils.sourceforge.net/docs/ref/rst/directives.html#table) 指令作为 *grid* 和 *simple* 语法的可选包装。

它们在HTML输出中工作正常, 但是在LaTeX中使用表格时会有一些问题:列宽很难自动正确确定。因此, 存在以下指令:

`` .. tabularcolumns:: column spec ``
:  该指令为源文件中出现的下一个表提供了 “column spec” 。规范是LaTeX “tabulary” 包的环境的第二个参数(Sphinx用它来翻译表)。它可以具有:

   ```rst
   |l|l|l|
   ```

   这意味着三个左调整, 不间断的列。对于应该自动断开的具有较长文本的列, 请使用标准的 `p{width}` 构造或tabulary的自动说明符:

   | `L` | 用自动宽度冲洗左列 |
   | ------------------------------ | ------------------ |
   | `R` | 用自动宽度冲洗右列 |
   | `C` | 中心列, 自动宽度   |
   | `J` | 对齐自动宽度的列   |

   `LRCJ` 列的自动宽度由 `tabulary` 与第一遍中观察到的份额成比例, 其中表格单元格以其自然的 “horizontal” 宽度呈现。

   默认情况下, Sphinx为每列使用带有 `J` 的表格布局。

   **0.3 新版功能.**

   **在 1.6 版更改:** 由于自定义Sphinx LaTeX宏, 合并的单元格现在可以包含多个段落并且处理得更好。这种新颖的情况促使人们默认切换到 `J` 说明符而不是 `L` 。

   ```{hint}
   Sphinx 实际上使用 `T` 说明符来完成 `\newcolumntype{T}{J}` 。要恢复到以前的默认值, 请在LaTeX前导码中插入 `\newcolumntype{T}{L}` (参见 [`latex_elements`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-latex_elements) )。

   tabulary 的一个常见问题是内容很少的列被 “squeezed” 。最小列宽是一个名为 `\tymin` 的表格参数。您可以通过 `\setlength{\tymin}{40pt}` 在 LaTeX 前导码中全局设置它。

   否则, 使用 [`tabularcolumns`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-tabularcolumns "tabularcolumns directive") 指令, 对该列使用显式的 `p{40pt}` (例如)。您也可以使用 `l` 说明符, 但如果某个合并单元格与该列相交, 则会使设置列宽的任务更加困难。
   ```

   ```{warning}
   使用 `longtable` 而不是 `tabulary` 呈现超过30行的表, 以允许分页。 `L`，`R`，… 说明符不适用于这些表。

   包含类似列表的元素(如对象描述, 块引用或任何类型的列表)的表不能使用 `tabulary` 开箱即用。因此, 如果你没有给出 `tabularcolumns` 指令, 它们就会被设置为标准的LaTeX `tabular` (或 `longtable`)环境。如果你这样做, 表格将设置为 `tabulary`，但你必须使用 `p{width}` 构造(或者下面描述的Sphinx的 `\X` 和 `\Y` 说明符) )包含这些元素的列。

   文字块根本不适用于 `tabulary`，因此包含文字块的表总是设置为 `tabular` 。用于文字块的逐字环境仅适用于 `p{width}` (和 `\X` 或 `\Y`)列, 因此Sphinx为包含文字块的表生成此类列规范。
   ```

   从 Sphinx 1.5 开始, 使用 `\X{a}{b}` 说明符(说明字母*中有*反斜杠)。它就像 `p{width}`，宽度设置为当前行宽的一小部分 `a/b` 。你可以在 [`tabularcolumns`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-tabularcolumns "tabularcolumns directive") 中使用它(如果某些LaTeX宏也被称为 `\X`，这不是问题。)

   它不需要 `b` 是列的总数，也不需要 `\X` 指定的分数之和加起来是 1。例如 `|\X{2}{5}|\X{1}{5}|\X{1}{5}|` 是合法的, 表格将占据线宽的80％，它的三个列中的第一个具有与下两个列的总和相同的宽度。

   这由 [table](http://docutils.sourceforge.net/docs/ref/rst/directives.html#table) 指令的 `:widths:` 选项使用。

   从 Sphinx 1.6 开始, 还有 `\Y{f}` 说明符允许一个十进制参数, 例如 `\Y{0.15}`: 这与 `\X{3}{20}` 的效果相同。

   **在 1.6 版更改:** 来自复杂网格表(多行, 多列或两者)的合并单元格现在允许块引用, 列表, 文字块, … 和常规单元格一样。

   Sphinx 的合并单元格与 `p{width}`, `\X{a}{b}`，`Y{f}` 和 tabulary 列完美交互。

   ```{note}
   [`tabularcolumns`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-tabularcolumns "tabularcolumns directive") 与表指令的 `:widths:` 选项冲突。如果两者都指定, 则 `:widths:` 选项将被忽略。
   ```

## 数学

数学的输入语言是LaTeX标记。这是纯文本数学符号的事实上的标准, 并且具有额外的优点, 即在构建LaTeX输出时不需要进一步的转换。

请记住, 当你将数学标记放在 **Python文档字符串** 中读取 [`autodoc`](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/autodoc.html#module-sphinx.ext.autodoc "sphinx.ext.autodoc: Include documentation from docstrings.") 时, 你要么必须加倍所有的反斜杠, 要么使用Python原始字符串(`r"raw"`)。

`` .. math:: ``
:  显示数学的指令(数学为自己的整行)。

   该指令支持多个方程式, 应该用空行分隔:

   ```rst
   .. math::

      (a + b)^2 = a^2 + 2ab + b^2

      (a - b)^2 = a^2 - 2ab + b^2
   ```

   另外, 每个单独的等式都设置在一个 `split` 环境中, 这意味着你可以在一个等式中有多个对齐的行, 在 `＆` 对齐并用 `\\`

   ```rst
   .. math::

      (a + b)^2  &=  (a + b)(a + b) \\
               &=  a^2 + 2ab + b^2
   ```

   有关更多详细信息, 请查看 [AmSMath LaTeX package](https://www.ams.org/publications/authors/tex/amslatex) 的文档。

   当数学只有一行文本时, 它也可以作为指令参数给出:

   ```rst
   .. math:: (a + b)^2 = a^2 + 2ab + b^2
   ```

   通常, 方程式没有编号。如果你想要你的方程式得到一个数字, 使用 `label` 选项。给定时, 它选择方程的内部标签, 通过它可以交叉引用, 并导致发出方程编号。请参阅 [`eq`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html#role-eq "eq role") 作为示例。编号样式取决于输出格式。

   还有一个选项 `nowrap` 可以防止在数学环境中包含给定的数学。当您提供此选项时, 您必须确保自己正确设置了数学运算。例如:

   ```rst
   .. math::
      :nowrap:

      \begin{eqnarray}
         y    & = & ax^2 + bx + c \\
         f(x) & = & x^2 + 2xy + y^2
      \end{eqnarray}
   ```


```{seealso}
[Sphinx中对HTML输出的数学支持](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/math.html#math-support)使用HTML构建器渲染数学选项。

[`latex_engine`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-latex_engine)说明如何配置LaTeX构建器以在数学标记中支持Unicode文字。
```

## 语法制作显示

特殊标记可用于显示正式语法的制作。标记很简单, 不会尝试对 BNF（或任何派生形式）的所有方面进行建模, 但提供的足够允许以一种方式显示无上下文的语法, 使得符号的使用呈现为定义的超链接符号。有这个指令：

`` .. productionlist:: [productionGroup] ``
:  这条指令是用来包围一组产品的。每个产品都在一行中给出，由一个名称组成，用一个冒号与下面的定义隔开。如果定义跨越多行，每一个延续行必须以冒号开始，并与第一行中的冒号放置在同一列。生产列表指令参数中不允许有空行。

   该定义可以包含标记为解释文本的标记名称(例如 `` “sum ::= `integer` "+" `integer`” ``) - 这会生成对这些标记的生成的交叉引用。在生产列表之外, 您可以使用以下命令引用令牌生产 [`token`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html#role-token "token role") 。

   [productionlist](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-productionlist) 的参数 `productionGroup` 用于区分属于不同语法的不同生产列表集。因此，具有相同生产组的多个生产列表在同一范围内定义规则。

   Inside of the production list, tokens implicitly refer to productions from the current group. You can refer to the production of another grammar by prefixing the token with its group name and a colon, e.g, “`otherGroup:sum`”. If the group of the token should not be shown in the production, it can be prefixed by a tilde, e.g., “~otherGroup:sum”. To refer to a production from an unnamed grammar, the token should be prefixed by a colon, e.g., “`:sum`”.

   Outside of the production list, if you have given a productionGroup argument you must prefix the token name in the cross-reference with the group name and a colon, e.g., “`myGroup:sum`” instead of just “sum”. If the group should not be shown in the title of the link either an explicit title can be given (e.g., “`myTitle <myGroup:sum>`”), or the target can be prefixed with a tilde (e.g., “`~myGroup:sum`”).

   Note that no further reST parsing is done in the production, so that you don’t have to escape `*` or `|` characters.

   The following is an example taken from the Python Reference Manual:

   ```rst
   .. productionlist::
      try_stmt: try1_stmt | try2_stmt
      try1_stmt: "try" ":" `suite`
               : ("except" [`expression` ["," `target`]] ":" `suite`)+
               : ["else" ":" `suite`]
               : ["finally" ":" `suite`]
      try2_stmt: "try" ":" `suite`
               : "finally" ":" `suite`
   ```

[^1]: LaTeX 编写器只引用文档中第一个 toctree 指令的 `maxdepth` 选项。

[^2]:  关于可用的globbing语法的注释:你可以使用标准shell构造 `*`, `?`, `[...]` 和 `[!...]` 这些都不匹配斜杠。双星 `**` 可用于匹配任何字符序列 *including* 斜杠。

[^3]: 有一个标准的 `.. include` 指令, 但如果找不到该文件, 则会引发错误。这只发出警告。

[^4]: 对于大多数构建器, 名称和格式是相同的。目前, 只有从html构建器派生的构建器才能区分构建器格式和构建器名称。

      请注意, 当前构建器标记在 `conf.py` 中不可用, 它仅在构建器初始化后可用。
