---
layout: default
title: 整理温控档位与分相位视频
description: 把性能策略、Rust 调试后端和短短长相机预览纳入系统服务。
---

系统能够持续运行之后，温控和调试视频需要成为可管理的系统功能。两者都建立在已有设备接口上：温控使用 thermal/cpufreq/devfreq，视频订阅显示或相机生产者。

## 两档清晰的运行策略

原厂档保留 schedutil 和 simple_ondemand，按已采集的设备参数管理温度与频率。性能档使用 performance governor，并调整大核首级阈值。所选档位保存到配置，systemd 服务负责重新应用，独立 critical 保护保留。

内核负责可观测、可控制的接口，rootfs 负责策略文件和服务。这种分工使 Debian 与 SteamOS 能复用同一套板级能力。

## 让相机的三个相位看得见

调试视频继续完善 Rust 服务，把短、短、长三相输入分别送到网页。手柄亮点和头部跟踪场景由此可以分开观察；源帧、编码结果与浏览器显示也分别记账。

相机、屏幕和控制器状态仍由各自生产者提供，调试台负责汇总和控制入口。这些工作后来一起整理进 qiyu-debug 与 qiyu-rootfs，成为系统可组合的功能模块。

继续阅读：[电源与温控]({{ '/wiki/hardware/' | relative_url }})、[调试视频]({{ '/wiki/debug-and-video/' | relative_url }})。资料：[T01]({{ '/wiki/evidence-sources/#t01' | relative_url }})、[D03]({{ '/wiki/evidence-sources/#d03' | relative_url }})。
