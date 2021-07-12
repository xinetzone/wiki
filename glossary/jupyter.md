# Jupyter

```{glossary}
[Jupyter](https://jupyter.org/index.html)
    Jupyter（(/ˈdʒuːpɪtər/）是一个项目和社区，其目标是“开发跨数十种编程语言的[开源软件](https://en.wikipedia.org/wiki/Open-source_software)、[开放标准](https://en.wikipedia.org/wiki/Open_standard)和[交互式计算](https://en.wikipedia.org/wiki/Interactive_computing)服务”。它是 2014 年由 [Fernando Pérez](https://en.wikipedia.org/wiki/Fernando_P%C3%A9rez_(software_developer)) 从 {term}`IPython` 剥离出来的。Project Jupyter 的名字引用了Jupyter 支持的三种核心编程语言 [Julia](https://en.wikipedia.org/wiki/R_(programming_language))、{term}`Python` 和 [R](https://en.wikipedia.org/wiki/R_(programming_language))，同时也是向伽利略（[Galileo](https://en.wikipedia.org/wiki/Galileo_Galilei)）记录木星卫星发现的笔记本致敬。

[IPython](https://ipython.org/index.html)
    IPython（Interactive Python）是一个用于多种编程语言的交互式计算的 [shell 命令](https://en.wikipedia.org/wiki/Shell_(computing))，最初是为 Python 编程语言开发的，它提供[内省](https://en.wikipedia.org/wiki/Introspection_(computer_science))、[富媒体](https://en.wikipedia.org/wiki/Rich_media)、shell 语法、[制表符补全](https://en.wikipedia.org/wiki/Tab_completion)和历史记录。

[JupyterHub](https://jupyterhub.readthedocs.io/en/stable/)
    JupyterHub 是 Jupyter 社区的核心开源工具，它允许您部署向多个用户提供远程数据科学环境的应用程序。它可以部署在云中，也可以部署在您自己的硬件上。
    JupyterHub 是一个用于 Jupyter 笔记本的多用户服务器。它通过生成、管理和代理许多单一的 Jupyter 笔记本服务器来支持许多用户。虽然 JupyterHub 需要管理服务器，但是像 Jupyo 这样的第三方服务通过托管和管理云中的多用户 Jupyter 笔记本提供了一种替代方法。

[Jupyter Notebook](https://jupyter-notebook.readthedocs.io/en/stable/index.html)
    Jupyter Notebook（原名 IPython Notebook）是一个[基于 Web 的交互式](https://en.wikipedia.org/wiki/Web_application)计算环境，用于创建 Jupyter 笔记本文档。“Notebook”这个术语可以指代许多不同的实体，主要是 Jupyter [Web 应用程序](https://en.wikipedia.org/wiki/Web_application)、Jupyter Python [Web 服务器](https://en.wikipedia.org/wiki/Web_server)或 Jupyter [文档格式](https://en.wikipedia.org/wiki/Document_file_format)，具体取决于上下文。Jupyter Notebook 文档是一个 [JSON](https://en.wikipedia.org/wiki/JSON) 文档，遵循版本模式，包含输入/输出单元格的有序列表，可以包含代码、文本（使用 [Markdown](https://en.wikipedia.org/wiki/Markdown "Markdown")）、数学、[绘图](https://en.wikipedia.org/wiki/Plot_(graphics) "Plot (graphics)")和富媒体，通常以 ".ipynb" 作为扩展名。

    一个 Jupyter Notebook 可以被转换成许多开放的标准输出格式（[HTML](https://en.wikipedia.org/wiki/HTML "Open standard"), [presentation slides](https://en.wikipedia.org/wiki/Presentation_slide), [LaTeX](https://en.wikipedia.org/wiki/LaTeX "Presentation slide"), [PDF](https://en.wikipedia.org/wiki/PDF "PDF"), [ReStructuredText](https://en.wikipedia.org/wiki/ReStructuredText "ReStructuredText"), [Markdown](https://en.wikipedia.org/wiki/Markdown), Python）通过 Web 界面的 "Download As" ，通过 `nbconvert` 库或“Jupyter nbconvert”命令行界面在 shell 中。为了简化 web上的笔记本文档可视化，`nbconvert`库是通过 NbViewer 提供的一个服务，它可以获取任何公开可用的笔记本文档的 URL，将其转换为 HTML 并显示给用户。

    Jupyter Notebook提供了一个基于浏览器的REPL，构建于许多流行的开源库之上：
    *  [IPython](https://en.wikipedia.org/wiki/IPython "IPython")
    * [ØMQ](https://en.wikipedia.org/wiki/ZeroMQ "Open-source software") (ZeroMQ)
    * [Tornado (web server)](https://en.wikipedia.org/wiki/Tornado_(web_server) "Tornado (web server)")
    * [jQuery](https://en.wikipedia.org/wiki/JQuery "JQuery")
    * [Bootstrap (front-end framework)](https://en.wikipedia.org/wiki/Bootstrap_(front-end_framework) "Bootstrap (front-end framework)")
    * [MathJax](https://en.wikipedia.org/wiki/MathJax)

[Jupyter 内核](https://jupyter.readthedocs.io/en/latest/projects/kernels.html)
    Jupyter 内核是一个负责处理各种类型的请求（[代码执行](https://en.wikipedia.org/wiki/Execution_(computing))、[代码完成](https://en.wikipedia.org/wiki/Autocomplete)、检查）并提供应答的程序。内核使用 ZeroMQ 与 Jupyter 的其他组件进行通信，因此可以位于相同或[远程机器](https://en.wikipedia.org/wiki/Remote_computer)上。与许多其他类似于 notebook 的接口不同，在 Jupyter 中，内核不知道它们被附加到一个特定的文档，并且可以一次连接到多个客户机。通常，内核只允许执行一种语言，但也有一些例外。

[JupyterLab](https://jupyterlab.readthedocs.io/en/stable/index.html)
    JupyterLab 是Project Jupyter 的一个较新的用户界面。它在一个灵活的用户界面中提供了经典的 Jupyter Notebook 的构建模块（笔记本、终端、文本编辑器、文件浏览器、富输出等）。第一个稳定版本于 2018 年 2 月 20 日发布。

[Jupyter Book](https://pypi.org/project/jupyter-book/)
    Jupyter Book 是一个开源项目，用于从计算材料中构建图书和文档。它允许用户在 Markdown 中构造内容，Markdown 的一个扩展版本名为 MyST，数学和公式 使用 MathJax, Jupyter notebook, reStructuredText，在构建时运行的 Jupyter notebook 的输出。可以产生多种输出格式（目前是单文件、多页 HTML 网页和 PDF 文件）。

[Jupyter Notebook Viewer](https://nbviewer.jupyter.org/)
    分享 Jupyter 笔记本的简单方法。

[nbgrader](https://nbgrader.readthedocs.io/en/stable/)
    nbgrader 是一个在 Jupyter 笔记本上创建和批改作业的工具。它允许教师创建包含 Python 编码练习或任何其他支持的内核和文本响应的作业。提交的作业可以自动评分、手动评分或两者混合。

[并行计算](https://en.wikipedia.org/wiki/Parallel_computing)
    ...
```

多媒体是数字化的方式组织文字、相片、视觉艺术、声音、动画、影像。当多媒体的使用者或是观看者，可以自主性操作多媒体内的元件或元素可以在任何时间传递或上传，叫做**互动式多媒体**。使用者在互相连结的架构中可以自由地搜寻、互动，被称为**超媒体**。
