# 其他

Doctree 元素：[field_list](https://docutils.sourceforge.io/docs/ref/doctree.html#field-list), [field](https://docutils.sourceforge.io/docs/ref/doctree.html#field), [field_name](https://docutils.sourceforge.io/docs/ref/doctree.html#field-name), [field_body](https://docutils.sourceforge.io/docs/ref/doctree.html#field-body).

字段列表（Field Lists，[ref](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#field-lists)）被用作扩展语法的一部分，如指令的选项，或类似数据库的记录（records），用于进一步处理。它们也可用于类似于数据库记录的两列表结构（标签和数据对）。reStructuredText 的应用可以在某些情况下识别字段名并转换字段或字段体。例子见下面的[书目字段](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#toc-entry-18)，或 [reStructuredText 指令](https://docutils.sourceforge.io/docs/ref/rst/directives.html) 中的 "[image](https://docutils.sourceforge.io/docs/ref/rst/directives.html#image)" 和 "[meta](https://docutils.sourceforge.io/docs/ref/rst/directives.html#metadata)" 指令。

字段列表是像这样标记的字段序列：

```rst
:fieldname: Field content
```

字段列表是字段名到字段体的映射，以 [RFC822](http://www.rfc-editor.org/rfc/rfc822.txt) 头文件为模型。字段名可以由任何字符组成，但字段名内的冒号（":"）在后面有空白时必须进行反斜线转义。内联标记在字段名中被解析，但在字段名中使用带有明确角色的[解释文本](https://docutils.sourceforge.io/docs/ref/rst/restructuredtext.html#interpreted-text)时必须注意：角色必须是解释文本的后缀。在进一步处理或转换时，字段名是不分大小写的。字段名与单冒号前缀和后缀一起构成字段标记。字段标记之后是空白和字段主体。字段主体可以包含多个主体元素，相对于字段标记缩进。字段名标记后的第一行决定了字段主体的缩进。比如说：

````{panels}
:container: w3-card-4 w3-pale-green w3-padding
:column: col-lg-12 px-2 py-2
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

Sphinx 扩展了标准的 docutils 行为，并拦截了文档开头指定的字段列表。有关更多信息，请参阅 [Field Lists](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/field-lists.html)。

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

## 书目字段

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

## 选项列表

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