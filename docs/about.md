# 关于本项目

## 项目目标

把一组 Markdown 文档变成任何人都能访问的公开网站，并让「修改 → 推送 → 自动构建 → 自动部署 → 网页更新」这条链路完整跑通，不需要手工上传文件，也不需要自己申请访问令牌或维护发布分支。

## 技术组成

| 工具 | 作用 | 在本仓库中的对应物 |
| --- | --- | --- |
| Markdown | 编写文档内容 | `docs/` 下的 index / about / guide / api 四页 |
| MkDocs Material | 将文档构建成静态网站 | `mkdocs.yml` 的 `theme` 与 `nav`；生成 `site/` |
| GitHub Actions | 自动执行安装、构建和部署 | `.github/workflows/deploy.yml` |
| GitHub Pages | 托管并提供网站访问地址 | https://mr-ship.github.io/my-docs/ |

## 四层关系：我自己的理解

| 层次 | 是否手写 | 是否提交到 main | 变化由谁引起 |
| --- | --- | --- | --- |
| 源码 `docs/` + `mkdocs.yml` | 是 | 是 | 我修改 Markdown 或导航 |
| 构建产物 `site/` | 否 | 否，已写进 `.gitignore` | 每次 `mkdocs build` 重新生成 |
| 工作流 `deploy.yml` | 是 | 是 | 只在调整部署流程时修改 |
| 公开网站 | 否 | 否 | `deploy` 任务发布构建产物 |

最容易混淆的是「构建产物」和「网站」：`site/` 只是本机或 CI 里的一堆 HTML 文件，上传到 Actions 构件后就已经和源码仓库解耦了；最终用户访问的页面来自 Pages 的托管环境，而不是 `main` 分支上的文件。所以本地删掉 `site/` 不会让网站下线，重新构建一次就能得到完全相同的结果。

## 为什么选择官方 Pages Actions 方案

工作流显式授予 `pages: write` 与 `id-token: write`，权限只给 `deploy` 任务，构建任务仍是只读的 `contents: read`。这样既不需要把仓库默认权限改成全局读写，也不需要创建 `gh-pages` 分支或开启「允许 Actions 创建和批准 PR」。发布来源设为 **GitHub Actions** 后，部署记录会集中出现在 `github-pages` 环境里，排错时比较容易对照。

继续阅读：[使用指南](guide.md) · [API 参考](api.md) · [返回首页](index.md)
