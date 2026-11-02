---
layout: default
title: 分离内核、功能源码与工程文档
---

内核以 `qiyu-kernel` 为主仓库，A081 的初始下游提交栈建立在 Linux 7.3 的真实主线记录之上。运行时按共享采集、头部跟踪、手柄跟踪、VR 接入和会话管理拆分；调试前后端同仓，rootfs 保留系统适配器、设备和功能 profile 的扩展维度。

跨仓库规划从 kernel 主分支迁到本仓库的 Wiki；Blog 记录变化过程。内核仓库保留内核源码、型号构建说明和维护记录。

七个功能源码仓库已完成首次发布，包含来源摘要和可执行的构建/打包入口。共享采集、头部跟踪、ARM32 SDK helper、SteamVR provider、Rust/WebRTC 后端及原生观测 helper 已离线编译；会话脚本完成打包和语法验证，rootfs 完成两种系统组合的合成基底组装验证，未重新生成完整系统或实机验收。没有操作设备或晋升基线。具体边界见 [架构规划]({{ '/wiki/architecture/' | relative_url }})。

[源码发布与验证记录]({{ '/wiki/source-publication/' | relative_url }}) 包含具体提交和限制。
