# Sphinx Build

````{panels}
:container: w3-card-4 w3-padding w3-pale-blue
:column: col-lg-12 px-2 py-2
---
[Sphinx Build](https://github.com/marketplace/actions/sphinx-build)
:   这是一个 Github action，用于在项目中查找 Sphinx 文档文件夹。它使用 Sphinx 构建文档，构建过程中的任何错误都在 Github 状态检查时冒泡出来。
^^^
```{rubric} 主要目的：
```

- 运行 CI 测试以确保文档正常构建。
- 允许贡献者在 Github 上通过简单的文档更改获取构建错误，而不必安装 Sphinx 并在本地构建。

```{rubric} 使用：
```

为 action 创建一个工作流，例如：

```yaml
name: "Pull Request Docs Check"
on: 
- pull_request

jobs:
  docs:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v1
    - uses: ammaraskar/sphinx-action@master
      with:
        docs-folder: "docs/"
```

- 如果你的项目需要任何 Python 依赖项（主题、构建工具等），那么将它们放在 `docs` 文件夹中的 `requirements.txt` 文件中。
- 如果您有多个 Sphinx 文档文件夹，请使用多个 `uses` 块。

有关使用此操作的完整示例（包括高级用法），请查看 [ammaraskar/sphinx-action-test: Testing repo for my sphinx github action](https://github.com/ammaraskar/sphinx-action-test)。

```{rubric} 大型操作：
```
一些非常好的操作可以很好地使用这个工具，比如 [`actions/upload-artifact`](https://github.com/actions/upload-artifact) 和 [`ad-m/github-push-action`](https://github.com/ad-m/github-push-action)。

你可以使用这些使构建的 HTML 和 PDF 作为工件：

```yaml
    - uses: actions/upload-artifact@v1
      with:
        name: DocumentationHTML
        path: docs/_build/html/
```

或者将 `docs` 更改自动推送到 `gh-pages` 分支，工作流程的代码：

```yaml
    - name: Commit documentation changes
      run: |
        git clone https://github.com/ammaraskar/sphinx-action-test.git --branch gh-pages --single-branch gh-pages
        cp -r docs/_build/html/* gh-pages/
        cd gh-pages
        git config --local user.email "action@github.com"
        git config --local user.name "GitHub Action"
        git add .
        git commit -m "Update documentation" -a || true
        # The above command will fail if no changes were present, so we ignore
        # the return code.
    - name: Push changes
      uses: ad-m/github-push-action@master
      with:
        branch: gh-pages
        directory: gh-pages
        github_token: ${{ secrets.GITHUB_TOKEN }}
```

想要了解一个完整的例子，请看：[ammaraskar/sphinx-action-test](https://github.com/ammaraskar/sphinx-action-test)。

```{rubric} 高级用法：
```

如果你想定制用来构建 `docs` 的命令（默认 `make html`），你可以在 `with` 块中提供一个 `build-command`。例如，要直接调用 `sphinx-build`，可以使用：

```yaml
    - uses: ammaraskar/sphinx-action@master
      with:
        docs-folder: "docs/"
        build-command: "sphinx-build -b html . _build"
```

如果你的构建需要安装系统级依赖项，你可以使用 `pre-build-command` 参数，如下所示：

```yaml
    - uses: ammaraskar/sphinx-action@master
      with:
        docs-folder: "docs2/"
        pre-build-command: "apt-get update -y && apt-get install -y latexmk texlive-latex-recommended texlive-latex-extra texlive-fonts-recommended"
        build-command: "make latexpdf"
```

```{rubric} 运行测试：
```

```sh
python -m unittest
```

```{rubric} 格式化：
```

请使用 [black](https://github.com/psf/black) 格式：

```sh
black entrypoint.py sphinx_action tests
```
````
