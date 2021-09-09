# reStructuredText 快速上手

参考：[reStructuredText Primer — Sphinx documentation (sphinx-doc.org)](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html) & [reStructuredText Markup Specification](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html)

reStructuredText 是 Sphinx 使用的默认纯文本标记语言。本节是对 reStructuredText(reST) 概念和语法的简要介绍，旨在为作者提供足够的信息来有效地编写文档。由于 reST 被设计成一种简单的、不显眼的标记语言，这不会花费太多时间。下面介绍 Doctree Body 元素。

## 段落

段落（[ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#paragraphs)）是 reST 文档中最基本的块。段落只是由一个或多个空行分隔的文本块。与在 Python 中一样，缩进在 reST 中很重要，因此同一段落中的所有行必须左对齐到相同的缩进级别。

````{panels}
:container: w3-card-4 w3-pale-green w3-padding
:column: col-lg-6 px-2 py-2
---
:header: w3-pale-blue
RST
^^^
```rst
This is a paragraph.  It's quite
short.

   This paragraph will result in an indented block of
   text, typically used for quoting other text.

This is another one.
```
---
:header: w3-pale-red
渲染：
^^^
```{eval-rst}
This is a paragraph.  It's quite
short.

   This paragraph will result in an indented block of
   text, typically used for quoting other text.

This is another one.
```
````

## 块引用

````{panels}
:container: w3-card-4 w3-pale-green w3-padding
:column: col-lg-6 px-2 py-2
---
:header: w3-pale-blue
RST
^^^
```rst
Block quotes consist of indented body elements:

    This theory, that is mine, is mine.

    -- Anne Elk (Miss)
```
---
:header: w3-pale-red
渲染：
^^^
```{eval-rst}
Block quotes consist of indented body elements:

    This theory, that is mine, is mine.

    -- Anne Elk (Miss)
```
````

## 行内标记

标准的 reST 行内标记非常简单：

* 一个星号: `*text*` 用于 emphasis（*斜体*），
* 两个星号: `**text**` 用于 strong emphasis（**粗体**）
* 反引号: ` ``text`` ` 代码示例。

如果星号或反引号出现在正在运行的文本中并且可能与行内标记分隔符混淆，则必须使用反斜杠对其进行转义。

请注意此标记的一些限制:

- 它不能嵌套，
- 内容无法以空格开头或结尾：如 `* text*` 是错误的
- 必须通过非单词字符将其与周围文本分开。使用反斜杠转义空格来解决这个问题：`` thisis\ *one*\ word ``。

在将来的 docutils 版本中可能会取消这些限制。

也可以使用 role 替换或扩展此行内标记。有关更多信息，请参阅 [角色](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#rst-roles-alt)。

## 列表和引用样块

### 普通列表

列表标记（[ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#bullet-lists)）是很自然的：只需在段落的开头放置一个星号并适当缩进。编号列表也是如此，它们也可以用 `#.` 号自动编号。

````{panels}
:container: w3-card-4 w3-pale-green w3-padding
:column: col-lg-6 px-2 py-2
---
:header: w3-pale-blue
RST
^^^
```rst
* This is a bulleted list.
* It has two items, the second
  item uses two lines.

1. This is a numbered list.
2. It has two items too.

#. This is a numbered list.
#. It has two items too.
```
---
:header: w3-pale-red
渲染：
^^^
```{eval-rst}
* This is a bulleted list.
* It has two items, the second
  item uses two lines.

1. This is a numbered list.
2. It has two items too.

#. This is a numbered list.
#. It has two items too.
```
````

嵌套列表是可能的，但要注意，它们必须用空行与父级列表项分开。

````{panels}
:container: w3-card-4 w3-pale-green w3-padding
:column: col-lg-6 px-2 py-2
---
:header: w3-pale-blue
RST
^^^
```rst
* this is
* a list

  * with a nested list
  * and some subitems

* and here the parent list continues
```
---
:header: w3-pale-red
渲染：
^^^
```{eval-rst}
* this is
* a list

  * with a nested list
  * and some subitems

* and here the parent list continues
```
````

### 定义列表

定义列表（[ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#definition-lists)）的创建方法如下。

````{panels}
:container: w3-card-4 w3-pale-green w3-padding
:column: col-lg-6 px-2 py-2
---
:header: w3-pale-blue
RST
^^^
```rst
term (up to a line of text)
   Definition of the term, which must be indented

   and can even consist of multiple paragraphs

next term
   Description.
```
---
:header: w3-pale-red
渲染：
^^^
```{eval-rst}
term (up to a line of text)
   Definition of the term, which must be indented

   and can even consist of multiple paragraphs

next term
   Description.
```
````

请注意，术语不能包含多行文本。

引用的段落([ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#block-quotes))只是通过缩进它们而不是周围的段落来创建。

行块([ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#line-blocks))是一种保留换行符的方法：

````{panels}
:container: w3-card-4 w3-pale-green w3-padding
:column: col-lg-6 px-2 py-2
---
:header: w3-pale-blue
RST
^^^
```rst
| These lines are
| broken exactly like in
| the source file.
```
---
:header: w3-pale-red
渲染：
^^^
```{eval-rst}
| These lines are
| broken exactly like in
| the source file.
```
````

(rst:field-lists)=
### 字段列表

Doctree 元素：[field_list](https://docutils.sourceforge.io/docs/ref/doctree.html#field-list), [field](https://docutils.sourceforge.io/docs/ref/doctree.html#field), [field_name](https://docutils.sourceforge.io/docs/ref/doctree.html#field-name), [field_body](https://docutils.sourceforge.io/docs/ref/doctree.html#field-body).

字段列表（Field Lists，[ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#field-lists)）被用作扩展语法的一部分，如指令的选项，或类似数据库的记录（records），用于进一步处理。它们也可用于类似于数据库记录的两列表结构（标签和数据对）。reStructuredText 的应用可以在某些情况下识别字段名并转换字段或字段体。例子见下面的[书目字段](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#toc-entry-18)，或 [reStructuredText 指令](https://docutils.sourceforge.io/docs/ref/rst/directives.html) 中的 "[image](https://docutils.sourceforge.io/docs/ref/rst/directives.html#image)" 和 "[meta](https://docutils.sourceforge.io/docs/ref/rst/directives.html#metadata)" 指令。

字段列表是像这样标记的字段序列：

```rst
:fieldname: Field content
```

字段列表是字段名到字段体的映射，以 [RFC822](http://www.rfc-editor.org/rfc/rfc822.txt) 头文件为模型。字段名可以由任何字符组成，但字段名内的冒号（":"）在后面有空白时必须进行反斜线转义。内联标记在字段名中被解析，但在字段名中使用带有明确角色的[解释文本](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#interpreted-text)时必须注意：角色必须是解释文本的后缀。在进一步处理或转换时，字段名是不分大小写的。字段名与单冒号前缀和后缀一起构成字段标记。字段标记之后是空白和字段主体。字段主体可以包含多个主体元素，相对于字段标记缩进。字段名标记后的第一行决定了字段主体的缩进。比如说：

````{panels}
:container: w3-card-4 w3-pale-green w3-padding
:column: col-lg-6 px-2 py-2
---
:header: w3-pale-blue
RST
^^^
```rst
:Date: 2001-08-16
:Version: 1
:Authors: - Me
          - Myself
          - I
:Indentation: Since the field marker may be quite long, the second
   and subsequent lines of the field body do not have to line up
   with the first line, but they must be indented relative to the
   field name marker, and they must line up with each other.
:Parameter i: integer
```
---
:header: w3-pale-red
渲染：
^^^
```{eval-rst}
:Date: 2001-08-16
:Version: 1
:Authors: - Me
          - Myself
          - I
:Indentation: Since the field marker may be quite long, the second
   and subsequent lines of the field body do not have to line up
   with the first line, but they must be indented relative to the
   field name marker, and they must line up with each other.
:Parameter i: integer
```
````

多字段名中各个字的解释由应用程序决定。应用程序可以为字段名指定一种语法。例如，第二个和随后的字可以被视为 "参数"，带引号的短语可以被视为一个单一的参数，并且可以增加对 "name=value" 语法的直接支持。

标准的 [RFC822](http://www.rfc-editor.org/rfc/rfc822.txt) 标题不能用于这个结构，因为它们是模糊的。在一个行的开头，一个词后面有一个冒号，这在书面文本中很常见。然而，在定义明确的情况下，例如当字段列表无一例外地出现在文档的开头时（PEP 和电子邮件），可以使用标准的 RFC822 头信息。

语法图（简化）：

```rst
+--------------------+----------------------+
| ":" field name ":" | field body           |
+-------+------------+                      |
        | (body elements)+                  |
        +-----------------------------------+
```

它们通常用于 Python 文档中：

```python
def my_function(my_arg, my_other_arg):
    """A function just for me.

    :param my_arg: The first of my arguments.
    :param my_other_arg: The second of my arguments.

    :returns: A message (just for me, of course).
    """
```

Sphinx 扩展了标准的 docutils 行为，并拦截了文档开头指定的字段列表。有关更多信息，请参阅 [Field Lists](https://www.sphinx-doc.org/en/master/usage/restructuredtext/field-lists.html)。

````{panels}
:container: w3-card-4 w3-pale-green w3-padding
:column: col-lg-6 px-2 py-2
---
:header: w3-pale-blue
RST
^^^
```rst
:what: Field lists map field names to field bodies, like
       database records.  They are often part of an extension
       syntax.

:how: The field marker is a colon, the field name, and a
      colon.

      The field body may contain one or more body elements,
      indented relative to the field marker.
```
---
:header: w3-pale-red
渲染：
^^^
```{eval-rst}
:what: Field lists map field names to field bodies, like
       database records.  They are often part of an extension
       syntax.

:how: The field marker is a colon, the field name, and a
      colon.

      The field body may contain one or more body elements,
      indented relative to the field marker.
```
````

#### 书目字段

Doctree 元素：[docinfo](https://docutils.sourceforge.io/docs/ref/doctree.html#docinfo), [address](https://docutils.sourceforge.io/docs/ref/doctree.html#address), [author](https://docutils.sourceforge.io/docs/ref/doctree.html#author), [authors](https://docutils.sourceforge.io/docs/ref/doctree.html#authors), [contact](https://docutils.sourceforge.io/docs/ref/doctree.html#contact), [copyright](https://docutils.sourceforge.io/docs/ref/doctree.html#copyright), [date](https://docutils.sourceforge.io/docs/ref/doctree.html#date), [organization](https://docutils.sourceforge.io/docs/ref/doctree.html#organization), [revision](https://docutils.sourceforge.io/docs/ref/doctree.html#revision), [status](https://docutils.sourceforge.io/docs/ref/doctree.html#status), [topic](https://docutils.sourceforge.io/docs/ref/doctree.html#topic), [version](https://docutils.sourceforge.io/docs/ref/doctree.html#version).

当一个字段列表是文档中的第一个元素（如果有的话，在文档标题之后）[^7]，它的字段可能被转化为文档书目数据。这种书目数据对应于一本书的正面内容，如标题页和版权页。

[^7]: In additon to the document title and subtitle, also [comments](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#comments), [substitution definitions](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#substitution-definitions), [hyperlink targets](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#hyperlink-targets), and "header", "footer", "meta", and "raw" [directives](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#directives) may be placed before the bibliographic fields.

某些注册的字段名（列在下面）被识别并转化为相应的 doctree 元素，大多数成为 docinfo 元素的子元素。这些字段不需要排序，尽管它们可以被重新排列以适应文档结构，如前所述。除非下面另有说明，每个书目元素的字段体可以只包含一个段落。字段体可以被检查为 [RCS 关键词](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#rcs-keywords) 并被清理。任何未被识别的字段将作为通用字段保留在 docinfo 元素中。

已注册的书目字段名和它们相应的树状元素如下：

Field name |doctree element
:-|:-
  Abstract    |  [topic](https://docutils.sourceforge.io/docs/ref/doctree.html#topic)
  Address     |  [address](https://docutils.sourceforge.io/docs/ref/doctree.html#address)
  Author      |  [author](https://docutils.sourceforge.io/docs/ref/doctree.html#author)
  Authors      | [authors](https://docutils.sourceforge.io/docs/ref/doctree.html#authors)
  Contact      | [contact](https://docutils.sourceforge.io/docs/ref/doctree.html#contact)
  Copyright     |[copyright](https://docutils.sourceforge.io/docs/ref/doctree.html#copyright)
  Date          |[date](https://docutils.sourceforge.io/docs/ref/doctree.html#date)
  Dedication    |[topic](https://docutils.sourceforge.io/docs/ref/doctree.html#topic)
  Organization  |[organization](https://docutils.sourceforge.io/docs/ref/doctree.html#organization)
  Revision      |[revision](https://docutils.sourceforge.io/docs/ref/doctree.html#revision)
  Status       | [status](https://docutils.sourceforge.io/docs/ref/doctree.html#status)
  Version      | [version](https://docutils.sourceforge.io/docs/ref/doctree.html#version)

### 选项列表

Doctree 元素：[option_list](https://docutils.sourceforge.io/docs/ref/doctree.html#option-list), [option_list_item](https://docutils.sourceforge.io/docs/ref/doctree.html#option-list-item), [option_group](https://docutils.sourceforge.io/docs/ref/doctree.html#option-group), [option](https://docutils.sourceforge.io/docs/ref/doctree.html#option), [option_string](https://docutils.sourceforge.io/docs/ref/doctree.html#option-string), [option_argument](https://docutils.sourceforge.io/docs/ref/doctree.html#option-argument), [description](https://docutils.sourceforge.io/docs/ref/doctree.html#description).

[选项列表](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#option-lists) 是命令行选项和描述的两栏列表，记录了一个程序的选项。比如说：

````{panels}
:container: w3-card-4 w3-pale-green w3-padding
:column: col-lg-6 px-2 py-2
---
:header: w3-pale-blue
RST
^^^
```rst
-a            command-line option "a"
-b file       options can have arguments
              and long descriptions
--long        options can be long also
--input=file  long options can also have
              arguments
/V            DOS/VMS-style options too
```
---
:header: w3-pale-red
渲染：
^^^
```{eval-rst}
-a            command-line option "a"
-b file       options can have arguments
              and long descriptions
--long        options can be long also
--input=file  long options can also have
              arguments
/V            DOS/VMS-style options too
```
````

reStructuredText 识别的选项有几种类型：

- 短 POSIX 选项由一个破折号和一个选项字母组成。
- 长的 POSIX 选项由两个破折号和一个选项词组成；有些系统使用单破折号。
- 旧的 GNU 风格的 "加号 "选项由一个加号和一个选项字母组成（"加号 "选项现在已经废弃，不鼓励使用）。
- DOS/VMS 的选项由一个斜线和一个选项字母或单词组成。

请注意，POSIX 风格和 DOS/VMS 风格的选项都可以被 DOS 或 Windows 软件使用。这些和其他的变化有时会混在一起使用。上面的名称只是为了方便而选择的。

短和长 POSIX 选项的语法是基于 Python 的 [`getopt.py`](http://www.python.org/doc/current/lib/module-getopt.html) 模块所支持的语法，该模块实现了一个与 [GNU libc getopt_long()](http://www.gnu.org/software/libc/manual/html_node/Getopt-Long-Options.html) 函数类似的选项解析器，但有一些限制。有许多不同的选项系统，reStructuredText 的选项列表并不支持所有这些系统。

尽管在命令行上使用长的 POSIX 和 DOS/VMS 选项词时，操作系统或应用程序可能允许截断，但 reStructuredText 的选项列表并不显示或支持任何特殊的语法。应该给出完整的选项词，并在适用的情况下辅以截断的说明。

选项后面可以有一个参数占位符，其作用和语法应该在描述文本中解释。空格或等号可以作为选项和参数占位符之间的分隔符；简短的选项（只有"`-`"或 "`+`"前缀）可以省略分隔符。选项参数可以采取两种形式之一：

- 以字母（`[a-zA-Z]`）开头，随后由字母、数字、下划线和连字符组成（`[a-zA-Z0-9_-]`）。
- 以开角括号（`<`）开始，以闭角括号（`>`）结束；除开角括号外，内部允许任何字符。

可以列出多个选项 "同义词"，共享一个描述。它们必须用逗号分隔。

选项和描述之间必须至少有两个空格。描述可以包含多个主体元素。选项标记后的第一行决定了描述的缩进。与其他类型的列表一样，在第一个选项列表项之前和最后一个选项列表项之后必须有空行，但在选项条目之间是可选的。

简化语法图：

```rst
+----------------------------+-------------+
| option [" " argument] "  " | description |
+-------+--------------------+             |
        | (body elements)+                 |
        +----------------------------------+
```

## Literal 块

通过使用特殊标记 `::` 结束段落来引入文字代码块([ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#literal-blocks))。文字块必须缩进(和所有段落一样，用空行与周围的段分开)：

````{panels}
:container: w3-card-4 w3-pale-green w3-padding
:column: col-lg-12 px-2 py-2
---
:header: w3-pale-blue
RST
^^^
```rst
This is a normal text paragraph. The next paragraph is a code sample::

   It is not processed in any way, except
   that the indentation is removed.

   It can span multiple lines.

This is a normal text paragraph again.

Literal blocks are either indented or line-prefix-quoted blocks,
and indicated with a double-colon ("::") at the end of the
preceding paragraph (right here -->)::

    if literal_block:
        text = 'is left as-is'
        spaces_and_linebreaks = 'are preserved'
        markup_processing = None
```
---
:header: w3-pale-red
渲染：
^^^
```{eval-rst}
This is a normal text paragraph. The next paragraph is a code sample::

   It is not processed in any way, except
   that the indentation is removed.

   It can span multiple lines.

This is a normal text paragraph again.

Literal blocks are either indented or line-prefix-quoted blocks,
and indicated with a double-colon ("::") at the end of the
preceding paragraph (right here -->)::

    if literal_block:
        text = 'is left as-is'
        spaces_and_linebreaks = 'are preserved'
        markup_processing = None
```
````

处理 `::` 标记是聪明的：

- 如果它作为自己的段落出现，则该段落完全不在文档中。
- 如果前面有空格，则删除标记。
- 如果前面是非空格，则标记将替换为单个冒号。

这样，上面例子的第一段中的第二句将呈现为 “The next paragraph is a code sample:”。

可以使用 [highlight](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#directive-highlight) 指令在文档范围内为这些文字块启用代码突出显示，并在项目范围内使用 [highlight_language](https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-highlight_language) 配置选项。[code-block](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#directive-code-block) 指令可用于逐块设置突出显示。这些指令将在后面讨论。

## Doctest 块

Doctest 块([ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#doctest-blocks))是剪切并粘贴到文档字符串中的交互式 Python 会话。它们不需要 [literal blocks](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#rst-literal-blocks) 语法。doctest 块必须以空行结束，并且 不 以未使用的 prompt 结束：

````{panels}
:container: w3-card-4 w3-pale-green w3-padding
:column: col-lg-12 px-2 py-2
---
:header: w3-pale-blue
RST
^^^
```rst
>>> 1 + 1
2
>>> print('Python-specific usage examples; begun with ">>>"')
Python-specific usage examples; begun with ">>>"
>>> print('(cut and pasted from interactive Python sessions)')
(cut and pasted from interactive Python sessions)
```
---
:header: w3-pale-red
渲染：
^^^
```{eval-rst}
>>> 1 + 1
2
>>> print('Python-specific usage examples; begun with ">>>"')
Python-specific usage examples; begun with ">>>"
>>> print('(cut and pasted from interactive Python sessions)')
(cut and pasted from interactive Python sessions)
```
````

## 表

### Grid 表

对于 Grid 表 ([ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#grid-tables))，你必须自己”绘制”单元格网格。他们看起来像这样：

````{panels}
:container: w3-card-4 w3-pale-green w3-padding
:column: col-lg-12 px-2 py-2
---
:header: w3-pale-blue
RST
^^^
```rst
+------------------------+------------+----------+----------+
| Header row, column 1   | Header 2   | Header 3 | Header 4 |
| (header rows optional) |            |          |          |
+========================+============+==========+==========+
| body row 1, column 1   | column 2   | column 3 | column 4 |
+------------------------+------------+----------+----------+
| body row 2             | ...        | ...      |          |
+------------------------+------------+----------+----------+
```
---
:header: w3-pale-red
渲染：
^^^
```{eval-rst}
+------------------------+------------+----------+----------+
| Header row, column 1   | Header 2   | Header 3 | Header 4 |
| (header rows optional) |            |          |          |
+========================+============+==========+==========+
| body row 1, column 1   | column 2   | column 3 | column 4 |
+------------------------+------------+----------+----------+
| body row 2             | ...        | ...      |          |
+------------------------+------------+----------+----------+
```
````

### 简单表

简单表 ([ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#simple-tables))更容易编写，但有限制: 它们必须包含多行，并且第一列单元格不能包含多行。他们看起来像这样：

````{panels}
:container: w3-card-4 w3-pale-green w3-padding
:column: col-lg-6 px-2 py-2
---
:header: w3-pale-blue
RST
^^^
```rst
=====  =====  =======
A      B      A and B
=====  =====  =======
False  False  False
True   False  False
False  True   False
True   True   True
=====  =====  =======
```
---
:header: w3-pale-red
渲染：
^^^
```{eval-rst}
=====  =====  =======
A      B      A and B
=====  =====  =======
False  False  False
True   False  False
False  True   False
True   True   True
=====  =====  =======
```
````

支持另外两种语法: CSV 表 和 List 表。他们使用 显式标记块。有关更多信息，请参阅 [表格](https://www.sphinx-doc.org/en/master/usage/restructuredtext/directives.html#table-directives)。

## 超链接

### 外部链接

使用  `` `Link text <https://domain.invalid/>`_ `` 用于内联网络链接。如果链接文本应该是网络地址，你根本不需要特殊的标记，解析器会在普通文本中找到链接和邮件地址。

```{important}
在链接文本和 URL 的开头 `<` 之间必须有一个空格。
```

你也可以把链接和目标定义分开([ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#hyperlink-targets))，就像这样：

```rst
This is a paragraph that contains `a link`_.

.. _a link: https://domain.invalid/
```

### 内部链接

内部链接是通过 Sphinx 提供的特殊 reST 角色完成的，请参阅特定标记部分 [交叉引用任意位置](https://www.sphinx-doc.org/en/master/usage/restructuredtext/roles.html#ref-role)。

## 章节

章节标题（[ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#sections)）是通过在章节标题下划线（也可选择在上面划线）来创建的，标点符号的长度至少与文本一样。

```rst
=================
This is a heading
=================
```

通常，没有为某些字符分配标题级别，因为结构是从标题的连续性确定的。但是，这个约定在 [Python 的样式指南](https://docs.python.org/devguide/documenting.html#style-guide) 中用于记录 ，您可以遵循：

- `#` 有上线，用于编（parts）
- `*` 有上线，用于章（chapters）
- `=`, 用于节（sections）
- `-`, 用于小节（subsections）
- `^`, 用于子小节（subsubsections）
- `"`, 用于段（paragraphs）

当然，您可以自由使用自己的标记字符(请参阅 reST 文档)，并使用更深层次的嵌套级别，但请记住，大多数目标格式（HTML，LaTeX）具有有限的支持嵌套深度。

## 角色

角色或 "自定义解释的文本角色"（[ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#roles)）是一个内联的明确标记。它标志着封闭的文本应该以一种特定的方式被解释。Sphinx 用它来提供语义标记和标识符的交叉引用，如在适当的章节中描述。一般的语法是 `` :rolename:`content` ``。

Docutils 支持以下角色：

* [emphasis](https://docutils.sourceforge.io/docs/ref/rst/roles.html#emphasis) – 相当于 `*emphasis*`
* [strong](https://docutils.sourceforge.io/docs/ref/rst/roles.html#strong) – 相当于 `**strong**`
* [literal](https://docutils.sourceforge.io/docs/ref/rst/roles.html#literal) – 相当于 ` ``literal`` `
* [subscript](https://docutils.sourceforge.io/docs/ref/rst/roles.html#subscript) – 下标文本
* [superscript](https://docutils.sourceforge.io/docs/ref/rst/roles.html#superscript) – 上标文本
* [title-reference](https://docutils.sourceforge.io/docs/ref/rst/roles.html#title-reference) – 书籍、期刊和其他材料的标题

请参阅 [角色](rst:roles)，了解 Sphinx 添加的角色。

## 显式标记

“显式标记”（[ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#explicit-markup-blocks)）在 reST 中用于大多数需要特殊处理的构造，例如脚注，特别突出显示的段落，注释和泛型指令。

显式标记块以 ”空行+`..`” 开头，后跟空格，并在相同的缩进级别由下一段终止。（显式标记和普通段落之间需要有一个空行。这可能听起来有点复杂，但是当你写它时它足够直观。）


### 指令

指令（[ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#directives)）是一个通用的显式标记块。和角色一样，它是 reST 的扩展机制之一，Sphinx 也大量使用它。

Docutils 支持以下指令：

* Admonitions: [attention](https://docutils.sourceforge.io/docs/ref/rst/directives.html#attention), [caution](https://docutils.sourceforge.io/docs/ref/rst/directives.html#caution), [danger](https://docutils.sourceforge.io/docs/ref/rst/directives.html#danger), [error](https://docutils.sourceforge.io/docs/ref/rst/directives.html#error), [hint](https://docutils.sourceforge.io/docs/ref/rst/directives.html#hint), [important](https://docutils.sourceforge.io/docs/ref/rst/directives.html#important), [note](https://docutils.sourceforge.io/docs/ref/rst/directives.html#note), [tip](https://docutils.sourceforge.io/docs/ref/rst/directives.html#tip), [warning](https://docutils.sourceforge.io/docs/ref/rst/directives.html#warning) and the generic [admonition](https://docutils.sourceforge.io/docs/ref/rst/directives.html#admonitions). (Most themes style only “note” and “warning” specially.)
* Images:

  * [image](https://docutils.sourceforge.io/docs/ref/rst/directives.html#image) (see also [Images](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#images) below)
  * [figure](https://docutils.sourceforge.io/docs/ref/rst/directives.html#figure) (an image with caption and optional legend)
* Additional body elements:

  * [contents](https://docutils.sourceforge.io/docs/ref/rst/directives.html#table-of-contents) (a local, i.e. for the current file only, table of contents)
  * [container](https://docutils.sourceforge.io/docs/ref/rst/directives.html#container) (a container with a custom class, useful to generate an outer `<div>` in HTML)
  * [rubric](https://docutils.sourceforge.io/docs/ref/rst/directives.html#rubric) (a heading without relation to the document sectioning)
  * [topic](https://docutils.sourceforge.io/docs/ref/rst/directives.html#topic), [sidebar](https://docutils.sourceforge.io/docs/ref/rst/directives.html#sidebar) (special highlighted body elements)
  * [parsed-literal](https://docutils.sourceforge.io/docs/ref/rst/directives.html#parsed-literal) (literal block that supports inline markup)
  * [epigraph](https://docutils.sourceforge.io/docs/ref/rst/directives.html#epigraph) (a block quote with optional attribution line)
  * [highlights](https://docutils.sourceforge.io/docs/ref/rst/directives.html#highlights), [pull-quote](https://docutils.sourceforge.io/docs/ref/rst/directives.html#pull-quote) (block quotes with their own class attribute)
  * [compound](https://docutils.sourceforge.io/docs/ref/rst/directives.html#compound-paragraph) (a compound paragraph)
* Special tables:

  * [table](https://docutils.sourceforge.io/docs/ref/rst/directives.html#table) (a table with title)
  * [csv-table](https://docutils.sourceforge.io/docs/ref/rst/directives.html#csv-table) (a table generated from comma-separated values)
  * [list-table](https://docutils.sourceforge.io/docs/ref/rst/directives.html#list-table) (a table generated from a list of lists)
* Special directives:

  * [raw](https://docutils.sourceforge.io/docs/ref/rst/directives.html#raw-data-pass-through) (include raw target-format markup)
  * [include](https://docutils.sourceforge.io/docs/ref/rst/directives.html#include) (include reStructuredText from another file) – in Sphinx, when given an absolute include file path, this directive takes it as relative to the source directory
  * [class](https://docutils.sourceforge.io/docs/ref/rst/directives.html#class) (assign a class attribute to the next element) [^1]
* HTML specifics:

  * [meta](https://docutils.sourceforge.io/docs/ref/rst/directives.html#meta) (generation of HTML `<meta>` tags, see also [HTML Metadata](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#html-meta) below)
  * [title](https://docutils.sourceforge.io/docs/ref/rst/directives.html#metadata-document-title) (override document title)
* Influencing markup:

  * [default-role](https://docutils.sourceforge.io/docs/ref/rst/directives.html#default-role) (set a new default role)
  * [role](https://docutils.sourceforge.io/docs/ref/rst/directives.html#role) (create a new role)

  Since these are only per-file, better use Sphinx’s facilities for setting the [`default_role`](https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-default_role).

[^1]: When the default domain contains a class directive, this directive will be shadowed. Therefore, Sphinx re-exports it as rst-class.

```{warning}
不要使用指令 [sectnum](https://docutils.sourceforge.io/docs/ref/rst/directives.html#sectnum), [header](https://docutils.sourceforge.io/docs/ref/rst/directives.html#header) 和 [footer](https://docutils.sourceforge.io/docs/ref/rst/directives.html#footer)。
```

Sphinx 添加的指令描述于 [](rst:directives)。

基本上，一个指令由名称、参数、选项和内容组成。（请记住这个术语，它将在下一章描述自定义指令时使用））看一下这个例子：

```rst
.. function:: foo(x)
              foo(y, z)
   :module: some.module.name

   Return a line of text input from the user.
```

`function` 是指令名称。这里给出了两个参数，第一行和第二行的其余部分，以及一个选项 `module`（如你所见，选项在紧跟在参数后面的行中给出并由冒号表示）。选项必须缩进到与指令内容相同的级别。

指令内容在空白行后面，并相对于指令开始缩进。

### 脚注

对于脚注([ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#footnotes))，使用 `[#name]_` 标记脚注位置，并在 “Footnotes” 标题后添加脚注主体在文档底部，像这样：

```rst
Lorem ipsum [#f1]_ dolor sit amet ... [#f2]_

.. rubric:: Footnotes

.. [#f1] Text of the first footnote.
.. [#f2] Text of the second footnote.
```

您还可以明确编号脚注(`[1]_`)或使用不带名字的自动编号脚注(`[#]_`)。

### 引文

支持标准 reST 引用([ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#citations))，其附加功能是 “global” ，即所有引用都可以从所有文件引用。像这样使用它们：

```rst
Lorem ipsum [Ref]_ dolor sit amet.

.. [Ref] Book or article reference, URL or whatever.
```

引用用法类似于脚注用法，但标签不是数字或以”`#`”开头。

### 替换

reST支持 “substitutions” ([ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#substitution-definitions))，它们是文本中按 `` |name| `` 引用的文本和/或标记。它们被定义为带有显式标记块的脚注，如下：

```rst
.. |name| replace:: replacement *text*
```

或者

```rst
.. |caution| image:: warning.png
             :alt: Warning!
```

有关详细信息，请参阅 [reST 参考替换](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#substitution-definitions)。

如果要对所有文档使用一些替换，请将它们放入 [rst_prolog](https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-rst_prolog) 或 [rst_epilog](https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-rst_epilog) 或将它们放入单独的文件中，并将其包含在要使用它们的所有文档中，使用 `include` 指令。（确保为 `include` 文件提供与其他源文件不同的文件扩展名，以避免 Sphinx 将其作为独立文档发现。）

Sphinx 定义了一些默认替换，请参阅 [替换](https://www.sphinx-doc.org/en/master/usage/restructuredtext/roles.html#default-substitutions)。

### 评论

每个显式标记块都不是有效的标记结构（如上面的脚注），它被视为注释([ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#comments))。例如：

```rst
.. This is a comment.
```

您可以在评论开始后缩进文本以形成多行注释：

```rst
..
   This whole indented block
   is a comment.

   Still in the comment.
```

## 图片

reST 支持一个 `image` 指令([ref](https://docutils.sourceforge.io/docs/ref/rst/directives.html#image))，像这样使用：

```rst
.. image:: gnu.png
   (options)
```

在 Sphinx 中使用时，给定的文件名（此处为 gnu.png）必须相对于源文件，或者绝对意味着它们相对于顶级源目录。例如，文件 `sketch/spam.rst` 可以将图像 `images/spam.png` 写为 `../images/spam.png` 或 `/images/spam.png`。

Sphinx 会自动将图像文件复制到构建的输出目录的子目录中（例如，用于 HTML 输出的 `_static` 目录）。

图像大小选项（`width` 和 `height`）的解释如下：如果大小没有单位或单位是像素，则只有支持像素的输出通道才会考虑给定大小。其他单位（如点的 `pt`）将用于 HTML 和 LaTeX 输出（后者用 `bp` 替换 `pt`，因为这是 TeX 单元，`72bp=1in`)。

Sphinx 通过允许扩展名的星号来扩展标准的 docutils 行为：

```rst
.. image:: gnu.*
```

然后，Sphinx 搜索与提供的模式匹配的所有图像并确定其类型。然后，每个构建器从这些候选者中选择最佳图像。例如，如果给出了文件名 `gnu.*` 并且源树中存在两个文件 `gnu.pdf` 和 `gnu.png`，则 LaTeX 构建器将选择前者，而 HTML 构建器更喜欢后者。支持的图像类型和选择优先级定义在 [构建器](https://www.sphinx-doc.org/en/master/usage/builders/index.html)。

请注意，图像文件名不应包含空格。

## HTML Metadata

元指令（[ref](https://docutils.sourceforge.io/docs/ref/rst/directives.html#meta)）允许指定 Sphinx 文档页面的 HTML 元数据元素。例如，指令：

```rst
.. meta::
   :description: The Sphinx documentation builder
   :keywords: Sphinx, documentation, builder
```

将产生以下 HTML 输出：

```html
<meta name="description" content="The Sphinx documentation builder">
<meta name="keywords" content="Sphinx, documentation, builder">
```

同时，Sphinx 会把元指令中指定的关键词添加到搜索索引中。因此，元元素的 `lang` 属性被考虑。例如，指令：

```rst
.. meta::
   :keywords: backup
   :keywords lang=en: pleasefindthiskey pleasefindthiskeytoo
   :keywords lang=de: bittediesenkeyfinden
```

在具有不同语言配置的构建的搜索索引中添加以下词语：

* `pleasefindthiskey`, `pleasefindthiskeytoo` to *English* builds;
* `bittediesenkeyfinden` to *German* builds;
* `backup` to builds in all languages.

## 源编码

由于在 reST 中包含特殊字符如 em 破折号或版权标志的最简单方法是直接把它们写成 Unicode 字符，所以必须指定一个编码。Sphinx 假设源文件默认为 UTF-8 编码；你可以用 [`source_encoding`](https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-source_encoding) 配置值来改变它。

## 陷阱

在编写 reST 文档时，通常会遇到一些问题：

- 行内标记的分离：如上所述，行内标记跨度必须通过非单词字符与周围文本分开，您必须使用反斜杠转义空格来绕过它。有关详细信息，请参阅 [引用](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#substitution-definitions)。
- 没有嵌套的行内标记：像 `` *see :func:`foo`* `` 这样的东西是不可能的。
