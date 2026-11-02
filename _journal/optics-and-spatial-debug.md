---
layout: default
order: 7
title: 复用原厂光学模型，建立统一调试台
description: 把光学校正与设备观测整理成可独立演进的运行时和调试功能。
---

内屏和运行时接通后，开发继续处理透镜模型与日常调试入口。前者决定图像如何映射到眼睛，后者让系统状态、跟踪输入和实际画面能够一起观察。

## 把原厂光学经验接入 Monado

光学校正复用原厂 QVR 参数，在运行时侧补上屏幕缩放、径向畸变与 RGB 修正。候选采用独立前缀和配置保存，便于在已有输出路径上比较与调整。这个阶段整理了模型、参数和接口，实际佩戴仍是后续调整依据。

## 六个相机角色，一个调试入口

调试页面把设备、头部、手柄、相机和视频收进统一导航。四路追踪相机的预览来自已有生产者 tap，独立透视输入保留自己的状态。网页明确显示 live、stale 或 waiting，便于区分输入来源。

Spatial UI 随后部署到设备，并检查了移动端布局。后端以接口和 WebSocket 提供数据，页面订阅已经运行的生产者；打开调试台无需另起一套追踪相机采集。

光学与调试分别维护，但共享同一个工程目标：让已有功能更容易理解、调整和继续开发。

继续阅读：[光学与运行时]({{ '/wiki/vr-runtime/' | relative_url }})、[调试系统]({{ '/wiki/debug-and-video/' | relative_url }})。资料：[S06]({{ '/wiki/evidence-sources/#s06' | relative_url }})、[D01]({{ '/wiki/evidence-sources/#d01' | relative_url }})、[D02]({{ '/wiki/evidence-sources/#d02' | relative_url }})。
