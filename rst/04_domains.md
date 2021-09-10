(rst:domains)=
# 域（raw）

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
## C 域（待更）

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

(rst:domains/cpp)=
## C++ 域（待更）

C++ 域（名称  **cpp**）支持记录 C++ 项目。

### 声明实体的指令

以下指令可用。所有声明都可以从可见性声明开始（`public` ，`private` 或 `protected`）：

`.. cpp:class::``<span> class specifier`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-cpp:class "永久链接至目标")`.. cpp:struct::``<span> class specifier`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-cpp:struct "永久链接至目标")描述一个类/结构，可能带有继承规范，例如:

```
.. cpp:class:: MyClass : public MyBase, MyOtherBase
```

区别 [`cpp:class`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-cpp:class "cpp:class directive") 和 [`cpp:struct`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-cpp:struct "cpp:struct directive") 只是装饰:输出中呈现的前缀，以及索引中显示的说明符。

该类可以直接在嵌套范围内声明，例如:

```
.. cpp:class:: OuterScope::MyClass : public MyBase, MyOtherBase
```

可以声明一个类模板:

```
.. cpp:class:: template<typename T, std::size_t N> std::array
```

或者换行:

```
.. cpp:class:: template<typename T, std::size_t N> \
               std::array
```

可以声明完整和部分模板特化:

```
.. cpp:class:: template<> \
               std::array<bool, 256>

.. cpp:class:: template<typename T> \
               std::array<T, 42>
```

**2.0 新版功能:** [`cpp:struct`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-cpp:struct "cpp:struct directive") 指令。

`.. cpp:function::``<span> (member) function prototype`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-cpp:function "永久链接至目标")描述一个函数或成员函数，例如:

```
.. cpp:function:: bool myMethod(int arg1, std::string arg2)

   A function with parameters and types.

.. cpp:function:: bool myMethod(int, double)

   A function with unnamed parameters.

.. cpp:function:: const T &MyClass::operator[](std::size_t i) const

   An overload for the indexing operator.

.. cpp:function:: operator bool() const

   A casting operator.

.. cpp:function:: constexpr void foo(std::string &bar[2]) noexcept

   A constexpr function.

.. cpp:function:: MyClass::MyClass(const MyClass&) = default

   A copy constructor with default implementation.
```

函数模板也可以描述:

```
.. cpp:function:: template<typename U> \
                  void print(U &&u)
```

和函数模板专业化:

```
.. cpp:function:: template<> \
                  void print(int i)
```

`.. cpp:member::``<span> (member) variable declaration`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-cpp:member "永久链接至目标")`.. cpp:var::``<span> (member) variable declaration`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-cpp:var "永久链接至目标")描述变量或成员变量，例如:

```
.. cpp:member:: std::string MyClass::myMember

.. cpp:var:: std::string MyClass::myOtherMember[N][M]

.. cpp:member:: int a = 42
```

变量模板也可以描述:

```
.. cpp:member:: template<class T> \
                constexpr T pi = T(3.1415926535897932385)
```

`.. cpp:type::``<span> typedef declaration`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-cpp:type "永久链接至目标")`.. cpp:type::``<span> name``.. cpp:type::``<span> type alias declaration`描述typedef声明中的类型，类型别名声明，或者只是具有未指定类型的类型的名称，例如:

```
.. cpp:type:: std::vector<int> MyList

   A typedef-like declaration of a type.

.. cpp:type:: MyContainer::const_iterator

   Declaration of a type alias with unspecified type.

.. cpp:type:: MyType = std::unordered_map<int, std::string>

   Declaration of a type alias.
```

类型别名也可以模板化:

```
.. cpp:type:: template<typename T> \
              MyContainer = std::vector<T>
```

该示例呈现如下。

*typedef *std::vector`<int>` `MyList`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv46MyList "永久链接至目标")
类型的typedef式声明。

*type *`MyContainer<code class="sig-prename descclassname">::</code>``const_iterator`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4N11MyContainer14const_iteratorE "永久链接至目标")
声明具有未指定类型的类型别名。

