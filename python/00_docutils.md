# Docutils

````{div} w3-pale-green w3-padding
```{glossary}
[Docutils](https://docutils.sourceforge.io/)
    Docutils 是一个模块化系统，用于将文档处理成有用的格式，如 HTML、XML 和 LaTeX。对于输入，Docutils 支持 reStructuredText，一种易于阅读的、所见即所得（what-you-see-is-what-you-get）的纯文本（plaintext）标记语法。
    
    详细内容见 [Docutils 文档](https://xinetzone.github.io/docutils/)。

[reStructuredText](https://docutils.sourceforge.io/rst.html)
    {term}`Docutils` 的**标记语法和解析器组件**。reStructuredText（RST，ReST 或 reST）是一个易于阅读、所见即所得的纯文本标记语法和解析器系统。它对于 in-line 程序文档（如 Python 文档字符串）、快速创建简单网页和独立文档都很有用。reStructuredText 专为特定应用程序域的可扩展性而设计。reStructuredText 解析器是 Docutils 的一个组件。reStructuredText 是 [StructuredText](http://dev.zope.org/Members/jim/StructuredTextWiki/FrontPage/) 和 [Setext](https://docutils.sourceforge.io/mirror/setext.html) 轻量级标记系统的修订和重新解释。
    
    reStructuredText 的主要目标是定义和实现在 Python 文档字符串和其他文档域中使用的标记语法，该语法可读且简单，但对于非平凡的使用来说功能足够强大。标记的目的是将 reStructuredText 文档转换为有用的结构化数据格式。
```
````

## RST 语法

### Headers

```
Section Header
==============

Subsection Header
-----------------
```

### Lists

```
- A bullet list item
- Second item

  - A sub item

- Spacing between items separates list items

* Different bullet symbols create separate lists

- Third item

1) An enumerated list item

2) Second item

   a) Sub item that goes on at length and thus needs
      to be wrapped. Note the indentation that must
      match the beginning of the text, not the 
      enumerator.

      i) List items can even include

         paragraph breaks.

3) Third item

#) Another enumerated list item

#) Second item
```

### Images

```
.. image:: /path/to/image.jpg
```

### Named links

```
A sentence with links to `Wikipedia`_ and the `Linux kernel archive`_.

.. _Wikipedia: https://www.wikipedia.org/
.. _Linux kernel archive: https://www.kernel.org/
```

### Anonymous links

```
Another sentence with an `anonymous link to the Python website`__.

__ https://www.python.org/
```

N.B.: named links and anonymous links are enclosed in grave accents (`), and not in apostrophes (').

### Literal blocks

```
::

  some literal text

This may also be used inline at the end of a paragraph, like so::

  some more literal text

.. code:: python

   print("A literal block directive explicitly marked as python code")
```
