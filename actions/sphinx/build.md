# 通用 Sphinx Action

创建一个通用的 Sphinx Action：

```yaml
name: deploy
on:
  push:
    branches:
      - main

jobs:
  # 这个工作流程包含一个名为 "build" 的 job
  build:
    # job 将运行的运行器的类型
    runs-on: ubuntu-latest

    # steps 将作为工作的一部分而执行的任务序列
    steps:
      # 这个动作在 $GITHUB_WORKSPACE 下签出你的版本库，以便工作流就可以访问它
      - uses: actions/checkout@v2
      # 设定 Python 环境
      - name: Set up Python
        uses: actions/setup-python@v2
        with:
          python-version: 3.9
      # 设定 conda 环境
      - uses: s-weigand/setup-conda@v1
      - run: conda --version
      - run: which python

      # 安装依赖包
      - name: Install dependencies
        run: |
          pip install -r requirements.txt
          conda install ipykernel
          python -m ipykernel install --user --name ai --display-name "ai"
      
      # 安装 HTML 主题
      - name: Install theme
        run: |
          git clone https://github.com/xinetzone/xin-css.git ./_static/xin-css
          git clone https://github.com/xinetzone/w3css.git ./_static/w3css

      # 构建 Sphinx 文档
      - name: Build the book
        run: |
          make html

      # 部署 HTML 到 gh-pages 分支
      - name: GitHub Pages action
        uses: peaceiris/actions-gh-pages@v3.6.1
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./_build/html
          user_name: "github-actions[bot]"
          user_email: "github-actions[bot]@users.noreply.github.com"
```

详细见 [sphinx-action 文档](https://xinetzone.github.io/sphinx-action/)。