*using *`MyType` = std::unordered_map<int, std::string>[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv46MyType "永久链接至目标")
声明类型别名。

template<typename `T`>
*using *`MyContainer` = std::vector<[T](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0E11MyContainer "MyContainer::T")>[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0E11MyContainer "永久链接至目标")
`.. cpp:enum::``<span> unscoped enum declaration`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-cpp:enum "永久链接至目标")`.. cpp:enum-struct::``<span> scoped enum declaration`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-cpp:enum-struct "永久链接至目标")`.. cpp:enum-class::``<span> scoped enum declaration`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-cpp:enum-class "永久链接至目标")描述(范围)枚举，可能具有指定的基础类型。在unscoped枚举中声明的任何枚举器都将在枚举范围和父范围内声明。例子:

```
.. cpp:enum:: MyEnum

   An unscoped enum.

.. cpp:enum:: MySpecificEnum : long

   An unscoped enum with specified underlying type.

.. cpp:enum-class:: MyScopedEnum

   A scoped enum.

.. cpp:enum-struct:: protected MyScopedVisibilityEnum : std::underlying_type<MySpecificEnum>::type

   A scoped enum with non-default visibility, and with a specified underlying type.
```

`.. cpp:enumerator::``<span> name`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-cpp:enumerator "永久链接至目标")`.. cpp:enumerator::``<span> name = constant`描述一个枚举器，可选择定义其值，例如:

```
.. cpp:enumerator:: MyEnum::myEnumerator

.. cpp:enumerator:: MyEnum::myOtherEnumerator = 42
```

`.. cpp:union::``<span> name`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-cpp:union "永久链接至目标")描述一个联盟。

**1.8 新版功能.**

`.. cpp:concept::``<span> template-parameter-list name`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-cpp:concept "永久链接至目标")警告

对概念的支持是实验性的。它基于当前的标准草案和概念技术规范。这些功能可能随着它们的发展而变化。

描述一个概念。它必须有1个模板参数列表。名称可以是嵌套名称。例:

```
.. cpp:concept:: template<typename It> std::Iterator

   Proxy to an element of a notional sequence that can be compared,
   indirected, or incremented.

   **Notation**

   .. cpp:var:: It r

      An lvalue.

   **Valid Expressions**

   - :cpp:expr:`*r`, when :cpp:expr:`r` is dereferenceable.
   - :cpp:expr:`++r`, with return type :cpp:expr:`It&`, when :cpp:expr:`r` is incrementable.
```

这将呈现如下:

template<typename `It`>
*concept *`std<code class="sig-prename descclassname">::</code>``Iterator`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0ENSt8IteratorE "永久链接至目标")
代理到可以比较，间接或增量的有理序列的元素。

**Notation**

[It](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0ENSt8IteratorE "std::Iterator::It") `r`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4NSt8Iterator1rE "永久链接至目标")
一个左值。

**Valid Expressions**

* `*<a class="reference internal" href="https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4NSt8Iterator1rE" title="std::Iterator::r">r</a>`, 当 `<a class="reference internal" href="https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4NSt8Iterator1rE" title="std::Iterator::r">r</a>` 是可解除引用的。
* `++<a class="reference internal" href="https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4NSt8Iterator1rE" title="std::Iterator::r">r</a>` ，返回类型 `<a class="reference internal" href="https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0ENSt8IteratorE" title="std::Iterator::It">It</a>&`, when `<a class="reference internal" href="https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4NSt8Iterator1rE" title="std::Iterator::r">r</a>` 是可递增的。

**1.5 新版功能.**

#### 选项

一些指令支持选项:

