(wiki:myst/syntax)=
# MyST 语法指南 

> {sub-ref}`today` | {sub-ref}`wordcount-minutes` min read

作为一个基础，MyST 遵守 [CommonMark 规范](https://spec.commonmark.org/)。为此，它使用了 [markdown-it-py](https://github.com/executablebooks/markdown-it-py) 分析器，这是一个结构良好的 Python Markdown 分析器，符合 CommonMark 规范，而且可以扩展。

MyST 为 CommonMark 增加了几个新的语法选项，以便与 Sphinx 一起使用，Sphinx 是 Python 生态系统中广泛使用的文档生成引擎。

下面是解析的语法 "tokens" 的摘要，以及从 CommonMark 风味的 Markdown 中获得的几个主要扩展的进一步细节。

:::{seealso}
- 关于用 MyST Markdown 编写指令和角色的介绍，请看 {ref}`intro/writing`。
- 查看 [MyST-Markdown VS Code 扩展](https://marketplace.visualstudio.com/items?itemName=ExecutableBookProject.myst-highlight)，以获得 MyST 扩展语法高亮。
:::

MyST 建立在 markdown-it 定义的标记上，以扩展 [CommonMark Spec](https://spec.commonmark.org/0.29/) 中描述的语法，解析器就是根据这个语法测试的。

% TODO link to markdown-it documentation

(syntax/directives)=

## 指令--一个块级的扩展点

指令的语法是用三括号和大括号定义的。它实际上是一个代码块，语言周围有大括号，指令名称代替了语言名称。它类似于 RMarkdown 定义 "可运行单元" 的方式。下面是基本的结构：

`````{list-table}
---
header-rows: 1
---
* - MyST
  - reStructuredText
* - ````md
    ```{directivename} arguments
    ---
    key1: val1
    key2: val2
    ---
    This is
    directive content
    ```
    ````
  - ```rst
    .. directivename:: arguments
       :key1: val1
       :key2: val2

       This is
       directive content
    ```
`````

例如，下面的代码：

````md
```{admonition} This is my admonition
This is my note
```
````

将产生这种训诫：

```{admonition} This is my admonition
This is my note
```

### 参数化指令

对于以参数作为输入的指令，有两种方法可以将其参数化。在每种情况下，选项本身都是以 `key: value` 对的形式给出。下面是一个例子：

**使用 YAML frontmatter**。在指令的第一行之后的 YAML front-matter 将被解析为该指令的选项。这需要用 `---` 行来包围。两者之间的所有内容都将被 YAML 解析，并作为关键字参数传递给你的指令。比如说：

````md
```{code-block} python
---
lineno-start: 10
emphasize-lines: 1, 3
caption: |
    This is my
    multi-line caption. It is *pretty nifty* ;-)
---
a = 2
print('my 1st line')
print(f'my {a}nd line')
```
````

```{code-block} python
---
lineno-start: 10
emphasize-lines: 1, 3
caption: |
    This is my
    multi-line caption. It is *pretty nifty* ;-)
---
a = 2
print('my 1st line')
print(f'my {a}nd line')
```

使用 `:` 字符的简写选项。如果你的指令只需要一到两个选项，并且希望节省行数，你也可以在指令的第一行之后指定指令选项的集合，每行前面都有 `:`。然后，每一行的前导符 `:` 会被删除，剩下的部分会被解析为 YAML。

比如说：

````md
```{code-block} python
:lineno-start: 10
:emphasize-lines: 1, 3

a = 2
print('my 1st line')
print(f'my {a}nd line')
```
````

(syntax/directives/parsing)=
### 指令如何解析内容

有些指令会解析其内容块中的内容。MyST 将这些内容解析为 Markdown。

这意味着 MyST 的 Markdown 可以写在任何用 MyST markdown 写的指令的内容区里。比如说：

````md
```{admonition} My markdown link
Here is [markdown link syntax](https://jupyter.org)
```
````

```{admonition} My markdown link
Here is [markdown link syntax](https://jupyter.org)
```

作为不需要参数的指令的简称，当没有使用参数选项时（见下文），你可以在指令名称后直接开始内容。

````md
```{note} Notes require **no** arguments, so content can start here.
```
````

```{note} Notes require **no** arguments, so content can start here.
```

对于特殊情况，MySt 还提供 `eval-rst` 指令。这将把内容解析为 ReStructuredText：

````md
```{eval-rst}
.. figure:: img/fun-fish.png
  :width: 100px
  :name: rst-fun-fish

  Party time!

A reference from inside: :ref:`rst-fun-fish`

A reference from outside: :ref:`syntax/directives/parsing`
```
````

```{eval-rst}
.. figure:: img/fun-fish.png
  :width: 100px
  :name: rst-fun-fish

  Party time!

A reference from inside: :ref:`rst-fun-fish`

A reference from outside: :ref:`syntax/directives/parsing`
```

注意这段文字是如何与文件的其他部分结合在一起的，所以我们也可以在文件的其他任何地方引用 [party fish](rst-fun-fish)。

### 嵌套指令

你可以通过确保最外层指令所对应的刻度线长于内部指令的刻度线来嵌套指令。例如，将一个警告嵌套在一个注解块内，像这样：

`````md
````{note}
The next info should be nested
```{warning}
Here's my warning
```
````
`````

下面是它的渲染效果：

````{note}
The next info should be nested
```{warning}
Here's my warning
```
````

你可以缩进内部代码栅栏，只要它们的缩进幅度不超过 3 个空格。否则，它们将被呈现为 "原始代码" 块。

`````md
````{note}
The warning block will be properly-parsed

   ```{warning}
   Here's my warning
   ```

But the next block will be parsed as raw text

    ```{warning}
    Here's my raw text warning that isn't parsed...
    ```
````
`````

````{note}
The warning block will be properly-parsed

   ```{warning}
   Here's my warning
   ```

But the next block will be parsed as raw text

    ```{warning}
    Here's my raw text warning that isn't parsed...
    ```
````

如果你愿意的话，这真的可以被滥用 :-)

``````{note}
The next info should be nested
`````{warning}
Here's my warning
````{admonition} Yep another admonition
```python
# All this fuss was about this boring python?!
print('yep!')
```
````
`````
``````

### Markdown 友好指令

想使用能在标准 Markdown 编辑器中正确显示的语法吗？请看 [扩展语法选项](syntax/colon_fence)。

```md
:::{note}
This text is **standard** _Markdown_
:::
```

:::{note}
This text is **standard** _Markdown_
:::

(syntax/roles)=

## 角色--内联扩展点

角色类似于指令--它们允许你定义任意的新功能，但它们是内联使用的。要定义一个内联角色，请使用以下形式：

````{list-table}
---
header-rows: 1
---
* - MyST
  - reStructuredText
* - ````md
    {role-name}`role content`
    ````
  - ```rst
    :role-name:`role content`
    ```
````

例如，下面的代码：

```md
Since Pythagoras, we know that {math}`a^2 + b^2 = c^2`
```

变成

Since Pythagoras, we know that {math}`a^2 + b^2 = c^2`

你可以使用角色来做一些事情，比如引用方程式和你书中的其他项目。比如说：

````md
```{math} e^{i\pi} + 1 = 0
---
label: euler-eq
---
```

Euler's identity, equation {math:numref}`euler-eq`, was elected one of the
most beautiful mathematical formulas.
````

变成：

```{math} e^{i\pi} + 1 = 0
---
label: euler-eq
---
```

Euler's identity, equation {math:numref}`euler-eq`, was elected one of the
most beautiful mathematical formulas.

### 角色如何解析内容？

角色的内容被解析的方式不同，这取决于你所使用的角色。有些角色期望输入的内容将被用来改变功能。例如，`ref` 角色会假定输入内容是对网站其他部分的引用。然而，其他角色可能使用 MyST 解析器将输入内容解析为内容。

有些角色也会根据你传递的内容来扩展其功能。例如，按照上面 `ref` 的例子，如果你传递一个这样的字符串 `Content to display <myref>`，那么 `ref` 将显示 `Content to display`，并使用 `myref` 作为引用来查找。

角色如何解析这些内容取决于创建角色的作者。

(syntax/roles/special)=

(extra-markdown-syntax)=
## Extra markdown syntax

除了角色和指令之外，MyST 还支持 CommonMark 中不存在的额外的 Markdown 语法。在大多数情况下，这些是调用角色和指令的语法捷径。我们将在下面介绍一些常见的。

这个表格描述了 RST 和 MyST 的对应关系：

````{list-table}
---
header-rows: 1
---
* - Type
  - MyST
  - reStructuredText
* - Math shortcuts
  - `$x^2$`
  - N/A
* - Front matter
  - ```md
    ---
    key: val
    ---
    ```
  - ```md
    :key: val
    ```
* - Comments
  - `% comment`
  - `.. comment`
* - Targets
  - `(mytarget)=`
  - `.. _mytarget:`
````

(syntax/frontmatter)=

## Front Matter

这是一个位于文档开头的 YAML 块，例如在 [jekyll](https://jekyllrb.com/docs/front-matter/) 中使用。

:::{seealso}
Top-matter 也用于[替换语法扩展](syntax/substitutions)，并可用于存储博客发布的信息（见[ablog's myst-parser support](https://ablog.readthedocs.io/manual/markdown/)）。
:::

(syntax/html_meta)=

### 设置 HTML 元数据

front-matter 可以包含特殊的键 `html_meta`；一个包含数据的 dict，作为 [`<meta>` 元素](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta) 添加到生成的HTML中。这相当于使用 [RST `meta` 指令](https://www.sphinx-doc.org/en/master/usage/restructuredtext/basics.html#html-metadata)。

HTML 元数据也可以在 `conf.py` 中通过 `myst_html_meta` 变量全局添加，在这种情况下，它将被添加到所有 MyST 文档中。对于每个文档，`myst_html_meta` dict 将被文档级别的front-matter `html_meta` 所更新，而 front-matter 优先。

:::{tabbed} Sphinx Configuration

```python
language = "en"
myst_html_meta = {
    "description lang=en": "metadata description",
    "description lang=fr": "description des métadonnées",
    "keywords": "Sphinx, MyST",
    "property=og:locale":  "en_US"
}
```

:::

:::{tabbed} MyST Front-Matter

```yaml
---
html_meta:
  "description lang=en": "metadata description"
  "description lang=fr": "description des métadonnées"
  "keywords": "Sphinx, MyST"
  "property=og:locale": "en_US"
---
```

:::

:::{tabbed} RestructuredText

```restructuredtext
.. meta::
   :description lang=en: metadata description
   :description lang=fr: description des métadonnées
   :keywords: Sphinx, MyST
   :property=og:locale: en_US
```

:::

:::{tabbed} HTML Output

```html
<html lang="en">
  <head>
    <meta content="metadata description" lang="en" name="description" xml:lang="en" />
    <meta content="description des métadonnées" lang="fr" name="description" xml:lang="fr" />
    <meta name="keywords" content="Sphinx, MyST">
    <meta content="en_US" property="og:locale" />
```

:::


(syntax/comments)=

## 注释

你可以通过在一行的开头加上 `%` 字符来添加注释。这将阻止该行被解析到输出文档中。

例如，这段代码：

```md
% my comment
```

是在下面，但它不会被解析到文档中。

% my comment

````{important}
由于注释是一个块级实体，它们将终止前一个块。在实践中，这意味着下面的行将被分成两段，导致它们之间出现一个新的行：

```
a line
% a comment
another line
```

a line
% a comment
another line
````

(syntax/blockbreaks)=

## 块截断

你可以在一行的开头加上 `+++`，以增加一个区块分隔。该结构的预期使用情况是用于映射到基于单元格的文档格式，如 [Jupyter 笔记本](https://jupyter.org/)，以表示一个新的文本单元。它不会显示在渲染的文本中，而是存储在内部文档结构中供开发人员使用。

例如，这段代码：

```md
+++ some text
```

Is below, but it won't be parsed into the document.

+++

(syntax/targets)=

## 锚点和交叉引用

锚点是用来定义自定义的锚点，可以在文档的其他地方引用。它们通常放在章节标题之前，以便您可以方便地引用它们。

:::{tip}

如果你想为你的每个章节标题自动生成锚点，请查看扩展语法中的 [](syntax/header-anchors) 锚部分。

:::

锚标头是用这种语法定义的：

```md
(header_target)=
```

然后，它们可以用 [`ref` 内联角色](https://www.sphinx-doc.org/en/master/usage/restructuredtext/roles.html#role-ref) 来引用：

```md
{ref}`header_target`
```

默认情况下，引用将使用锚的文本（如章节标题），但也可以直接指定文本：

```md
{ref}`my text <header_target>`
```

例如，请看这个引用：{ref}`syntax/targets`，这里有一个回到本页顶部的 {ref}`my text <example_syntax>`。

或者使用 Markdown 语法：

```md
[my text](header_target)
```

相当于使用 [`any` 内联角色](https://www.sphinx-doc.org/en/master/usage/restructuredtext/roles.html#role-any):

```md
{any}`my text <header_target>`
```

但也能接受 "嵌套"语法（如粗体字），并能识别包含扩展名的文件路径（例如 `syntax/syntax` or `syntax/syntax.md`）

```{note}
如果你想让目标的标题插入你的文本中，你可以让 Markdown 链接的 "文本"部分为空。例如，这个 `[](wiki:myst/syntax)` 将导致 [](wiki:myst/syntax)。
```

## 图片

MyST 提供了几种不同的语法来在你的文档中加入图片。

标准的 Markdown 语法是：

```md
![fishy](img/fun-fish.png)
```

![fishy](img/fun-fish.png)

但你也可以启用扩展的图像语法，以控制宽度和标题等属性。参见 [](syntax/images)。

(syntax/footnotes)=
## 脚注

脚注使用 [pandoc 规范](https://pandoc.org/MANUAL.html#footnotes)。它们的标签以 `^` 开头，然后可以是任何字母数字字符串（没有空格），不区分大小写。

- 如果标签是一个整数，那么它将始终使用该整数来渲染标签（即它们是手动编号的）。
- 对于任何其他标签，它们将按照它们被引用的顺序自动编号，跳过任何手动编号的标签。

所有的脚注定义都被收集起来，并显示在页面的底部（按照它们被引用的顺序）。请注意，未被引用的脚注定义将不被显示。

```md
- This is a manually-numbered footnote reference.[^3]
- This is an auto-numbered footnote reference.[^myref]

[^myref]: This is an auto-numbered footnote definition.
[^3]: This is a manually-numbered footnote definition.
```

- This is a manually-numbered footnote reference.[^3]
- This is an auto-numbered footnote reference.[^myref]

[^myref]: This is an auto-numbered footnote definition.
[^3]: This is a manually-numbered footnote definition.

在脚注定义之后的任何前面的文本，如果缩进了四个或更多的空格，也将被包括在脚注定义中，并且该文本被呈现为 MyST Markdown，例如：

```md
A longer footnote definition.[^mylongdef]

[^mylongdef]: This is the _**footnote definition**_.

    That continues for all indented lines

    - even other block elements

    Plus any preceding unindented lines,
that are not separated by a blank line

This is not part of the footnote.
```

A longer footnote definition.[^mylongdef]

[^mylongdef]: This is the _**footnote definition**_.

    That continues for all indented lines

    - even other block elements

    Plus any preceding unindented lines,
that are not separated by a blank line

This is not part of the footnote.

````{important}
尽管脚注引用可以在指令中使用，例如 [^myref]，但我们建议不要在指令中设置脚注定义，除非它们只在同一指令中被引用：

```md
[^other]

[^other]: A definition within a directive
```

[^other]

[^other]: A definition within a directive

这是因为，在目前的实施中，它们可能无法在该特定指令上方的文本中引用。
````

默认情况下，在任何脚注之前都会有一个过渡线（带有 `footnotes` 类）。这可以通过在配置文件中添加 `myst_footnote_transition = False` 来关闭。

## 代码块


### 在原始 Markdown 块中显示 backticks

如果你想在你的 Markdown 中显示 backticks，你可以通过把它们嵌套在更大长度的后缀中来实现。 Markdown 会把最外面的反引号当作 "原始" 块的边缘，里面的东西都会显示出来。比如说：

``` `` `hi` `` ``` 将被渲染为 `` `hi` ``

并且

`````
````
```
hi
```
````
`````

将被渲染为：

````
```
hi
```
````
