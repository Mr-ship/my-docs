# my-docs｜MkDocs 文档站与 GitHub Pages 自动部署

这是实践项目 2 的个人作业仓库：用 Markdown 与 MkDocs Material 编写一个四页文档网站，再由 GitHub Actions 在每次推送后自动构建并部署到 GitHub Pages。

- **公开网站**：https://mr-ship.github.io/my-docs/
- **个人实验报告**：[个人实验报告](report/README.md)
- **源码仓库**：https://github.com/Mr-ship/my-docs
- **作者**：Mr-ship

## 仓库结构

```text
my-docs/
├── .github/workflows/deploy.yml   # 官方 Pages Actions 工作流
├── docs/                         # 四页网站源码（Markdown）
│   ├── index.md
│   ├── about.md
│   ├── guide.md
│   └── api.md
├── report/                       # 实验过程报告与关键截图
│   ├── README.md
│   └── images/
├── mkdocs.yml                    # 站点名称、主题与导航配置
├── requirements.txt              # MkDocs 与 Material 依赖
└── .gitignore                    # 忽略 site/、虚拟环境等
```

`docs/` 是需要人工维护的源码，`site/` 是构建产物（已忽略，不提交）。工作流的 `build` 任务生成并上传 `site/`，`deploy` 任务把它发布到 Pages，因此不需要创建 `gh-pages` 分支，也不需要任何访问令牌。

## 本地预览

```bash
python -m venv .venv
source .venv/Scripts/activate        # Windows Git Bash；PowerShell 用 .\.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
python -m mkdocs build --strict
python -m mkdocs serve               # 浏览器打开 http://127.0.0.1:8000/
```

推送到 `main` 且改动涉及 `docs/`、`mkdocs.yml`、`requirements.txt` 或工作流文件时会触发自动部署；仅修改本 README 或 `report/` 不会触发，这是预期行为。

## 文档页面

[首页](docs/index.md) · [关于本项目](docs/about.md) · [使用指南](docs/guide.md) · [API 参考](docs/api.md)
