# 安装工具集成

参考：[Setuptools integration — Sphinx documentation (sphinx-doc.org)](https://www.sphinx-doc.org/zh_CN/master/usage/advanced/setuptools.html)

Sphinx 支持通过自定义命令 `BuildDoc` 与 `setuptools` 和 `distutils` 整合。

## 使用安装工具集成

Sphinx 的构建可以从 distutils 触发，一些 Sphinx 的选项可以在 `setup.py` 或 `setup.cfg` 中设置，而不是 Sphinx 自己的配置文件。

例如，从`setup.py`：

```python
# this is only necessary when not using setuptools/distribute
from sphinx.setup_command import BuildDoc
cmdclass = {'build_sphinx': BuildDoc}

name = 'My project'
version = '1.2'
release = '1.2.0'
setup(
    name=name,
    author='Bernard Montgomery',
    version=release,
    cmdclass=cmdclass,
    # these are optional and override conf.py settings
    command_options={
        'build_sphinx': {
            'project': ('setup.py', name),
            'version': ('setup.py', version),
            'release': ('setup.py', release),
            'source_dir': ('setup.py', 'doc')}},
)
```

```{note}
如果你直接在 `setup()` 命令中设置 Sphinx 选项，把变量名中的连字符替换为下划线。在上面的例子中，`source-dir` 变成 `source_dir`。
```

或者在 `setup.cfg` 中添加：

```cfg
[build_sphinx]
project = 'My project'
version = 1.2
release = 1.2.0
source-dir = 'doc'
```

配置好后，通过调用 `setup.py` 上的相关命令来调用：

```sh
$ python setup.py build_sphinx
```

## 安装工具集成选项

`fresh-env`
:   一个布尔值，决定在构建时是否应该丢弃保存的环境。默认为 `false`。

    这也可以通过向 `setup.py` 传递 `-E` 标志来设置：

    ```sh
    $ python setup.py build_sphinx -E
    ```

`all-files`
:   一个布尔值，决定是否所有文件都要从头开始建立。默认为 `false`。

    这也可以通过向 `setup.py` 传递 `-a` 标志来设置：

    ```sh
    $ python setup.py build_sphinx -a
    ```

`source-dir`
:   目标源目录。这可以是相对于 `setup.py` 或 `setup.cfg` 文件的，也可以是绝对的。如果其中包含名为 `conf.py` 的文件，则默认为 `./doc` 或 `./docs`（首先检查 `./doc`）；否则，默认为 `source-dir`。

    这也可以通过向 `setup.py` 传递 `-s` 标志来设置：

    ```sh
    $ python setup.py build_sphinx -c $CONFIG_DIR
    ```

`builder`
:   要使用的生成器或生成器列表。默认值为“`html`”。

    也可以通过将 `-b` 标志传递给 `setup.py`：

    ```sh
    $ python setup.py build_sphinx -b $BUILDER
    ```

    ```{versionchanged} 1.6
    这现在可以是逗号分隔或空格分隔的构建器列表
    ```

`warning-is-error`
:   一个布尔值，确保Sphinx的警告会导致构建失败。默认是 `false`。

    这也可以通过向 `setup.py` 传递 `-W`标志来设置：

    ```sh
    $ python setup.py build_sphinx -W
    ```

`project`
:   记录的项目名称。默认值为 `''`。

`version`
:   短X.Y版本。默认值为 `''`。

`release`
:   完整版本，包括 `alpha/beta/rc` tags。默认值为 `''`。

`today`
:   如何设置当前日期的格式，用于替换“今天”。默认值为 `''`。

`link-index`
:   一个布尔值，确保 `index.html` 将被链接到根文档。默认是 `false`。

    这也可以通过向 `setup.py` 传递 `-i` 标志来设置。

    ```sh
    $ python setup.py build_sphinx -i
    ```

`copyright`
:   版权字符串。默认值为 `''`。

`nitpicky`
:   使用 `nit-picky` 模式。目前来说，就是对每个缺少的引用给出 warning。使用配置 [`nitpick_ignore`](https://www.sphinx-doc.org/en/master/usage/configuration.html#confval-nitpick_ignore)，可以把某些引用标为“known missing”。

`pdb`
:   用于在异常时配置“`pdb`”的布尔值。默认值为 `false`。