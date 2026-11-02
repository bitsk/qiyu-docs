---
layout: default
title: 开发历程
---

# 开发历程

这条路线从奇遇 MIX A081 的原厂硬件资料开始，先让主线 Linux 驱动设备，再让 Debian、SteamOS 和 VR 运行时接力工作。最初解决的是启动与调试入口，随后逐步接通显示、图形、传感器和相机，最后把这些能力组织成可启动、可退出、可维护的 VR 系统。

## 关键阶段

| 时间 | 主要进展 | 工程重点 |
| --- | --- | --- |
| r1 | 主线启动、USB/UFS、CPU/GPU 与内屏接入 | 原厂 FDT 转换、主线依赖补齐、双 DSI/DSC 与 PHY |
| r1 | osstore 与持久 Debian | 保留 Android 数据，systemd、无线、GPU 输出和电源管理 |
| r1～27 | 相机、IMU、CDSP 与原生 XR 桌面 | 共享缓冲、时间戳、FastRPC、真实设备上的 OpenXR |
| r1 | SteamOS 桌面与原生 ARM64 SteamVR | 独立 rootfs、私有图形栈、分离的 GPU/KMS 路径 |
| r1 | Dashboard、QVR 头部跟踪与 Venus | 实际相机/IMU 数据进入 SteamVR，硬件 H.264 接入 |
| r1～31 | 原厂光学校正、手柄定位与调试台 | 原厂模型复用、Bionic helper、同步 GPIO、Rust/WebRTC |
| r1 | 两档温控与正式会话整合 | 内核接口、系统服务和运行时生命周期协同 |
| r1 | 渲染与呈现调度优化 | Turnip 查询等待、显示缓冲、运行时偏移持久化 |
| r1 | 按功能发布代码仓库 | 保留真实主线历史，固定组件提交，独立维护文档 |

## 按主题阅读

- [硬件适配总览]({{ '/wiki/hardware/' | relative_url }})：板级依据、基础设备、电源、无线与音频。
- [显示与 GPU]({{ '/wiki/display-and-gpu/' | relative_url }})：双 DSI 内屏、PLL、DRM/MSM、Mesa 与 Turnip。
- [相机、IMU 与共享采集]({{ '/wiki/cameras-and-sensors/' | relative_url }})：四路追踪相机、透视相机、QCFT 与 SEE。
- [Debian 到 SteamOS]({{ '/wiki/steamos/' | relative_url }})：持久系统、图形栈、Steam 客户端和多系统组合。
- [VR 运行时]({{ '/wiki/vr-runtime/' | relative_url }})：Monado、QVR/CDSP、手柄 SDK 与会话编排。
- [调试系统与视频]({{ '/wiki/debug-and-video/' | relative_url }})：网页、Rust 后端、H.264、WebRTC 和相机预览。

[Blog]({{ '/blog/' | relative_url }}) 按开发阶段讲述每个阶段的实现过程；Wiki 按主题说明今天如何理解这些代码。首批历史整理于 r1，原始资料的路径和摘要统一收在[来源附录]({{ '/wiki/evidence-sources/' | relative_url }})。
