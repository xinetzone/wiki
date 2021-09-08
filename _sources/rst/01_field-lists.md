# 字段列表

[如前所述](rst:field-lists)，字段列表是标记为这样的字段序列：

```rst
:fieldname: Field content
```

{term}`Sphinx` 为字段列表扩展了标准的 docutils 行为，并增加了一些额外的功能，这些功能在本节中有所涉及。

```{note}
字段列表的值将被解析为字符串。你不能使用 Python 集合，如列表或字典。
```

## 整个文件的元数据

靠近文件顶部的字段列表通常被 docutils 解析为 *docinfo* 并显示在页面上。然而，在 Sphinx 中，在任何其他标记之前的字段列表被从 docinfo 移到 Sphinx 环境中作为文档元数据，并且不在输出中显示。

```{note}
出现在文档标题之后的字段列表将作为正常的 docinfo 的一部分，并将在输出中显示。
```

## 特殊元数据字段

与 docutils 相比，Sphinx 为 bibliographic 字段提供自定义行为。

目前，这些元数据字段被识别：

`tocdepth`
:   文件的目录的最大深度。

    ```rst
    :tocdepth: 2
    ```

    ```{note}
    这个元数据影响到本地 `toctree` 的深度。但它对全局树的深度没有影响。所以这不会改变一些使用全局的主题的侧边栏。
    ```

`nocomments`
:   如果设置了，网络应用程序将不会为从该源文件生成的页面显示评论表单。
    ```rst
    :nocomments:
    ```

`orphan`
:   如果设置，关于此文件未被包含在任何 `toctree` 中的警告将被抑制。

    ```rst
    :orphan:
    ```

`nosearch`
:   如果设置，该文件的全文搜索被禁用。

    ```rst
    :nosearch:
    ```

    ```{note}
    即使设置了 `nosearch` 选项，对象搜索仍然可用。
    ```
