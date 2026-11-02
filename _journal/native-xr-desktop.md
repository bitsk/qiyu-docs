---
layout: default
title: 把 XR 桌面从 QEMU 搬到真实头显
description: 桌面内容、OpenXR 图层、实际 IIO 姿态与 Vulkan 输出在 A081 上连接起来。
---

原生 XR 桌面先在 QEMU 中验证交互：Openbox/tint2 提供桌面，TigerVNC 传递画面，SDL2/LibVNCClient 把桌面内容送入 OpenXR quad layer。这样可以先完成启动、文件保存和界面流程，再逐步接入真实硬件。

## 换成设备上的真实输入和显示

进入 A081 后，关键变化是图层背后的系统。Monado 使用实际 Vulkan/DRM 图形路径，IIO 提供惯性输入，桌面应用的内容变化最终到达 DPU。应用、运行时和扫描输出开始成为一条完整的软件链。

原生会话也逐步补上常规系统能力：启动桌面应用、保存文件、录音入口和会话清理。它们沿用 Debian 的包、服务与 UCM 配置，不需要为每项功能另造一个临时运行环境。

## 这个原型留下了什么

原型把客户端上传、内容变化、合成与提交阶段分开记录，也保留了按内容变化更新的路径。这些经验后来用于理解 SteamVR、调试视频和 GPU/显示之间的关系。

QEMU 的软件渲染原型和 A081 的设备实现仍分别保存。迁移的是桌面组织方式与应用逻辑，硬件输入和图形输出通过真实设备路径接入。

继续阅读：[VR 运行时]({{ '/wiki/vr-runtime/' | relative_url }})。资料：[R02]({{ '/wiki/evidence-sources/#r02' | relative_url }})、[R03]({{ '/wiki/evidence-sources/#r03' | relative_url }})。
