---
layout: default
order: 11
title: 把内核、VR 功能与工程文档分开维护
description: 保留可更新的主线提交栈，按功能组织源码，把 Wiki 和 Blog 放到独立工程站点。
---

硬件、系统和 VR 开发积累到一定程度后，源码需要按功能分开维护。内核以 qiyu-kernel 为主仓库，在真实 Linux 主线记录上保留 A081 下游提交；驱动、binding/DTS 和配置继续按子系统组织，方便以后更新基底。

用户态分成统一设备采集、头部跟踪、手柄跟踪、VR runtime 和会话管理。调试网页、后端、API 与视频 helper 保持同仓；rootfs 用系统适配器、设备 overlay 和功能 profile 组合所需模块，给更多系统和奇遇型号留下入口。

分仓围绕代码所有权和接口展开。统一采集仍可作为库嵌入唯一生产者，头部与手柄继续共享硬件输入；系统装配通过固定提交和摘要选择组件。

工程文档从 kernel 主分支迁入 qiyu-docs：Wiki 按模块解释实现与维护，Blog 按开发阶段讲述过程。首批历史整理串起主线启动、持久 Debian、相机与 DSP、SteamOS、手柄、调试台和渲染优化。

继续阅读：[仓库架构]({{ '/wiki/architecture/' | relative_url }})、[开发历程]({{ '/wiki/development-history/' | relative_url }})、[源码版本记录]({{ '/wiki/source-publication/' | relative_url }})。
