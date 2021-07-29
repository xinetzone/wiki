# 开发提示和技巧

本节提供了一些在可执行图书项目生态系统中开发项目的有用提示和技巧。

## 使用 Sphinx

[sphinx 开发指南](https://www.sphinx-doc.org/en/master/develop.html) 也是理解 Sphinx 如何工作的有用资源。

### 访问 Sphinx 抽象语法树（AST）

要理解 Sphinx 如何组织文档，访问抽象语法树（AST）的 `xml` 表示是非常重要的一步。

获取此信息的一种方法是从项目的 `_build` 目录中包含的 `.doctree` 目录。

一旦你构建了一个示例项目，你就可以通过在 Python 中加载经过 `pickle` 的文档树来访问 AST：

```python
import pickle
doc = pickle.load(open("_build/.doctrees/<file>", "rb"))
```

获取用于测试目的的伪 `xml` 表示：

```python
pseudoxml = doc.pformat()
```

并获得完整的 `xml`：

```python
xml = doc.asdom().toprettyxml()
```