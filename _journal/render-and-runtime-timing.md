---
layout: default
title: 优化 SteamVR 渲染与呈现调度
description: 聚焦 Turnip 查询等待、显示缓冲与可持久化的会话时序。
---

这一阶段从已经运行的 SteamVR 出发，缩小优化范围：固定 Dashboard/语言页视角，关闭画面采集，对照 compositor 的 CPU、GPU 和提交节奏，再调整有明确作用位置的代码。

## 减少查询等待的额外开销

Turnip 的修改保留原有 depth-cache 处理，将查询等待设为 20 µs，并让显示路径至少分配三个 image。GPU 完成与呈现完成检查继续保留，运行包也去掉了诊断构建的额外日志。

固定场景对照中，OpenVR 帧序号速率由约 48.60 提升到 50.23～51.28 Hz，compositor CPU 时间下降约 43%～51%。这些数据给出了本轮修改的实际收益范围，也让后续工作能围绕剩余 GPU 和显示开销继续推进。

## 把时序配置变成会话能力

会话侧将 -11 ms 的运行时偏移保存为配置，并在 compositor 重启后重新应用。完整重启后，正式跟踪、手柄、调试与观看服务能够按既有入口恢复。配置由会话层管理，图形补丁继续由运行时图形目录维护。

视频路径同样保留单独观测：一轮有界原生 H.264 任务完成 120 帧编码和 120 帧解码。这里的渲染、编码和软件回调数据各有对应的测量范围，实际游戏与佩戴体验留给后续使用阶段。

继续阅读：[显示、GPU 与后续优化]({{ '/wiki/display-and-gpu/' | relative_url }})。资料：[P01]({{ '/wiki/evidence-sources/#p01' | relative_url }})、[P02]({{ '/wiki/evidence-sources/#p02' | relative_url }})。
