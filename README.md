# Qiyu 工程文档

奇遇 Linux 工程的跨仓库架构、维护指南、Wiki 与开发 Blog。

- 文档站：https://bitsk.github.io/qiyu-docs/
- Wiki：[wiki/](wiki/)
- 架构：[wiki/architecture.md](wiki/architecture.md)
- 开发记录：[_posts/](_posts/)
- 内核代码：[bitsk/qiyu-kernel](https://github.com/bitsk/qiyu-kernel)

本仓库承载工程文档与页面源码。功能代码、接口定义与本组件构建入口由各代码仓库维护。Jekyll 原生 Markdown 页面由 GitHub Pages 构建，`_layouts/` 与 `assets/` 为站点呈现；不需要额外文档托管服务。

原误放在 kernel 主分支的两次跨仓库规划提交保留为 [迁移补丁](history/r1-project-planning.patch.gz)，正文已迁入 Wiki。
