(rst:domains)=
# 域

参考：[域](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html) 

最初，Sphinx 是为一个单一的项目设想的，即 Python 语言的文档。不久之后，它作为一个文档工具被提供给所有人，但 Python 模块的文档仍然被深深地嵌入其中--最基本的指令，比如 `function`，是为 Python 对象设计的。由于 Sphinx 已经变得有些流行，人们对将其用于许多不同的目的产生了兴趣。C/C++ 项目，JavaScript，甚至是 reStructuredText 标记（比如这个文档）。

虽然这总是可行的，但现在通过为每个此类目的提供 **domain**，可以更轻松地支持使用不同编程语言的项目文档，甚至是主要 Sphinx 发行版不支持的项目文档。

域是一组标签(reStructuredText [directive](https://www.sphinx-doc.org/zh_CN/master/glossary.html#term-directive) 和 [role](https://www.sphinx-doc.org/zh_CN/master/glossary.html#term-role)s)，用于描述和链接 [object](https://www.sphinx-doc.org/zh_CN/master/glossary.html#term-object)，例如编程语言的元素。域中的指令和角色名称具有诸如 `domain:name` 之类的名称，例如 `py:function`。域还可以提供自定义索引(例如Python模块索引)。

拥有域意味着当一组文档想要引用如 C++ 和 Python 类时，没有命名问题。这也意味着支持全新语言文档的扩展更容易编写。

本节介绍 Sphinx 提供的域名。域 API 也在以下部分中记录 [域 API](https://www.sphinx-doc.org/zh_CN/master/extdev/domainapi.html#domain-api) 。

## 基本标记

大多数域提供了许多 *object description directives*，用于描述模块提供的特定对象。每个指令都需要一个或多个签名来提供有关所描述内容的基本信息，内容应该是描述。基本版本在一般索引中生成条目；如果不需要索引条目，可以给出指令选项标志 `:noindex:` 。使用 Python 域指令的示例：

````{panels}
:container: w3-card-4 w3-pale-green w3-padding
:column: col-lg-6 px-2 py-2
---
:header: w3-pale-blue
RST
^^^
```rst
.. py:function:: spam(eggs)
                 ham(eggs)

   Spam or ham the foo.
```
---
:header: w3-pale-red
渲染：
^^^
```{eval-rst}
.. py:function:: spam(eggs)
                 ham(eggs)

   Spam or ham the foo.
```
````

这描述了两个 Python 函数 `spam` 和 `ham` 。 (请注意，当签名变得太长时，如果向下一行中继续的行添加反斜杠，则可以将其分解。示例：

````{panels}
:container: w3-card-4 w3-pale-green w3-padding
:column: col-lg-12 px-2 py-2
---
:header: w3-pale-blue
RST
^^^
```rst
.. py:function:: filterwarnings(action, message='', category=Warning, \
                                module='', lineno=0, append=False)
   :noindex:
```
---
:header: w3-pale-red
渲染：
^^^
```{eval-rst}
.. py:function:: filterwarnings(action, message='', category=Warning, \
                                module='', lineno=0, append=False)
   :noindex:
```
````

（这个例子还展示了如何使用 `:noindex:` 标志。）

域还提供链接回这些对象描述的角色。例如，要链接到上面示例中描述的其中一个函数，您可以这样：

```rst
The function :py:func:`spam` does a similar thing.
```

如您所见，指令和角色名称都包含域名和指令名称。

### 默认域

对于仅从一个域描述对象的文档，作者在指定默认值后，不必再在每个指令，角色等处再次声明其名称。这可以通过配置值 [`primary_domain`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-primary_domain) 或通过此指令来完成:

`` .. default-domain:: name `` 选择一个新的默认域。虽然 [`primary_domain`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-primary_domain) 选择全局默认值，但这只在同一个文件中有效。

如果没有选择其他默认值，则 Python 域（名为 `py`）是默认值，主要是为了与为旧版 Sphinx 编写的文档兼容。

可以在不提供域名的情况下提及属于默认域的指令和角色，即:

```rst
.. function:: pyfunc()

   Describes a Python function.

Reference to :func:`pyfunc`.
```

### 交叉引用语法

对于域提供的交叉引用角色，存在与一般交叉引用相同的工具。请参阅 [交叉引用语法](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html#xref-syntax) 。

简而言之:

* 你可以提供一个明确的标题和引用目标：``  :role:`title <target>` `` 将引用 *target* ，但链接文本将是 *title* 。
* 如果在内容前加上 `!` ，则不会创建引用/超链接。
* 如果在内容前面添加 `~` ，则链接文本将只是目标的最后一个组件。例如，`` :py:meth:`~Queue.Queue.get` `` 将引用 `Queue.Queue.get` 但仅显示 `get` 作为链接文本。

(rst:domains/python)=
## Python 域

Python 域（名称 **py**）为模块声明提供以下指令：

### 常用指令

````{panels}
:container: w3-card-4 w3-pale-green w3-padding
:column: col-lg-12 px-2 py-1
:header: w3-pale-blue w3-large
---
`` .. py:module:: name`` 
^^^
该指令标志着模块（或包的子模块）描述的开始，在这种情况下，名称应该是完全限定的，包括包名称。它不会创建内容（例如 [`py:class`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-py:class "py:class directive") 确实如此）。

该指令还将导致全局模块索引中的条目变化。

`:platform: platforms (comma separated list)` 选项(如果存在)是一个逗号分隔的模块可用平台列表(如果它在所有平台上都可用，则应省略该选项)。密钥是短标识符；正在使用的示例包括 “IRIX” ，”Mac” ， “Windows” 和 “Unix” 。使用已适用的密钥非常重要。

`:synopsis: purpose (text)` 选项应包含一个描述模块用途的句子 - 它目前仅用于全局模块索引。

可以给出 `:deprecated: (no argument)` 选项(没有值)将模块标记为已弃用；它将在各个地点被指定。
---
`` .. py:currentmodule:: name `` 
^^^
该指令告诉 Sphinx，这里记录的类，函数等都在给定的模块中(如 [`py:module`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-py:module "py:module directive"))，但它不会创建索引条目，全局模块索引中的条目，或者一个链接目标 [`py:mod`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-py:mod "py:mod role") 。这在模块中的事物文档分布在多个文件或部分的情况下很有用 - 一个位置具有 [`py:module`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-py:module "py:module directive") 指令，其他只有 [`py:currentmodule`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-py:currentmodule "py:currentmodule directive") 。
````

为模块和类内容提供以下指令：

````{panels}
:container: w3-card-4 w3-pale-green w3-padding
:column: col-lg-12 px-2 py-1
:header: w3-pale-blue w3-large
---
`` .. py:function:: name(parameters) ``
^^^
描述模块级函数。签名应该包含 Python 函数定义中给出的参数，请参阅 [Python签名](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#signatures)。例如：

```rst
.. py:function:: Timer.repeat(repeat=3, number=1000000)
```

对于方法你应该使用 [`py:method`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-py:method "py:method directive") 。

描述通常包括有关所需参数及其使用方式的信息(特别是是否修改了作为参数传递的可变对象)，副作用和可能的异常。

这个信息可以(在任何 `py` 指令中)可选地以结构化形式给出，参见 [信息字段列表](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#info-field-lists) 。

`:async:` (无值)
:  表示该函数是一个异步函数。

`:canonical:` (包括模块名称在内的全限定名称)
:  如果该对象是从其他模块导入的，描述该对象的定义位置
---
`` .. py:data:: name ``
^^^
描述模块中的全局数据，包括用作“定义的常量”的变量和值。使用此环境不记录类和对象属性。

`:type:` 变量的类型 (文本)

`:value:` 变量的初始值 (文本)


`:canonical:` (包括模块名称在内的全限定名称)
:  如果该对象是从其他模块导入的，描述该对象的定义位置

---
`` .. py:exception:: name ``
^^^
描述异常类。签名可以，但不必包括带有构造函数参数的括号。

`:final:` (无值)
:  表明该类是一个 final 类。

---
`` .. py:class:: name `` & `` .. py:class:: name(parameters) ``
^^^
描述一个类。签名可以选择包括带有参数的括号，这些参数将显示为构造函数参数。另见 [Python签名](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#signatures) 。

属于该类的方法和属性应放在此指令的主体中。如果将它们放在外面，则提供的名称应包含类名，以便交叉引用仍然有效。例:

```rst
.. py:class:: Foo

   .. py:method:: quux()

-- or --

.. py:class:: Bar

.. py:method:: Bar.quux()
```

第一种方式是首选方式。

`:canonical:` (包括模块名称在内的全限定名称)
:  如果该对象是从其他模块导入的，描述该对象的定义位置


`:final:` (无值)
:  Indicate the class is a final class.

---
`` .. py:attribute:: name ``
^^^
描述对象数据属性。描述应包括有关预期数据类型的信息以及是否可以直接更改。

`:type:` 属性的类型 (文本)

`:value:` 属性的初始值 (文本)

`:canonical:` (包括模块名称在内的全限定名称)
:  如果该对象是从其他模块导入的，描述该对象的定义位置

---
`` .. py:property:: name ``
^^^
描述了一个对象的属性。

`:abstractmethod:` (无值)
:  表示该属性是抽象的。

`:classmethod:` (无值)
:  表示该属性是一个类方法。

`:type:` 属性的类型 (文本)

---
`` .. py:method:: name(parameters) ``
^^^
描述对象方法。参数不应包含 `self` 参数。描述应该包括与 `function` 描述的类似的信息。另见 [Python签名](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#signatures) 和 [信息字段列表](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#info-field-lists) 。

`:abstractmethod:` (无值)
:  表示该方法是一个抽象的方法。

`:async:` (无值)
:  表明该方法是一个异步方法。

`:canonical:` (包括模块名称在内的全称)
:  如果该对象是从其他模块导入的，描述该对象的定义位置

`:classmethod:` (无值)
:  表明该方法是一个类方法。

`:final:` (无值)
:  表示该类是一个 final 方法。

`:property:` (无值)
:  表示该方法是一个属性。

   ```{deprecated} 4.0
   使用 [`py:property`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-py-property "py:property directive") 代替。
   ```

`:staticmethod:` (无值)
:  表明该方法是一个静态方法。

---
`` .. py:staticmethod:: name(parameters) ``
^^^
像 [`py:method`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-py:method "py:method directive") ，但表示该方法是一个静态方法。
---
`` .. py:classmethod:: name(parameters) ``
^^^
像 [`py:method`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-py:method "py:method directive") ，但表示该方法是一个类方法。

---
`` .. py:decorator:: name `` & `` .. py:decorator:: name(parameters) ``
^^^
描述装饰器功能。签名应表示作为装饰器的用法。例如，给定函数：

```python
def removename(func):
    func.__name__ = ''
    return func

def setnewname(name):
    def decorator(func):
        func.__name__ = name
        return func
    return decorator
```

描述应如下所示:

```rst
.. py:decorator:: removename

   Remove name of the decorated function.

.. py:decorator:: setnewname(name)

   Set name of the decorated function to *name*.
```

(而不是 `` .. py:decorator:: removename(func) ``。)

没有 `py:deco` 角色链接到用这个指令标记的装饰器；相反，使用 [`py:func`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-py:func "py:func role") 角色。

---
`` .. py:decoratormethod:: name ``

`` .. py:decoratormethod:: name(signature) ``
^^^
与 [`py:decorator`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-py:decorator "py:decorator directive") 相同，但对于作为方法的装饰器。

使用 [`py:meth`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-py:meth "py:meth role") 角色引用装饰器方法。
````

### Python 签名

函数，方法和类构造函数的签名可以像在 Python 中编写一样给出。

可以给出可选参数的默认值(但如果它们包含逗号，则会混淆签名解析器)。还可以给出 Python 3 样式的参数注释以及返回类型注释：

```rst
.. py:function:: compile(source : string, filename, symbol='file') -> ast object
```

对于具有可选参数但没有默认值的函数（通常在没有关键字参数支持的C扩展模块中实现的函数），可以使用括号指定可选部分:

```{eval-rst}
.. py:function:: compile(source[, filename[, symbol]])
```

习惯上将开口括号放在逗号之前。

### 信息字段列表

在 Python 对象描述指令中，具有这些字段的reST字段列表可以很好地识别和格式化：

* `param`, `parameter`, `arg`, `argument`, `key`, `keyword`: 描述参数。
* `type`：参数的类型。如果可能，创建一个链接。
* `raises`，`raise`，`except`，`exception`：那（以及什么时候）会出现一个特定的异常。
* `var`, `ivar`, `cvar`：变量的描述。
* `vartype`: 变量类型。如果可能，创建一个链接。
* `returns`, `return`：返回值的描述。
* `rtype`：返回值类型。如果可能，创建一个链接。

```{note}
在 4.1.2 版本中，所有 `var` ， `ivar` 和 `cvar` 都表示为 “Variable” 。完全没有区别。
```

字段名称必须包含这些关键字之一和参数（除了 `returns` 和 `rtype` ，它们不需要参数）。这可以用一个例子来解释：

```rst
.. py:function:: send_message(sender, recipient, message_body, [priority=1])

   Send a message to a recipient

   :param str sender: The person sending the message
   :param str recipient: The recipient of the message
   :param str message_body: The body of the message
   :param priority: The priority of the message, can be a number 1-5
   :type priority: integer or None
   :return: the message id
   :rtype: int
   :raises ValueError: if the message_body exceeds 160 characters
   :raises TypeError: if the message_body is not a basestring
```

这将呈现如下：

````{div} w3-padding w3-pale-red
```{eval-rst}
.. py:function:: send_message(sender, recipient, message_body, [priority=1])

   Send a message to a recipient

   :param str sender: The person sending the message
   :param str recipient: The recipient of the message
   :param str message_body: The body of the message
   :param priority: The priority of the message, can be a number 1-5
   :type priority: integer or None
   :return: the message id
   :rtype: int
   :raises ValueError: if the message_body exceeds 160 characters
   :raises TypeError: if the message_body is not a basestring
```
````

如果类型是单个单词，也可以组合参数类型和描述，如下：

```rst
:param int priority: The priority of the message, can be a number 1-5
```

可以使用以下语法自动链接容器类型（如列表和词典）:

```rst
:type priorities: list(int)
:type priorities: list[int]
:type mapping: dict(str, int)
:type mapping: dict[str, int]
:type point: tuple(float, float)
:type point: tuple[float, float]
```

如果用 "or" 字分隔，一个类型字段中的多个类型将被自动链接：

```rst
:type an_arg: int or None
:vartype a_var: str or int
:rtype: float or str
```

### 交叉引用 Python 对象

以下角色引用模块中的对象，如果找到匹配的标识符，则可能是超链接：

````{div} w3-card-4 w3-padding w3-pale-green

`:py:mod:`
:  引用模块；可以使用点名称。这也应该用于包名称。

`:py:func:`
:  引用 Python 函数；可以使用点名称。角色文本不需要包括尾随括号以增强可读性；如果 [`add_function_parentheses`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-add_function_parentheses) 配置值为 `True` (默认值)，它们将由 Sphinx 自动添加。

`:py:data:`
:   引用模块级变量。

`:py:const:`
:   引用一个“定义的”常量。这可能是一个不打算更改的 Python 变量。

`:py:class:`
:   引用一个类；可以使用点名称。

`:py:meth:`
:   引用对象的方法。角色文本可以包括类型名称和方法名称；如果它出现在类型的描述中，则可以省略类型名称。可以使用点状名称。

`:py:attr:`
:   引用对象的数据属性。

   ```{note}
   这个角色也能够指的是属性。
   ```

`:py:exc:`
:   引用异常。可以使用点状名称。

`:py:obj:`
:  引用未指定类型的对象。例如，作为 [`default_role`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-default_role) 有用。
````

此标记中包含的名称可以包括模块名称和/或类名称。例如，`` :py:func:`filter`  `` 可以引用当前模块中名为 `filter` 的函数，或者该名称的内置函数。相比之下，`` :py:func:`foo.filter` `` 清楚地引用了 `foo` 模块中的 `filter` 函数。

通常，首先搜索这些角色中的名称而不进一步限定，然后将当前模块名称添加到前面，然后使用当前模块和类名(如果有)作为前缀。如果您在名称前加一个点，则此顺序相反。例如，在 Python 的文档 `codecs` 模块中，`` :py:func:`open` `` 总是引用内置函数，而 `` :py:func:`.open` `` 指 `codecs.open()` 。

类似的启发式方法用于确定名称是否是当前记录的类的属性。

此外，如果名称以点为前缀，并且未找到完全匹配，则将目标作为后缀，并搜索具有该后缀的所有对象名称。例如，`` :py:meth:`.TarFile.close` `` 引用 `tarfile.TarFile.close()` 函数，即使当前模块不是 `tarfile` 。由于这可能会变得模棱两可，如果有多个可能匹配，您将收到 Sphinx 的警告。

请注意，您可以组合使用 `~` 和 `.` 前缀: `` :py:meth:`~.TarFile.close` `` 将引用 `tarfile.TarFile.close()` 方法，但可见的链接标题只是 `close()`。

(rst:domains/c)=
## C 域

C 域（名称  **c**）适用于 C API 的文档。

`` .. c:member:: declaration `` & `` .. c:var:: declaration ``
:  描述了一个 C 结构的成员或变量。签名示例：

   ```rst
   .. c:member:: PyObject *PyTypeObject.tp_bases
   ```

   这两个指令之间的区别只是表面上的。

`` .. c:function:: function prototype ``
:  描述 C 函数。签名应如 C 所示，例如：

   ```rst
   .. c:function:: PyObject* PyType_GenericAlloc(PyTypeObject *type, Py_ssize_t nitems)
   ```

   这也用于描述类似函数的预处理器宏。应该给出参数的名称，以便它们可以在描述中使用。

请注意，签名中不必使用反斜杠转义星号，因为 reST 内联器不会对其进行解析。

`` .. c:macro:: name `` & `` .. c:macro:: name(arg list) ``
:  描述一个 C 语言的宏，即一个 C 语言的`#define`，没有替换文本。

`` .. c:struct:: name ``
:  描述 C 结构。

`` .. c:union:: name ``
:  描述 C union.

`` .. c:enum:: name ``
:  描述 C enum.

`` .. c:enumerator:: name ``
:  描述 C enumerator.

`` .. c:type:: typedef-like declaration `` & `` .. c:type:: name ``
:  描述一个 C 类型，可以是一个 typedef，也可以是一个未指定的类型的别名。

### 交叉引用 C 构造

如果在文档中定义了以下角色，则会创建对C语言构造的交叉引用:

- `:c:member:`
- `:c:data:`
- `:c:var:`
- `:c:func:`
- `:c:macro:`
- `:c:struct:`
- `:c:union:`
- `:c:enum:`
- `:c:enumerator:`
- `:c:type:`

引用一个 C 语言声明，如上定义。请注意，`c:member`、`c:data`和 `c:var` 是等同的。

### 匿名实体

C 语言支持匿名结构、枚举和联合体。为了便于记录，它们必须被赋予一些以 `@` 开头的名称，例如 `@42` 或 `@data`。这些名字也可以在交叉引用中使用，尽管即使省略了嵌套符号也会被发现。`@...` 的名字将总是被呈现为 `[anonymous]`（可能是一个链接）。

例子：

````{panels}
:container: w3-card-4 w3-pale-green w3-padding
:column: col-lg-6 px-2 py-2
---
:header: w3-pale-blue
RST
^^^
```rst
.. c:struct:: Data

   .. c:union:: @data

      .. c:var:: int a

      .. c:var:: double b

Explicit ref: :c:var:`Data.@data.a`. Short-hand ref: :c:var:`Data.a`.
```
---
:header: w3-pale-red
渲染：
^^^
```{eval-rst}
.. c:struct:: Data

   .. c:union:: @data

      .. c:var:: int a

      .. c:var:: double b

Explicit ref: :c:var:`Data.@data.a`. Short-hand ref: :c:var:`Data.a`.
```
````

### 别名声明

有时，将声明列在主要文档以外的地方可能会有帮助，例如，在创建一个接口的概要时。以下指令可用于此目的。

`` .. c:alias:: name ``
:  插入一个或多个别名声明。每个实体都可以像他们在 `c:any` 角色中那样被指定。例如：

   ````{panels}
   :container: w3-card-4 w3-pale-green w3-padding
   :column: col-lg-6 px-2 py-2
   ---
   :header: w3-pale-blue
   RST
   ^^^
   ```rst
   .. c:var:: int data2
   .. c:function:: int  f2(double k)

   .. c:alias:: data2
               f2
   ```
   ---
   :header: w3-pale-red
   渲染：
   ^^^
   ```{eval-rst}
   .. c:var:: int data2
   .. c:function:: int  f2(double k)

   .. c:alias:: data2
               f2
   ```
   ````

   **选项**

   `` :maxdepth: int ``
   :  也可以插入嵌套的声明，但要达到给定的总深度。使用0表示无限的深度，使用1表示只有提到的声明。默认为1。

   `` :noroot: ``
   :  跳过提到的声明，只渲染嵌套的声明。要求 `maxdepth` 为 0 或至少为 2。

### 内联表达式和类型

`:c:expr:` & `:c:texpr:`
:  插入一个 C 语言表达式或类型，可以是内联代码（`cpp:expr`）或内联文本（`cpp:texpr`）。比如说：

   ````{panels}
   :container: w3-card-4 w3-pale-green w3-padding
   :column: col-lg-6 px-2 py-2
   ---
   :header: w3-pale-blue
   RST
   ^^^
   ```rst
   .. c:var:: int b = 42

   .. c:function:: int f(int i)

   An expression: :c:expr:`b * f(b)` (or as text: :c:texpr:`b * f(b)`).

   A type: :c:expr:`const Data*`
   (or as text :c:texpr:`const Data*`).
   ```
   ---
   :header: w3-pale-red
   渲染：
   ^^^
   ```{eval-rst}
   .. c:var:: int b = 42

   .. c:function:: int f(int i)

   An expression: :c:expr:`b * f(b)` (or as text: :c:texpr:`b * f(b)`).

   A type: :c:expr:`const Data*`
   (or as text :c:texpr:`const Data*`).
   ```
   ````

### 名称空间

C 语言本身并不支持命名，但有时在文档中模仿它是很有用的，例如，显示备用的声明。该功能也可用于记录结构体/联合体/枚举体的成员，与它们的父声明分开。

当前的作用域可以通过三个命名空间指令来改变。它们管理一个堆栈声明，其中 `c:namespace` 重置了堆栈并改变了一个给定的作用域。

`c:namespace-push` 指令将作用域改变为当前作用域的一个给定的内部作用域。

`c:namespace-pop` 指令撤销最近的 `c:namespace-push` 指令。

`` .. c:namespace:: scope specification ``
:  将后续对象的当前作用域改为给定的作用域，并重置命名空间指令栈。注意，嵌套的作用域可以用点来分隔，例如：

   ```rst
   .. c:namespace:: Namespace1.Namespace2.SomeStruct.AnInnerStruct
   ```

   所有后续的对象都将被定义，就像它们的名字在声明时被预加了作用域一样。随后的交叉引用将从当前作用域开始搜索。

   使用 NULL 或 0 作为作用域将改变为全局作用域。

`` .. c:namespace-push:: scope specification ``
:  相对于当前的范围，改变范围。例如：

   ```rst
   .. c:namespace:: A.B

   .. c:namespace-push:: C.D
   ```

   目前的范围将是 `A.B.C.D`。

`` .. c:namespace-pop:: ``
:  撤销之前的 `c:namespace-push` 指令（不只是弹出一个范围）。例如：

   ```rst
   .. c:namespace:: A.B

   .. c:namespace-push:: C.D

   .. c:namespace-pop::
   ```

   当前的作用域将是 `A.B`（而不是 `A.B.C`）。

   如果之前没有使用 `c:namespace-push` 指令，而只是使用了 `c:namespace` 指令，那么当前作用域将被重置为全局作用域。也就是说，`` .. c:namespace:: A.B `` 相当于：

   ```rst
   .. c:namespace:: NULL

   .. c:namespace-push:: A.B
   ```

### 配置变量

参考：[Options for the C domain](https://www.sphinx-doc.org/en/master/usage/configuration.html#c-config)。

(rst:domains/cpp)=
## C++ 域

参见：[CPP Domains — Sphinx documentation (sphinx-doc.org)](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#cpp-domain)。

## 标准域

所谓的“标准”域收集所有不保证自己域名的标记。其指令和角色不以域名为前缀。

标准域也是使用 [`add_object_type()`](https://www.sphinx-doc.org/zh_CN/master/extdev/appapi.html#sphinx.application.Sphinx.add_object_type "sphinx.application.Sphinx.add_object_type") API 添加的自定义对象描述的位置。

有一组指令允许记录命令行程序:

`` .. option:: name args, name args, ... ``
:  描述命令行参数或开关。选项参数名称应括在尖括号中。例子：

   ```rst
   .. option:: dest_dir

      Destination directory.

   .. option:: -m <module>, --module <module>

      Run a module as a script.
   ```

   该指令将为给定的选项创建交叉引用目标，可通过以下方式引用 [`option`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html#role-option "option role") (在示例中，您将使用类似 `` :option:`dest_dir` ``, `` :option:`-m` ``, 要么 `` :option:`--module` ``)。

   `cmdoption` 指令是 `option` 指令的弃用别名。

`` .. envvar:: name ``
:  描述文档化代码或程序使用或定义的环境变量。可引用者 [`envvar`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html#role-envvar "envvar role") 。

`` .. program:: name ``
:  像 [`py:currentmodule`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-py:currentmodule "py:currentmodule directive") ，这个指令不产生输出。相反，它用于通知Sphinx所有以下内容 [`option`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-option "option directive") 指令文件选项称为 *name* 。

   如果你使用 [`program`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-program "program directive") ，你必须通过程序名来限定你的 [`option`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html#role-option "option role") 角色中的引用，所以如果你有以下情况：

   ```rst
   .. program:: rm

   .. option:: -r

      Work recursively.

   .. program:: svn

   .. option:: -r revision

      Specify the revision to work upon.
   ```

   然后 `` :option:`rm -r` `` 将引用第一个选项，而 `` :option:`svn -r` `` 将引用第二个选项。

   如果参数传递的是 `None`，该指令将重置当前的程序名称。

   程序名称可能包含空格（如果你想分别记录 `svn` 和 `svn commit` 这样的子命令）。

还有一个非常通用的对象描述指令，它不依赖于任何域：

`` .. describe:: text `` & `` .. object:: text ``
:  此伪指令生成与域提供的特定格式相同的格式，但不创建索引条目或交叉引用目标。例：

   ```rst
   .. describe:: PAPER

      You can set this variable to select a paper size.
   ```

(rst:domains/javascript)=
## JavaScript域

JavaScript域(名称  **js** )提供以下指令:

`` .. js:module:: name ``
:  该指令设置后面的对象声明的模块名称。 模块名称用于全局模块索引和交叉引用中。 该指令不会创建如下的对象标题 [`py:class`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-py:class "py:class directive") 。

   默认情况下，此指令将创建一个可链接的实体，并将在全局模块索引中生成一个条目，除非指定了 `noindex` 选项。如果指定了此选项，则该指令将仅更新当前模块名称。

`` .. js:function:: name(signature) ``
:  描述 JavaScript 函数或方法。如果要将参数描述为可选，请使用方括号 [documented](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#signatures) 用于 Python 签名。

   您可以使用字段来提供有关参数及其预期类型的更多详细信息，函数可能抛出的错误以及返回的值：

   ```rst
   .. js:function:: $.getJSON(href, callback[, errback])

      :param string href: An URI to the location of the resource.
      :param callback: Gets called with the object.
      :param errback:
         Gets called in case the request fails. And a lot of other
         text so we need multiple lines.
      :throws SomeError: For whatever reason in that case.
      :returns: Something.
   ```

   这表现为：

   ```{eval-rst}
   .. js:function:: $.getJSON(href, callback[, errback])

      :param string href: An URI to the location of the resource.
      :param callback: Gets called with the object.
      :param errback:
         Gets called in case the request fails. And a lot of other
         text so we need multiple lines.
      :throws SomeError: For whatever reason in that case.
      :returns: Something.
   ```

`` .. js:method:: name(signature) ``
:  该指令是以下的别名 [`js:function`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-js:function "js:function directive") ，但是它描述了一个作为类对象上的方法实现的函数。

`` .. js:class:: name ``
:  描述创建对象的构造函数。这基本上就像一个函数，但会出现一个 `class` 前缀:

   ```rst
   .. js:class:: MyAnimal(name[, age])

      :param string name: The name of the animal
      :param number age: an optional age for the animal
   ```

   这表现为：

   ```{eval-rst}
   .. js:class:: MyAnimal(name[, age])

      :param string name: The name of the animal
      :param number age: an optional age for the animal
   ```

`` .. js:data:: name ``
:  描述全局变量或常量。

`` .. js:attribute:: object.name ``
:  描述 *object* 的属性 *name*。

提供这些角色是为了引用所描述的对象:

- `:js:mod:`
- `:js:func:`
- `:js:meth:`
- `:js:class:`
- `:js:data:`
- `:js:attr:`

(rst:domains/rst)=
## reStructuredText 域

reStructuredText域（名称 **rst**）提供以下指令:

`` .. rst:directive:: name ``
:  描述 reST 指令。*name* 可以是单个指令名称或实际指令语法（`..` 前缀和 `::` 后缀），其参数将以不同方式呈现。例如：

   ````{panels}
   :container: w3-card-4 w3-pale-green w3-padding
   :column: col-lg-6 px-2 py-2
   ---
   :header: w3-pale-blue
   RST
   ^^^
   ```rst
   .. rst:directive:: foo

      Foo description.

   .. rst:directive:: .. bar:: baz

      Bar description.
   ```
   ---
   :header: w3-pale-red
   渲染：
   ^^^
   ```{eval-rst}
   .. rst:directive:: foo

      Foo description.

   .. rst:directive:: .. bar:: baz

      Bar description.
   ```
   ````

`` .. rst:directive:option:: name ``:
:  描述 reST 角色。例如：

   ````{panels}
   :container: w3-card-4 w3-pale-green w3-padding
   :column: col-lg-6 px-2 py-2
   ---
   :header: w3-pale-blue
   RST
   ^^^
   ```rst
   .. rst:role:: foo

      Foo description.
   ```
   ---
   :header: w3-pale-red
   渲染：
   ^^^
   ```{eval-rst}
   .. rst:role:: foo

      Foo description.
   ```
   ````

   **选项**
   `:type:` 描述参数 (文本)

   例子：

   ````{panels}
   :container: w3-card-4 w3-pale-green w3-padding
   :column: col-lg-6 px-2 py-2
   ---
   :header: w3-pale-blue
   RST
   ^^^
   ```rst
   .. rst:directive:: toctree

   .. rst:directive:option:: maxdepth
      :type: integer or no value
   ```
   ---
   :header: w3-pale-red
   渲染：
   ^^^
   ```{eval-rst}
   .. rst:directive:: toctree

   .. rst:directive:option:: maxdepth
      :type: integer or no value
   ```
   ````

提供这些角色是为了引用所描述的对象：

- `:rst:dir:`
- `:rst:role:`

(rst:domains/math)=
## 数学域

数学域（名称  **math**）提供以下角色：

`` :math:numref: ``
:  作用是通过数学指令的标签来交叉引用数学指令所定义的方程。例子：

   ````{panels}
   :container: w3-card-4 w3-pale-green w3-padding
   :column: col-lg-6 px-2 py-2
   ---
   :header: w3-pale-blue
   RST
   ^^^
   ```rst
   .. math:: e^{i\pi} + 1 = 0
      :label: euler

   Euler's identity, equation :math:numref:`euler`, was elected one of the
   most beautiful mathematical formulas.
   ```
   ---
   :header: w3-pale-red
   渲染：
   ^^^
   ```{eval-rst}
   .. math:: e^{i\pi} + 1 = 0
      :label: euler

   Euler's identity, equation :math:numref:`euler`, was elected one of the
   most beautiful mathematical formulas.
   ```
   ````

## 更多域

[sphinx-contrib](https://bitbucket.org/birkenfeld/sphinx-contrib/) 存储库包含更多可用作扩展的域；目前有 [Ada](https://pypi.org/project/sphinxcontrib-adadomain/), [CoffeeScript](https://pypi.org/project/sphinxcontrib-coffee/), [Erlang](https://pypi.org/project/sphinxcontrib-erlangdomain/), [HTTP](https://pypi.org/project/sphinxcontrib-httpdomain/), [Lasso](https://pypi.org/project/sphinxcontrib-lassodomain/), [MATLAB](https://pypi.org/project/sphinxcontrib-matlabdomain/), [PHP](https://pypi.org/project/sphinxcontrib-phpdomain/), 和 [Ruby](https://bitbucket.org/birkenfeld/sphinx-contrib/src/default/rubydomain) 域。另外还有 [Chapel](https://pypi.org/project/sphinxcontrib-chapeldomain/), [Common Lisp](https://pypi.org/project/sphinxcontrib-cldomain/), [dqn](https://pypi.org/project/sphinxcontrib-dqndomain/), [Go](https://pypi.org/project/sphinxcontrib-golangdomain/), [Jinja](https://pypi.org/project/sphinxcontrib-jinjadomain/), [Operation](https://pypi.org/project/sphinxcontrib-operationdomain/), 和 [Scala](https://pypi.org/project/sphinxcontrib-scaladomain/) 的域。