* `:noindex:` , 看到 [基本标记](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#basic-domain-markup).
* `:tparam-line-spec:` ，用于模板化声明。如果指定，则每个模板参数将在单独的行上呈现。**1.6 新版功能.**

### 匿名实体

C++ 支持匿名命名空间，类，枚举和联合。为了文档起见，必须给它们一些以 `@` 开头的名字，例如 `@42` 或 `@data` 。这些名称也可用于交叉引用和(类型)表达式，但即使省略也会找到嵌套符号。 `@<span> ...` 名称将始终显示为 **[anonymous]** (可能作为链接)。

例:

```
.. cpp:class:: Data

   .. cpp:union:: @data

      .. cpp:var:: int a

      .. cpp:var:: double b

Explicit ref: :cpp:var:`Data::@data::a`. Short-hand ref: :cpp:var:`Data::a`.
```

这将呈现为:

*class *`Data`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv44Data "永久链接至目标")
*union * **[anonymous]**[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4N4DataUt4_dataE "永久链接至目标")
int `a`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4N4DataUt4_data1aE "永久链接至目标")
double `b`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4N4DataUt4_data1bE "永久链接至目标")
显式ref: [`Data::[anonymous]::a`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4N4DataUt4_data1aE "Data::[anonymous]::a") 。简写ref: [`Data::a`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4N4DataUt4_data1aE "Data::[anonymous]::a") 。

**1.8 新版功能.**

### 别名声明

有时，除了主要文档之外，它可能是有用的列表声明，例如，在创建类接口的概要时。以下指令可用于此目的。

`.. cpp:alias::``<span> name or function signature`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-cpp:alias "永久链接至目标")插入一个或多个别名声明。可以在 [`cpp:any`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:any "cpp:any role") 角色中指定每个实体。如果给出了函数的名称(而不是完整的签名)，那么将列出函数的所有重载。

例如:

```
.. cpp:alias:: Data::a
               overload_example::C::f
```

becomes

int [a](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4N4DataUt4_data1aE "Data::[anonymous]::a")
void [f](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4NK16overload_example1C1fEd "overload_example::C::f")(double d**)** *const*
void [f](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4NK16overload_example1C1fEd "overload_example::C::f")(double d**)**
void [f](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4NK16overload_example1C1fEd "overload_example::C::f")(int i**)**
void [f](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4NK16overload_example1C1fEd "overload_example::C::f")(**)**
whereas:

```
.. cpp:alias:: void overload_example::C::f(double d) const
               void overload_example::C::f(double d)
```

becomes

void [f](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4NK16overload_example1C1fEd "overload_example::C::f")(double d**)** *const*
void [f](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4NK16overload_example1C1fEd "overload_example::C::f")(double d**)**
**2.0 新版功能.**

### 约束模板

警告

对概念的支持是实验性的。它基于当前的标准草案和概念技术规范。这些功能可能随着它们的发展而变化。

注解

Sphinx目前不支持 `requires` 条款。

#### 占位符

声明可以使用概念的名称来引入受约束的模板参数，或者使用关键字“auto”来引入无约束的模板参数:

```
.. cpp:function:: void f(auto &&arg)

   A function template with a single unconstrained template parameter.

.. cpp:function:: void f(std::Iterator it)

   A function template with a single template parameter, constrained by the
   Iterator concept.
```

#### 模板介绍

可以使用 template introduction 而不是模板参数列表声明简单约束函数或类模板:

```
.. cpp:function:: std::Iterator{It} void advance(It &it)

    A function template with a template parameter constrained to be an
    Iterator.

.. cpp:class:: std::LessThanComparable{T} MySortedContainer

    A class template with a template parameter constrained to be
    LessThanComparable.
```

它们呈现如下。

std::[Iterator](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0ENSt8IteratorE "std::Iterator"){`It`}
void `advance`([It](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0EXNSt8IteratorEI2ItEE7advancevR2It "advance::It") & *it* **)**[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0EXNSt8IteratorEI2ItEE7advancevR2It "永久链接至目标")
具有模板参数的函数模板被约束为迭代器。

std::LessThanComparable{`T`}
*class *`MySortedContainer`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0EXNSt18LessThanComparableEI1TEE17MySortedContainer "永久链接至目标")
具有模板参数的类模板约束为 LessThanComparable。

但请注意，不会对参数兼容性进行检查。例如，`Iterator{A，B，C}` 将被接受作为介绍，即使它不是有效的 C++。

### 内联表达式和类型

