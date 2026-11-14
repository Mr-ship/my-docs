# 使用指南

## 修改并发布一份文档

1. 修改 `docs/` 目录中的 Markdown 文件。
2. 在本地预览并检查页面、表格、代码块和页面间链接。
3. 用严格模式构建一次，任何警告都要先修掉再提交。
4. 提交修改并推送到 `main` 分支。
5. 在仓库的 `Actions` 页面查看 `Deploy MkDocs to GitHub Pages` 的运行状态，确认 `build` 与 `deploy` 都成功。
6. 打开网站检查内容是否更新；若没变化，先强制刷新或开无痕窗口排除缓存。

## 常用命令

```bash
# 创建并激活独立虚拟环境（Windows Git Bash）
python -m venv .venv
source .venv/Scripts/activate

# 安装依赖
python -m pip install -r requirements.txt

# 本地预览，通常为 http://127.0.0.1:8000/
python -m mkdocs serve

# 严格构建：生成 site/，出现警告即失败
python -m mkdocs build --strict
```

```powershell
# Windows PowerShell 的激活命令（与上一种终端二选一，不要重复执行）
.\.venv\Scripts\Activate.ps1
```

> macOS / Linux 则使用 `source .venv/bin/activate`。如果 PowerShell 阻止脚本运行，直接换用 Git Bash 的 `source` 命令即可，不必修改系统执行策略。

`--strict` 值得保留：它会在本地就把失效的相对链接、写错的 `nav` 路径暴露出来，而不是等到 CI 里 `build` 失败再回头查日志。

## 哪些改动会触发部署

工作流在 `push` 到 `main` 时，只有改动命中 `docs/**`、`mkdocs.yml`、`requirements.txt` 或 `.github/workflows/deploy.yml` 才会运行。

| 改动内容 | 是否自动部署 | 原因 |
| --- | --- | --- |
| 修改 `docs/index.md` | 是 | 命中 `docs/**`，网站内容确实变了 |
| 修改 `mkdocs.yml` 导航 | 是 | 命中配置文件，需要重新构建 |
| 只改 `report/README.md` 或根 `README.md` | 否 | 不在 `paths` 列表内，网站内容没变 |
| 在 Actions 页面点 `Run workflow` | 是 | `workflow_dispatch` 允许手动运行 |

最后一种手动运行适合首次部署失败后的重试，但「修改文档后自动更新」的验证必须用真正的 `git push`，不能用手动运行代替。

## 我的排错顺序

先看 `build`：严格构建失败，基本都是 Markdown 里写了不存在的相对链接、YAML 用了 Tab 缩进，或者把 `nav` 路径误写成 `docs/index.md`（正确写法是相对 `docs/` 的 `index.md`）。再看 `deploy`：这里失败通常是 Pages 发布来源没设成 **GitHub Actions**，或者工作流缺少 `pages: write` / `id-token: write` 权限。在 Actions 里展开红色步骤，日志会直接指出是哪一步、哪一行为什么失败。

相关页面：[返回首页](index.md) · [关于本项目](about.md) · [API 参考](api.md)
