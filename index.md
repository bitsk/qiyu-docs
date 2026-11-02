---
layout: default
title: 工程文档
---

<p class="eyebrow">QIYU · LINUX · OPEN ENGINEERING</p>
# 从硬件到可维护的系统

<p class="lead">奇遇 Linux 工程的架构约定、构建路径与开发记录。</p>

<div class="cards"><section class="card"><h2>工程 Wiki</h2><p>查阅仓库职责、系统配方与维护方法。</p><a href="{{ '/wiki/' | relative_url }}">打开 Wiki →</a></section><section class="card"><h2>开发 Blog</h2><p>按阶段追踪迁移、验证与版本演进。</p><a href="{{ '/blog/' | relative_url }}">阅读开发记录 →</a></section></div>

## 当前工作

内核以 `qiyu-kernel` 为主仓库，保留 Linux 主线历史和按子系统维护的下游提交。运行时、调试和 rootfs 的七个独立功能仓库已上传首批源码；[仓库架构](wiki/architecture.html) 说明各自职责，[发布记录](wiki/source-publication.html) 列出固定提交和验证范围。

工程文档集中在这里；具体硬件的功能验收、物理效果与长期稳定性各自记录，不以目录或规划代替验收。