`:cpp:expr:`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:expr "永久链接至目标")`:cpp:texpr:`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:texpr "永久链接至目标")插入C++ 表达式或键入内联代码 (`cpp:expr`) 或内联文本 (`cpp:texpr`) 。例如:

```
.. cpp:var:: int a = 42

.. cpp:function:: int f(int i)

An expression: :cpp:expr:`a * f(a)` (or as text: :cpp:texpr:`a * f(a)`).

A type: :cpp:expr:`const MySortedContainer<int>&`
(or as text :cpp:texpr:`const MySortedContainer<int>&`).
```

将呈现如下:

int `a` = 42[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv41a "永久链接至目标")
int `f`(int  *i* **)**[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv41fi "永久链接至目标")
表达式: `<a class="reference internal" href="https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv41a" title="a">a</a><span> *<span> <a class="reference internal" href="https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv41fi" title="f">f</a>(<a class="reference internal" href="https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv41a" title="a">a</a>)` (或文本: [a](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv41a "a") ***** [f](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv41fi "f")([a](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv41a "a")))。

类型: `<em class="property">const</em><span> <a class="reference internal" href="https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0EXNSt18LessThanComparableEI1TEE17MySortedContainer" title="MySortedContainer">MySortedContainer</a><int>&` (或作为文本 *const* [MySortedContainer](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0EXNSt18LessThanComparableEI1TEE17MySortedContainer "MySortedContainer")`<int>`&)。

**1.7 新版功能:** [`cpp:expr`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:expr "cpp:expr role") 角色。

**1.8 新版功能:** [`cpp:texpr`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:texpr "cpp:texpr role") 角色。

### 命名空间

C++ 域中的声明默认放在全局范围内。可以使用三个命名空间指令更改当前范围。他们管理堆栈声明，其中 `cpp:namespace` 重置堆栈并更改给定的范围。“

`cpp:namespace-push` 指令将范围更改为当前范围的给定内部范围。

`cpp:namespace-pop` 指令撤消了最新的 `cpp:namespace-push` 指令。

`.. cpp:namespace::``<span> scope specification`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-cpp:namespace "永久链接至目标")将后续对象的当前范围更改为给定范围，并重置命名空间指令堆栈。请注意，命名空间不需要与C++ 命名空间相对应，但可以以类的名称结尾，例如:

```
.. cpp:namespace:: Namespace1::Namespace2::SomeClass::AnInnerClass
```

将定义所有后续对象，就好像它们的名称是在前置范围的情况下声明的一样。将在当前范围中搜索后续交叉引用。

使用 `NULL` ， `0` 或 `nullptr` 作为范围将变为全局范围。

命名空间声明也可以模板化，例如:

```
.. cpp:class:: template<typename T> \
               std::vector

.. cpp:namespace:: template<typename T> std::vector

.. cpp:function:: std::size_t size() const
```

将 `size` 声明为类模板 `std<span> ::<span> vector` 的成员函数。等价地，这可以使用:

```
.. cpp:class:: template<typename T> \
               std::vector

   .. cpp:function:: std::size_t size() const
```

要么:

```
.. cpp:class:: template<typename T> \
               std::vector
```

`.. cpp:namespace-push::``<span> scope specification`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-cpp:namespace-push "永久链接至目标")相对于当前范围更改范围。例如，之后:

```
.. cpp:namespace:: A::B

.. cpp:namespace-push:: C::D
```

the current scope will be `A::B::C::D` .

**1.4 新版功能.**

`.. cpp:namespace-pop::`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#directive-cpp:namespace-pop "永久链接至目标")撤消之前的 `cpp:namespace-push` 指令(*not* 只是弹出作用域)。例如，之后:

```
.. cpp:namespace:: A::B

.. cpp:namespace-push:: C::D

.. cpp:namespace-pop::
```

当前范围将是 `A<span> ::<span> B` (*not* `A::B::C`)。

如果没有使用先前的 `cpp:namespace-push` 指令，但只使用 `cpp:namespace` 指令，则当前作用域将重置为全局作用域。也就是说，`cpp:namespace<span> ::<span> A<span> ::<span> B` 相当于:

