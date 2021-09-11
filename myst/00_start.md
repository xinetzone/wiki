# 快速上手

参考：[](sphinx/intro)

## 安装

安装 MyST 解析器可以访问两个工具：

- 一个可以解析 MyST Markdown 的 Python 库，并将其渲染成多种输出格式（特别是与 Sphinx 一起使用的 `docutils` 格式）。
- 一个 Sphinx 扩展，利用上述工具来解析你文档中的 MyST Markdown。

要安装 MyST 解析器，请在 Conda 环境下运行以下程序（推荐）：

```sh
conda install -c conda-forge myst-parser
```

或者

```sh
pip install myst-parser
```

## 在 Sphinx 中启用 MyST

Sphinx 是一个文件生成器，用于从多个源文件和 assets 中构建网站或书籍。要开始使用 Sphinx，请看他们的[快速入门指南](https://www.sphinx-doc.org/en/master/usage/quickstart.html)。本指南假设你已经有了一个预先存在的 Sphinx 网站，并能正常构建。

要在 Sphinx 中使用 MyST 解析器，只需在你的 `conf.py` 文件中加入以下内容：

```python
extensions = ["myst_parser"]
```

这将激活 MyST 解析器扩展，使所有扩展名为 `.md` 的文件被解析为 MyST。

```{admonition} 也可以同时使用 MyST 和 reStructuredText
激活 MyST 解析器将简单地用 MyST 解析 Markdown 文件，而 Sphinx 附带的 rST 解析器仍将以同样的方式工作。以 `.md` 结尾的文件将被解析为 MyST，而以 `.rst` 结尾的文件将被解析为 reStructuredText。
```

## 编写你的第一个 Markdown 文档

现在你已经在 Sphinx 中启用了 `myst-parser`，你可以在一个以 `.md` 结尾的文件中为你的页面编写 MyST Markdown。

```{note}
MyST Markdown 是两种口味的 Markdown 的混合物。

它的基础是支持 **CommonMark Markdown** 的所有语法。这是一种在许多项目中使用的社区标准的 Markdown。

此外，它还包括对 CommonMark 的一些扩展。这些扩展为技术写作增加了额外的语法功能，比如 Sphinx 使用的角色和指令。
```

首先，创建一个名为 `myfile.md` 的空文件，并给它一个 Markdown 标题和文本。

```md
# My nifty title

Some **text**!
```

在你的 Sphinx 项目的 "主文件"中（你的 Sphinx 文档的登陆页），在 `toctree` 指令中包含 `myfile.md`，这样它就被包含在你的文档中：

```rst
.. toctree::

   myfile.md
```

现在创建网站：

```sh
make html
```

并导航到你的登陆页面。你应该看到一个指向从 `myfile.md` 生成的页面的链接。点击这个链接，你就可以看到你的 Markdown 的渲染了。

## 用指令扩展 Markdown

MyST Markdown 最重要的功能是编写指令。指令有点像为编写内容而设计的函数。Sphinx 和 reStructuredText 广泛地使用指令。下面是一个指令在 MyST markdown 中的样子：

````{margin} 替代选项语法
如果你的指令有很多选项，或者有一个非常长的值（例如，跨越多行），那么你也可以把你的选项包在 `---` 行中，并把它们写成 YAML。比如说

```yaml
---
key1: val1
key2: |
  val line 1
  val line 2
---
```
````

````md
```{directivename} <directive arguments>
:optionname: <valuename>

<directive content>
```
````

对于那些熟悉 reStructuredText 的人来说，你可以在这里找到[从 MyST 指令语法到 RST 语法的映射](https://myst-parser.readthedocs.io/en/latest/syntax/syntax.html#syntax-directives)。

编写指令时有四个主要部分需要考虑：

- **指令的名称**有点像函数的名称。不同的名字触发不同的功能。它们被包裹在 `{}` 括号内。
- **指令参数**就在指令名称的后面。它们可以用来触发指令中的行为。
- **指令选项**位于指令的第一行之后。它们也控制指令的行为。
- **指令内容**是你放在指令中的标记。指令经常以一种特殊的方式显示这些内容。

例如，在你的 Narkdown 页面上添加一个 **`admonition`** 指令，像这样：

````md
# My nifty title

Some **text**!

```{admonition} Here's my title
:class: warning

Here's my admonition content
```
````

重新建立你的 Sphinx 网站，你应该看到新的告诫框出现了。

正如你所看到的，我们已经使用了上面描述的四个部分中的每一个来配置这个指令。下面是指令渲染后的样子：

```{admonition} Here's my title
:class: warning

Here's my admonition content
```

:::{seealso}
关于在 MyST 中使用指令的更多信息，请参见 {ref}`syntax/directives`。
:::

(sphinx/intro:reference)=
## 用一个角色引用一个章节的标签

角色是另一个核心的 Sphinx 工具。它们的行为类似于指令，但与文本连在一起，而不是在一个单独的块中。它们有以下形式：

```md
{rolename}`role content`
```


角色比指令更简单，尽管有些角色允许在其内容区域内使用更复杂的语法。

例如，`ref` 角色用于引用你的文档的其他部分，并允许你在角色中指定显示的文本和引用本身：

```md
{ref}`My displayed text <my-ref>`
```

例如，让我们在你的 Markdown 文件中添加一个**章节的引用**。要做到这一点，我们首先需要给你的页面的某个部分添加一个**标签**。要做到这一点，请使用以下结构：

```md
(label-name)=
## Some header
```

把这个添加到你上面的 Markdown文件中，像这样：

````md
# My nifty title

Some **text**!

```{admonition} Here's my title
:class: warning

Here's my admonition content
```

(section-two)=
## Here's another section

And some more content.
````

因为你的新章节有一个标签（`section-two`），你可以用 `ref` 角色引用它。像这样把它添加到你的 Markdown 文件中：

```md
(label-name)=
## Some header
```

把这个添加到你上面的 Markdown 文件中，像这样：

````md
# My nifty title

Some **text**!

```{admonition} Here's my title
:class: warning

Here's my admonition content
```

(section-two)=
## Here's another section

And some more content.

And here's {ref}`a reference to this section <section-two>`.
I can also reference the section {ref}`section-two` without specifying my title.
````

重新构建你的文档，你应该看到 `ref` 被自动插入。下面是一个例子，说明参考文献的角色在最终输出中的样子：

这里有一个引用 {ref}`sphinx/intro:reference`。

:::{seealso}
关于角色的更多信息，见 {ref}`syntax/roles`。
:::

## 使用额外的 MyST 语法添加评论

在 MyST 中还有许多其他类型的语法，使写作更有成效，更有乐趣。让我们来玩一玩几个选项。

首先，尝试写一个注释。这可以通过在你的 Markdown 文件中添加一个以 `%` 开头的行来完成。例如，试着在你的 Markdown 文件中添加一个注释，像这样。

````md
# My nifty title

Some **text**!

```{admonition} Here's my title
:class: warning

Here's my admonition content
```

(section-two)=
## Here's another section

And some more content.

% This comment won't make it into the outputs!
And here's {ref}`a reference to this section <section-two>`.
I can also reference the section {ref}`section-two` without specifying my title.
````

重新构建你的文档--注释不应该出现在输出中。

## 通过配置来扩展 MyST

到目前为止，已经涵盖了 MyST 的基本语法和 Sphinx。然而，你可以通过一些方法来扩展这个基本语法并获得新的功能。首先是用 MyST 解析器启用一些 "开箱即用" 的扩展。这些扩展添加了新的语法，虽然不是 "核心MyST" 的一部分，但还是很有用的（并且有一天可能成为核心 MyST 的一部分）。

让我们来扩展 MyST 的基本语法，以便为指令设置围栏。这允许你在 ` ``` ` 之外用 `::` 来定义指令。这对于内容中包含 Markdown 的指令很有用。通过使用 `:::`，非 MyST 的 Markdown 渲染器仍然能够渲染里面的内容（而不是将其显示为代码块）。

要激活扩展，在你的 `conf.py` 文件中添加一个列表，其中包含你想激活的扩展。例如，要激活 "colon code fences" 扩展，在你的 `conf.py` 文件中添加以下内容：

```python
myst_enable_extensions = [
  "colon_fence",
]
```

你现在可以使用 `::` 来定义指令。例如，像这样修改你的 Markdown 文件：

````md
# My nifty title

Some **text**!

```{admonition} Here's my title
:class: warning

Here's my admonition content
```

(section-two)=
## Here's another section

And some more content.

% This comment won't make it into the outputs!
And here's {ref}`a reference to this section <section-two>`.
I can also reference the section {ref}`section-two` without specifying my title.

:::{note}
And here's a note with a colon fence!
:::
````

当你建立你的网站时，它应该在你的输出中呈现为一个 "注释块"。

## 安装一个新的 Sphinx 扩展并使用其功能

另一种在 Sphinx 扩展 MyST 的方法是安装 Sphinx 扩展，定义新的指令。指令有点像 Sphinx 的 "函数"，安装一个新的包可以添加新的指令来使用你的内容。

例如，让我们安装 `sphinxcontib.mermaid` 扩展，这将允许我们用 MyST 生成 [Mermaid 图](https://mermaid-js.github.io/mermaid/#/)。

首先，安装 `sphinxcontrib.mermaid`：

```shell
pip install sphinxcontrib-mermaid
```

接下来，在 `conf.py` 中把它添加到你的扩展列表中：

```python
extensions = [
  "myst_parser",
  "sphinxcontrib.mermaid",
]
```

现在，在你的 Markdown 文件中添加一个 **mermaid 指令**。比如：

````md
# My nifty title

Some **text**!

```{admonition} Here's my title
:class: warning

Here's my admonition content
```

(section-two)=
## Here's another section

And some more content.

% This comment won't make it into the outputs!
And here's {ref}`a reference to this section <section-two>`.
I can also reference the section {ref}`section-two` without specifying my title.

:::{note}
And here's a note with a colon fence!
:::

And finally, here's a cool mermaid diagram!

```{mermaid}
sequenceDiagram
  participant Alice
  participant Bob
  Alice->John: Hello John, how are you?
  loop Healthcheck
      John->John: Fight against hypochondria
  end
  Note right of John: Rational thoughts <br/>prevail...
  John-->Alice: Great!
  John->Bob: How about you?
  Bob-->John: Jolly good!
```
````

当你建立你的文档时，你应该看到像这样的东西：

```{mermaid}
sequenceDiagram
  participant Alice
  participant Bob
  Alice->John: Hello John, how are you?
  loop Healthcheck
      John->John: Fight against hypochondria
  end
  Note right of John: Rational thoughts <br/>prevail...
  John-->Alice: Great!
  John->Bob: How about you?
  Bob-->John: Jolly good!
```

## 接下来的步骤 - 了解更多关于 MyST 语法的信息

在本教程中，我们介绍了 MyST Markdown 的一些基础知识，如何在 Sphinx 中启用和使用它，以及如何为新的使用场景扩展它。在 MyST（以及 Sphinx 生态系统）中还有很多我们没有涉及的功能。更多信息，请参阅 [MyST 语法文档](https://myst-parser.readthedocs.io/en/latest/syntax/syntax.html) 和 [关于使用 MyST 和 Sphinx 的文档](https://myst-parser.readthedocs.io/en/latest/sphinx/index.html)。
