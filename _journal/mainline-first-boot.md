---
layout: default
order: 1
title: 让主线 Linux 在 MIX 上启动并点亮内屏
description: 从原厂硬件资料出发，接通 USB、UFS、CPU、GPU 与双 DSI 显示。
---

MIX 的 Linux 适配从一件很具体的事开始：让自己的内核在这块板子上执行，并留下可读的启动信息。原厂运行时 FDT、DTBO、固件和日志提供板级依据，Linux 主线提供 SM8250 的基础驱动。工作是把两者接起来，补齐这台设备的电源、时钟、中断和连接关系。

## 先建立观察系统的入口

最早没有可用的 USB 控制台，因此先确认 PID 1，再通过 RAM 日志接力读取内核 ring buffer 和运行时设备树。日志中的 USB、UFS、时钟与调压器探测等待看似很多，实际上共同指向缺失的 PSCI CPU 电源域 provider。

补齐 CPU idle 和 PSCI domain 配置后，资源依赖向前推进，USB gadget 和 UFS 得以注册。再增加 OSM L3 interconnect，CPU 调频也接上主线接口。USB 双向控制台、8 个在线 CPU、三个频率域和温度区，成为此后开发的日常基础。

## 图形从命令走到画面

Adreno 650 先通过 DRM/MSM 完成命令提交、fence 等待与结果回读。显示则需要面板供电、双 DSI、DSC 与 PHY 一起工作。排查中用 DPU 硬件纯色绕过 framebuffer 路径，并逐项对照原厂活动寄存器，最终找到 PLL bias 引用计数与 SYSTEM_MUXES 初始化的问题。

这一天的成果是从启动、观察、设备注册推进到 GPU 和内屏。后续 Debian 和 SteamVR 都建立在这条硬件路径上。

继续阅读：[硬件适配]({{ '/wiki/hardware/' | relative_url }})、[显示与 GPU]({{ '/wiki/display-and-gpu/' | relative_url }})。资料：[H01]({{ '/wiki/evidence-sources/#h01' | relative_url }})、[H02]({{ '/wiki/evidence-sources/#h02' | relative_url }})、[H03]({{ '/wiki/evidence-sources/#h03' | relative_url }})、[H04]({{ '/wiki/evidence-sources/#h04' | relative_url }})、[H05]({{ '/wiki/evidence-sources/#h05' | relative_url }})。
