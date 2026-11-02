---
layout: default
title: 工程文档
---

<p class="eyebrow">QIYU · LINUX · OPEN ENGINEERING</p>
# 从硬件到可维护的系统

<p class="lead">奇遇 Linux 工程的硬件适配、SteamOS 集成、VR 运行时和开发记录。</p>

<div class="cards"><section class="card"><h2>硬件适配</h2><p>主线启动、显示与 GPU、相机和传感器。</p><a href="{{ '/wiki/hardware/' | relative_url }}">阅读硬件 Wiki →</a></section><section class="card"><h2>SteamOS 与多系统</h2><p>持久 Debian、SteamOS 桌面、组件与系统配方。</p><a href="{{ '/wiki/steamos/' | relative_url }}">查看系统集成 →</a></section><section class="card"><h2>VR 与调试</h2><p>Monado、QVR/CDSP、手柄 SDK 和 H.264/WebRTC。</p><a href="{{ '/wiki/vr-runtime/' | relative_url }}">阅读运行时开发 →</a></section><section class="card"><h2>开发 Blog</h2><p>从首次主线启动到正式会话与渲染优化。</p><a href="{{ '/blog/' | relative_url }}">按时间阅读 →</a></section></div>

## 一条连续的开发路线

从 A081 原厂硬件资料出发，接通主线 Linux 的基础设备与内屏，再把 Debian、SteamOS 和 VR 运行时逐步组合起来。[开发历程]({{ '/wiki/development-history/' | relative_url }}) 串起每个阶段，[工程 Wiki]({{ '/wiki/' | relative_url }}) 按模块说明实现与维护方式。

## 代码与工程文档

内核以 [qiyu-kernel](https://github.com/bitsk/qiyu-kernel) 为主，保留 Linux 主线历史和按子系统维护的下游提交。设备服务、跟踪、VR、会话、调试与 rootfs 分别在功能仓库中维护；[仓库架构]({{ '/wiki/architecture/' | relative_url }}) 和 [源码发布记录]({{ '/wiki/source-publication/' | relative_url }}) 提供入口。

工程文档与 Wiki/Blog 页面集中在这里。历史资料的路径与摘要见 [来源附录]({{ '/wiki/evidence-sources/' | relative_url }})。
