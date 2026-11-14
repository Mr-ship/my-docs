# 我的技术文档

欢迎访问我的个人文档网站。本网站使用 MkDocs Material 构建，由 GitHub Actions 在每次推送后自动部署到 GitHub Pages。

- 作者：Mr-ship
- 实践主题：静态文档自动部署
- 源码仓库：[Mr-ship/my-docs](https://github.com/Mr-ship/my-docs)
- [关于本项目](about.md)
- [使用指南](guide.md)
- [API 参考](api.md)

## 这个站点在做什么

我平时只维护 `docs/` 目录下的四个 Markdown 文件和根目录的 `mkdocs.yml`。配置文件决定页面如何组织成导航；执行构建命令后，MkDocs 把 Markdown 连同主题样式一起生成到 `site/`。这个目录是构建产物，我不手工编辑，也不提交到 `main`，因为它随时可以从源码重新生成。

真正把静态文件变成公开网站的是 `.github/workflows/deploy.yml`：工作流把 `site/` 打包成构件上传，再由官方 Pages 部署任务发布。整个链路里我唯一需要做的事，就是改文档并 `git push`。

## 我的实践体会

在之前的 Pi Agent 实践课里，我做的是「写一份报告、附上截图」这种一次性交付。这次的差别在于，文档本身变成了可运行的源码：改一行 Markdown、推送一次，几分钟后公开页面就会跟着变。

这让我第一次比较清楚地分清了四层东西——源码（`docs/` 与配置文件）、构建产物（`site/`）、自动化流程（Actions 工作流）和托管服务（GitHub Pages）。它们出错时的表现完全不同：文档写错或链接失效会卡在 `build --strict`；Pages 来源或权限配错会卡在 `deploy`；而页面看起来没更新，往往只是浏览器缓存。把问题定位到具体某一层，比反复重跑整个流程有用得多。

完整的操作记录、关键步骤截图和原理思考写在仓库的 [个人实验报告](https://github.com/Mr-ship/my-docs/blob/main/report/README.md) 里。
