(rst:roles)=
# 角色

参考：[角色](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html)

Sphinx 使用解释型文本角色将语义标记插入到文档中。它们被写成：

```rst
:rolename:`content`
```

```{note}
默认的角色（`` `content` ``）在默认情况下没有特殊含义。你可以自由地将它用于任何你喜欢的地方，例如变量名；使用 [default_role](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-default_role) 配置值将它设置为一个已知的角色 - [`:any:`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html#role-any) 角色用于查找任何东西，或者 [`py:obj`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-py-obj) 角色用于查找 Python 对象，在这方面非常有用。
```

请参阅 [域](rst:domains)，了解域添加的角色。

## 交叉引用语法

交叉引用由许多语义解释的文本角色生成。基本上，您只需要编写 `` :role:`target` ``，并且将为 *role* 指示的类型 *target* 项创建一个链接。链接的文本将与 *target* 相同。

但是，还有一些额外的功能可以使交叉引用角色更加通用：

* 你可以提供一个明确的标题和引用目标，比如在 reST 直接超链接中: `` :role:`title <target>` `` 将引用 *target* ，但是链接文本将是 *title*。
* 如果在内容前加上 `!` ，则不会创建引用/超链接。
* 如果在内容前面添加 `~`，则链接文本将只是目标的最后一个组件。例如，`` :py:meth:`~Queue.Queue.get` `` 将引用 `Queue.Queue.get` 但仅显示 `get` 作为链接文本。这不适用于所有交叉引用角色，但是特定于域。

  在 HTML 输出中，链接的 `title` 属性（例如在鼠标悬停时显示为工具提示）将始终是完整的目标名称。

### 交叉引用任何东西 `:any:`

这个便利角色试图尽力为其引用文本找到有效的目标。