```
.. cpp:namespace:: nullptr

.. cpp:namespace-push:: A::B
```

**1.4 新版功能.**

### 信息字段列表

C++ 指令支持以下信息字段(另请参阅 [信息字段列表](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#info-field-lists) ):

* param, parameter, arg, argument: 参数说明。
* tparam:模板参数的描述。
* returns, return:返回值的描述。
* throws, throw, exception: 可能抛出的异常的描述。

### 交叉引用

这些角色链接到给定的声明类型:

`:cpp:any:`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:any "永久链接至目标")`:cpp:class:`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:class "永久链接至目标")`:cpp:struct:`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:struct "永久链接至目标")`:cpp:func:`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:func "永久链接至目标")`:cpp:member:`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:member "永久链接至目标")`:cpp:var:`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:var "永久链接至目标")`:cpp:type:`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:type "永久链接至目标")`:cpp:concept:`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:concept "永久链接至目标")`:cpp:enum:`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:enum "永久链接至目标")`:cpp:enumerator:`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:enumerator "永久链接至目标")按名称引用C++ 声明(有关详细信息，请参见下文)。名称必须相对于链接的位置适当限定。

**2.0 新版功能:** [`cpp:struct`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:struct "cpp:struct role") 角色充当 [`cpp:class`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:class "cpp:class role") 角色的别名。

有关模板参数/参数的引用的注释

这些角色遵循Sphinx [交叉引用语法](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/roles.html#xref-syntax) 规则。这意味着在引用(部分)模板专业化时必须小心，例如:如果链接看起来像这样: `:cpp:class:`MyClass`<span>` `<int>``` 。这被解释为 `int` 的链接，标题为 `MyClass` 。在这种情况下，用反斜杠转义开口尖括号，如下所示: `:cpp:class:`MyClass\<int>`` 。

当不需要自定义标题时，使用内联表达式的角色可能很有用 [`cpp:expr`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:expr "cpp:expr role") 和 [`cpp:texpr`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:texpr "cpp:texpr role") ，其中尖括号不需要转义。

#### 没有模板参数和模板参数的声明

对于链接到非模板化声明，名称必须是嵌套名称，例如 `f` 或 `MyClass::f` 。

#### 重载(成员)函数

当仅使用其名称引用(成员)函数时，引用将指向任意匹配的重载。 [`cpp:any`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:any "cpp:any role") 和 [`cpp:func`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:func "cpp:func role") roles 使用另一种格式，它只是一个完整的函数声明。这将解决完全匹配的重载。例如，请考虑以下类声明:

*class *`C`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4N16overload_example1CE "永久链接至目标")
void `f`(double  *d* **)** *const*[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4NK16overload_example1C1fEd "永久链接至目标")
void `f`(double  *d* **)**[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4N16overload_example1C1fEd "永久链接至目标")
void `f`(int  *i* **)**[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4N16overload_example1C1fEi "永久链接至目标")
void `f`(**)**[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4N16overload_example1C1fEv "永久链接至目标")
引用使用 [`cpp:func`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#role-cpp:func "cpp:func role") 角色:

* 任意重载: `C::f` ， [`C::f()`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4NK16overload_example1C1fEd "overload_example::C::f")
* Also arbitrary overload: `C::f()`, [`C::f()`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4NK16overload_example1C1fEd "overload_example::C::f")
* 特定的重载: `void<span> C::f()` ， [`void<span> C::f()`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4N16overload_example1C1fEv "void C::f()")
* 具体超负荷: `void<span> C::f(int)`, [`void<span> C::f(int)`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4N16overload_example1C1fEi "void C::f(int)")
* 具体超负荷: `void<span> C::f(double)` , [`void<span> C::f(double)`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4N16overload_example1C1fEd "void C::f(double)")
* 具体超负荷: `void<span> C::f(double)<span> const` , [`void<span> C::f(double)<span> const`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4NK16overload_example1C1fEd "void C::f(double) const")

请注意 [`add_function_parentheses`](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#confval-add_function_parentheses) 配置变量不会影响特定的重载引用。

#### 模板化的声明

假设以下声明。

*class *`Wrapper`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv47Wrapper "永久链接至目标")
template<typename `TOuter`>
*class *`Outer`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0EN7Wrapper5OuterE "永久链接至目标")
template<typename `TInner`>
*class *`Inner`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0EN7Wrapper5Outer5InnerE "永久链接至目标")
通常，引用必须包含模板参数声明和限定名称前缀的模板参数。例如:

* `template\<typename<span> TOuter><span> Wrapper::Outer` ([`template<typename<span> TOuter><span> Wrapper::Outer`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0EN7Wrapper5OuterE "Wrapper::Outer"))
* `template\<typename<span> TOuter><span> template\<typename<span> TInner><span> Wrapper::Outer<TOuter>::Inner` (`template<typename<span> TOuter><span> template<typename<span> TInner><span> Wrapper::Outer<TOuter>::Inner`)

目前，如果模板参数标识符是相等的字符串，则查找仅成功。也就是说，`template\<typename<span> UOuter><span> Wrapper<span> ::<span> Outer` 将不起作用。

作为简写表示法，如果省略模板参数列表，则查找将采用主模板或非模板，而不是部分模板特化。这意味着以下参考资料也有效:“

* `Wrapper::Outer` ([`Wrapper::Outer`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0EN7Wrapper5OuterE "Wrapper::Outer"))
* `Wrapper::Outer::Inner` ([`Wrapper::Outer::Inner`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0EN7Wrapper5Outer5InnerE "Wrapper::Outer::Inner"))
* `template\<typename<span> TInner><span> Wrapper::Outer::Inner` ([`template<typename<span> TInner><span> Wrapper::Outer::Inner`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0EN7Wrapper5Outer5InnerE "Wrapper::Outer::Inner"))

#### (完整)模板专业化

假设以下声明。

template<typename `TOuter`>
*class *`Outer`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0E5Outer "永久链接至目标")
template<typename `TInner`>
*class *`Inner`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0EN5Outer5InnerE "永久链接至目标")
template<>
*class *`Outer<int>`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4IE5OuterIiE "永久链接至目标")
template<typename `TInner`>
*class *`Inner`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0EN5OuterIiE5InnerE "永久链接至目标")
template<>
*class *`Inner<bool>`[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4IEN5OuterIiE5InnerIbEE "永久链接至目标")
通常，引用必须包含每个模板参数列表的模板参数列表。 因此可以参考上面的完整专业化 `template\<><span> Outer\<int>` ([`template<><span> Outer<int>`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4IE5OuterIiE "Outer<int>")) and `template\<><span> template\<><span> Outer\<int>::Inner\<bool>` ([`template<><span> template<><span> Outer<int>::Inner<bool>`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4IEN5OuterIiE5InnerIbEE "Outer<int>::Inner<bool>")). 作为简写，可以省略空模板参数列表，例如， `Outer\<int>` ([`Outer<int>`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4IE5OuterIiE "Outer<int>")) and `Outer\<int>::Inner\<bool>` ([`Outer<int>::Inner<bool>`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4IEN5OuterIiE5InnerIbEE "Outer<int>::Inner<bool>")).

#### 部分模板专业化

假设以下声明。

template<typename `T`>
*class *`Outer`<[T](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0E5OuterIP1TE "Outer<T *>::T") *>[](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0E5OuterIP1TE "永久链接至目标")
对部分特化的引用必须始终包含模板参数列表，例如 `template\<typename<span> T><span> Outer\<T<span> *>` ([`template<typename<span> T><span> Outer<<span> T<span> *>`](https://www.sphinx-doc.org/zh_CN/master/usage/restructuredtext/domains.html#_CPPv4I0E5OuterIP1TE "Outer<T *>"))。目前，只有当模板参数标识符是相等的字符串时，查找才会成功。“

### 配置变量

请参阅 [C++ 域选项](https://www.sphinx-doc.org/zh_CN/master/usage/configuration.html#cpp-config) 。

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
