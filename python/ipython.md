# IPython

IPython 提供了以下特性：

- 交互式 shell（终端和基于 [Qt](https://en.wikipedia.org/wiki/Qt_(framework)) 的）。
- 一个基于浏览器的[笔记本界面](https://en.wikipedia.org/wiki/Notebook_interface)，支持代码、文本、数学表达式、内联图和其他媒体。
- 支持交互式数据可视化和 GUI 工具包的使用。
- 灵活的，可嵌入的解释器加载到自己的项目。
- [并行计算](https://en.wikipedia.org/wiki/Parallel_computing)工具。
- IPython 允许与 [Tkinter](https://en.wikipedia.org/wiki/Tkinter)、[PyGTK](https://en.wikipedia.org/wiki/PyGTK)、[PyQt](https://en.wikipedia.org/wiki/PyQt "PyGTK")/[PySide](https://en.wikipedia.org/wiki/PySide "SciPy") 和 [wxPython](https://en.wikipedia.org/wiki/WxPython) 进行非阻塞交互（标准的 Python shell 只允许与 Tkinter 交互）。IPython 可以使用异步状态回调和/或 [MPI](https://en.wikipedia.org/wiki/Message_Passing_Interface) 交互式地管理并行[计算集群](https://en.wikipedia.org/wiki/Computer_cluster)。IPython 也可以用作系统外壳的替代品。它的默认行为与 [Unix shell](https://en.wikipedia.org/wiki/Unix_shell) 非常相似，但是它允许自定义，并且可以在实时的 Python 环境中灵活地执行代码。使用 IPython 作为 shell 的替代并不常见，现在建议使用 Xonsh，它提供了 IPython 的大部分特性，并提供了更好的 shell 集成。

## IPython 并行计算

IPython 基于提供并行和分布式计算的体系结构。IPython 允许交互式地开发、执行、调试和监控并行应用程序，因此 IPython 中的 I（Interactive）这个架构抽象了并行性，使 IPython 支持许多不同风格的并行性，包括：

- [单程序、多数据](https://en.wikipedia.org/wiki/SPMD)（SPMD）并行
- [多程序、多数据](https://en.wikipedia.org/wiki/MPMD)（MPMD）并行
- 使用 [MPI](https://en.wikipedia.org/wiki/Message_Passing_Interface) 传递消息
- [任务并行](https://en.wikipedia.org/wiki/Task_parallelism)
- [数据并行](https://en.wikipedia.org/wiki/Data_parallelism)
- 以上方法的联合处理
- 自定义用户定义的方法

随着 IPython 4.0 的发布，并行计算功能成为可选的，并在 [ipyparallel](https://pypi.org/project/ipyparallel/) Python 包下发布。ipyparallel 的大部分功能现在都被更成熟的库（如 [Dask](https://en.wikipedia.org/wiki/Dask_(software))）覆盖了。

IPython 经常从 SciPy 堆栈的库中提取，如 [NumPy](https://en.wikipedia.org/wiki/NumPy) 和 [SciPy](https://en.wikipedia.org/wiki/SciPy)，这些库通常安装在许多科学 Python 发行版中 IPython 提供了与 SciPy 堆栈的一些库的集成，特别是 [matplotlib](https://en.wikipedia.org/wiki/Matplotlib)，当与 Jupyter 笔记本一起使用时产生内联图。Python 库可以实现特定于 IPython 的钩子来定制富对象显示。例如，当在 IPython 上下文中使用时，[SymPy](https://en.wikipedia.org/wiki/SymPy "SymPy") 实现了数学表达式的呈现，就像呈现 [LaTeX](https://en.wikipedia.org/wiki/LaTeX) 一样，而 [Pandas](https://en.wikipedia.org/wiki/Pandas_(software) "Pandas (software)") 数据框架使用 HTML 表示。