* 首先，它尝试引用的标准交叉引用目标 [`doc`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html#role-doc "doc role")，[`ref`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html#role-ref "ref role") 或 [`option`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html#role-option "option role") 。

  还会搜索通过扩展添加到标准域的自定义对象(请参阅 [`Sphinx.add_object_type()`](https://www.sphinx-doc.org/zh_CN/master/extdev/appapi.html#sphinx.application.Sphinx.add_object_type "sphinx.application.Sphinx.add_object_type") )。

* 然后，它在所有加载的域中查找对象(目标)。这取决于匹配必须具体的域。例如，在 Python 域中，`` :any:`Builder` `` 的引用将匹配 `sphinx.builders.Builder` 类。

如果未找到任何目标或多个目标，则会发出警告。对于多个目标，您可以将 “any” 更改为特定角色。

这个角色是设置 [`default_role`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-default_role) 的良好候选者。如果你这样做，你就可以在没有大量标记开销的情况下编写交叉引用。例如，在这个 Python 函数文档中：

```rst
.. function:: install()

   This function installs a `handler` for every signal known by the
   `signal` module.  See the section `about-signals` for more information.
```

可以引用词汇表术语(通常是 `` :term:`handler` `` )，一个 Python 模块（通常是 `` :py:mod:`signal` `` 或 `` :mod:`signal` ``）和一节（通常是 `` :ref:`about-signals` ``）。

[`any`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html#role-any "any role") 角色也与 [`intersphinx`](https://www.sphinx-doc.org/zh_CN/master/usage/extensions/intersphinx.html#module-sphinx.ext.intersphinx "sphinx.ext.intersphinx: Link to other Sphinx documentation.") 扩展一起使用：当没有找到本地交叉引用时，也会搜索所有对象类型的 intersphinx 库存。

### 交叉引用对象

这些角色用各自的域描述：

* [Python](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#python-roles)
* [C](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#c-roles)
* [C++](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#cpp-roles)
* [JavaScript](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#js-roles)
* [ReST](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#rst-roles)

### 交叉引用任意位置 `:ref:`

为了支持对任何文档中任意位置的交叉引用，使用标准 reST 标签。为此，标签名称在整个文档中必须是唯一的。您可以通过两种方式引用标签：

* 如果在标题之前直接放置标签，可以用 `` :ref:`label-name` `` 引用它。例如:

  ```rst
  .. _my-reference-label:

  Section to cross-reference
  --------------------------

  This is the text of the section.

  It refers to the section itself, see :ref:`my-reference-label`.
  ```

  然后 `:ref:` 角色将生成一个指向该部分的链接，链接标题为 “Section to cross-reference”。当 section 和 reference 在不同的源文件中时，这也可以正常工作。
  自动标签也适用于图像。例如：

  ```rst
  .. _my-figure:

  .. figure:: whatever

     Figure caption
  ```

  在这种情况下，引用 `` :ref:`my-figure` `` 将插入带有链接文本”Figure caption” 的图形的引用。

  对于使用 [table](https://docutils.sourceforge.io/docs/ref/rst/directives.html#table) 指令给出显式标题的表，同样适用。

* 仍未引用未放置在节标题之前的标签，但您必须使用以下语法为链接指定显式标题: `` :ref:`Link title <label-name>` ``。

```{note}
引用标签必须以下划线开头。引用标签时，必须省略下划线（参见上面的示例）。
```

使用 [`ref`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html#role-ref "ref role") 被建议通过标准的 reStructuredText 链接到部分（比如 `` `Section title`_ ``），因为它适用于文件，当部分标题发生变化时，如果不正确则会发出警告，并且适用于所有支持交叉引用的构建器。

### 交叉引用文档 `:doc:`

还有一种方法可以直接链接到文档，文档名称可以绝对或相对方式指定。例如，如果引用 `` :doc:`parrot` `` 出现在文档 `sketches/index` 中，则链接引用 `sketches/parrot`。 如果引用是 `` :doc:`/people` `` 或 `` :doc:`../people` ``，则链接引用 `people`。

如果没有给出明确的链接文本（比如通常: `` :doc:`Monty Python members </people>` ``），链接标题将是给定文档的标题。

### 引用可下载文件 `:download:`

此角色允许您链接到源树中的文件，这些文件不是可以查看的 reST 文档，而是可以下载的文件。

使用此角色时，引用的文件会在构建时自动标记为包含在输出中（显然，仅适用于 HTML 输出）。所有可下载的文件都放在输出目录的 `_downloads` 子目录中；处理重复的文件名。

一个例子:

```rst
See :download:`this example script <../example.py>`.
```

给定的文件名通常是相对于当前源文件所包含的目录，但如果它是绝对的（以 `/` 开头），则它被视为相对于顶级源目录。

`example.py` 文件将被复制到输出目录，并生成一个合适的链接。

不显示不可用的下载链接，您应该包含具有此角色的整个段落：

```rst
.. only:: builder_html

   See :download:`this example script <../example.py>`.
```

### 按图号交叉引用图像 `:numref:`

```{versionchanged} 1.5
`numref` 角色也可以引用段落。并且 `numref` 允许链接文本使用 `{name}`。
```

链接到指定的图像，表格，代码块和节；使用标准的 reST 标签。当您使用此角色时，它将插入带有链接文本的图形的引用，其图形编号如 “图1.1”。

如果给出了明确的链接文本（像往常一样: `` :numref:`Image of Sphinx (Fig. %s) <my-figure>` ``），链接标题将作为引用的标题。作为占位符，`%s` 和 {number} 被图形标题替换为图形编号和 `{name}`。如果没有给出明确的链接文本，则 [`numfig_format`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-numfig_format) 设置用作后备默认值。

如果 [`numfig`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-numfig) 是 `False`，数字没有编号，所以这个角色不插入引用，而是插入标签或链接文本。

### 交叉引用其他感兴趣的项目

以下角色可能会创建交叉引用，但不引用对象:

`:envvar:`
: 环境变量。生成索引条目。还会生成匹配的链接 [`envvar`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-envvar "envvar directive") 指令（如果存在）。

`:token:`
: 语法标记的名称（用于创建 [`productionlist`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#directive-productionlist "productionlist directive") 指令之间的链接）。

`:keyword:`
: Python 中关键字的名称。这将创建一个指向具有该名称的引用标签的链接（如果存在）。

`:option:`
: 可执行程序的命令行选项。这会生成一个指向 [`option`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-option "option directive") 指令的链接（如果存在）。

以下角色在以下内容中创建对术语的交叉引用 [glossary](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#glossary-directive) :

`:term:`
: 参考词汇表中的术语。使用 `glossary` 指令创建词汇表，该指令包含带有术语和定义的定义列表。它不必与 `term` 标记在同一个文件中，例如Python文档在 `glossary.rst` 文件中有一个全局词汇表。

如果您使用术语表中未解释的术语，您将在构建期间收到警告。

## Math

`:math:`
: 内联数学的作用。像这样使用：

```rst
Since Pythagoras, we know that :math:`a^2 + b^2 = c^2`.
```

`:eq:`
: 同样 [`math:numref`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-math-numref) 类似。

## 其他语义标记

除了以不同的样式格式化文本之外，以下角色不会做任何特殊操作:

`:abbr:`
: 缩写。如果角色内容包含带括号的解释，则将对其进行特殊处理:它将以HTML格式显示在工具提示中，并在 LaTeX 中仅输出一次。

  例: `` :abbr:`LIFO (last-in, first-out)` ``.

`:command:`
: 操作系统级命令的名称，例如 `rm`。

`:dfn:`
: 在文本中标记术语的定义实例。 (不生成索引条目。)

`:file:`
: 文件或目录的名称。在内容中，您可以使用花括号来表示 “变量” 部分，例如：

  ```rst
  ... is installed in :file:`/usr/lib/python2.{x}/site-packages` ...
  ```

  在构建的文档中， `x` 将以不同的方式显示，表示它将被 Python 次要版本替换。

`:guilabel:`
: 作为交互式用户界面的一部分呈现的标签应使用 “guilabel” 标记。这包括来自基于文本的界面的标签，例如使用 [`curses`](https://docs.python.org/3/library/curses.html#module-curses) 或其他基于文本的库创建的界面。界面中使用的任何标签都应标记为此角色，包括按钮标签，窗口标题，字段名称，菜单和菜单选择名称，甚至选择列表中的值。

  ```{versionchanged} 1.0
  GUI 标签的快捷键可以用 `&`（ampersand） 来包含；这将被剥离并在输出中显示为下划线（例如：`` :guilabel:`&Cancel` ``）。要包括一个字段的 `&`，请将其加倍。
  ```

`:kbd:`
: 标记一系列击键。密钥序列采用何种形式可能取决于平台或特定于应用程序的约定。如果没有相关约定，则应拼写修饰键的名称，以提高新用户和非母语人士的可访问性。例如，*xemacs* 键序列可以标记为 `` :kbd:`C-x C-f` ``，但是没有引用特定的应用程序或平台，相同的序列应该标记为 `` :kbd:`Control-x Control-f` ``（渲染：{kbd}`Control-x Control-f`）。

`:mailheader:`
: RFC 822 样式邮件头的名称。此标记并不意味着标题正在电子邮件消息中使用，但可用于引用相同 “样式” 的任何标题。这也用于各种 MIME 规范定义的标头。标题名称的输入方式应与通常在实践中找到的方式相同，在有多种常用用法的情况下，首选的是驼峰套管约定。例如: `` :mailheader:`Content-Type` ``。

`:makevar:`
: **make** 变量的名称。

`:manpage:`
: 对 Unix 手册页的引用，包括例如 `` :manpage:`ls(1)` ``。如果 [`manpages_url`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-manpages_url) 已定义，则创建指向外部站点的超链接，呈现联机帮助页。

`:menuselection:`
: 应使用 `menuselection` 角色标记菜单选择。这用于标记完整的菜单选择序列，包括选择子菜单和选择特定操作，或此类序列的任何子序列。各个选择的名称应该用 `-->` 分隔。

  例如，要标记选择 “{menuselection}`Start --> Programs`”，请使用此标记：

  ```rst
  :menuselection:`Start --> Programs`
  ```

  当包括一些包含一些尾随指示符的选择时，例如某些操作系统用来指示命令打开对话框的省略号时，应从选择名称中省略该指示符。

  `menuselection` 也支持＆符快捷键，如：[`guilabel`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html#role-guilabel "guilabel role")。

`:mimetype:`
: MIME 类型的名称，或 MIME 类型的组件（主要或次要部分，单独使用）。

`:newsgroup:`
: Usenet 新闻组的名称。

`:program:`
: 可执行程序的名称。这可能与某些平台的可执行文件的文件名不同。特别是，对于Windows程序，应省略 `.exe`（或其他）扩展名。

`:regexp:`
: 正则表达式。Quotes 不应包括在内。

`:samp:`
: 一段文字文本，例如代码。在内容中，您可以使用花括号来表示 “变量” 部分，如 [`file`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html#role-file "file role") 。例如，在 `` :samp:`print 1+{variable}` `` 中，将强调部分 `variable`。

  如果您不需要 “variable 部分” 指示，请使用标准的 ` ``code``  ` 。

  ```{versionchanged} 1.8
  允许用反斜杠转义花括号
  ```

还有一个 [`index`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/directives.html#role-index "index role") 角色来生成索引条目。

以下角色生成外部链接：

`:pep:`
: 对 Python Enhancement Proposal 的引用。这会生成适当的索引条目。生成文本 “PEP  *number* ” ;在HTML输出中，此文本是指向指定PEP的联机副本的超链接。你可以通过说 `` :pep:`number#anchor` `` 链接到特定的部分。

`:rfc:`
: 对 Internet 请求注释的引用。这会生成适当的索引条目。生成文本 “RFC  *number* ” ; 在HTML输出中，此文本是指向指定RFC的联机副本的超链接。你可以通过说 `` :rfc:`number#anchor` `` 链接到特定的部分。

请注意，包含超链接没有特殊角色，因为您可以使用标准 reST 标记来实现此目的。

## 替换

文档系统提供默认定义的三个替换。它们在构建配置文件中设置。

`|release|`
: 替换为项目发布的文档引用。这意味着是完整版本字符串，包括 alpha/beta/release 候选标签，例如 `2.5.2b3`。设置 [`release`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-release) 。

`|version|`
: 由文档引用的项目版本替换。这仅仅包括主要和次要版本部分，例如 ``2.5``，即使是版本 `2.5.1`。设置方 [`version`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-version) 。

`|today|`
: 替换为今天的日期（读取文档的日期）或构建配置文件中设置的日期。通常格式为 `April 14, 2007`。设置方式 [`today_fmt`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-today_fmt) 和 [`today`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-today)。
