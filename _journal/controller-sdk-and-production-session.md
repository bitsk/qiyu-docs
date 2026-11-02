---
layout: default
order: 8
title: 把原厂手柄定位接入正式 VR 会话
description: 恢复 SPI、相位门控、Bionic SDK 和 DSP 依赖，连通双手位置输出。
---

手柄接入跨越无线、相机和用户态 SDK。工程沿用原厂亮点检测、LED 模型、PnP 与 EKF 核心，逐步恢复它在 Linux 上需要的传输、时间和运行环境。

## 先恢复原厂主机链路

原厂活动通路是 SPI 配合 evdev 通知。主线侧把 Nordic 做成独占字符设备，提供原始全双工交换与 IRQ/完成时间；用户态负责解析、双手状态和无线时钟。

相机的 SOF 相位又与手柄校时有关。对照原厂主机实现与板端资料后，补上 camera2 SOF 驱动 GPIO92/93 的门控，让主机图像、曝光相位和手柄输入进入同一工作节奏。

## 让 ARM32 SDK 使用实际图像

原厂 SDK 使用 ARM32/Bionic，运行时主体则是 ARM64/glibc，因此采用独立 helper 和 IPC。共享采集层提供两组 Y8 mosaic、真实头姿与惯性输入，helper 负责调用 SDK 并处理异步缓冲释放。

视觉执行还需要匹配的 SNPE/DSP 环境，包括设备信息、ARM stub 与 DSP skel 版本。补齐这些依赖后，双手基本位置输出接通，正式会话也加入了对应运行计划、转发与自动启动入口。

这一步把原始手柄数据推进为 VR provider 可消费的姿态；持续稳定性与物理精度继续在同一实现上完善。

继续阅读：[VR 运行时与功能仓库]({{ '/wiki/vr-runtime/' | relative_url }})。资料：[R04]({{ '/wiki/evidence-sources/#r04' | relative_url }})、[R05]({{ '/wiki/evidence-sources/#r05' | relative_url }})、[R06]({{ '/wiki/evidence-sources/#r06' | relative_url }})。
