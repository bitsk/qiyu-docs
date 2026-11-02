---
layout: default
title: 调试系统与视频
---

# 调试系统与视频

调试系统把设备状态、跟踪输入和实际画面放到同一入口。它逐步从远程桌面与只读探针，发展为 Rust 后端、网页前端、H.264/WebRTC 和分相位相机预览。

## 调试台的功能组织

[qiyu-debug](https://github.com/bitsk/qiyu-debug) 将 frontend、backend、api 与原生视频 helper 放在一个仓库。前端提供会话、设备、头部/手柄、相机和视频控制页面，后端负责状态接口、WebSocket、会话桥接和视频连接。VR 与跟踪服务可以独立运行，网页按需接入。

Spatial UI 把这些信息组织为适合桌面和移动端的调试台。r1 已完成设备部署和浏览器检查，随后主要后端路径统一到 Rust。资料：[D02]({{ '/wiki/evidence-sources/#d02' | relative_url }})、[D03]({{ '/wiki/evidence-sources/#d03' | relative_url }})。

## 相机预览怎样共享输入

追踪相机通过已有生产者的 tap 提供预览，消费者以 latest-only 方式读取，不延长算法持有图像的时间。六个相机角色分别显示 live、stale、waiting 等状态；四路追踪输入与独立透视相机的结果使用各自标识。

后续界面把短、短、长三个相位分开展示，便于查看手柄亮点图像和头部跟踪输入。预览来自实际源帧及相位标记，仍受采集与编码的实际速度约束。资料：[D01]({{ '/wiki/evidence-sources/#d01' | relative_url }})、[D03]({{ '/wiki/evidence-sources/#d03' | relative_url }})。

## 远程画面的 H.264/WebRTC 路径

```text
显示 / 相机生产者
       ↓
GPU 转换与旋转 / NV12
       ↓
Venus H.264 编码
       ↓
WebRTC → 浏览器解码与显示
```

最初的 CPU 读回与 JPEG 适合维护用途，后续主路径转向实际 NV12、Venus 硬件编码和 WebRTC。内核补齐 V4L2 MEM2MEM、Venus 节点与固件，用户态连接转换、编码和传输；左右眼、码率、分辨率与视频权限也纳入会话控制。资料：[S04]({{ '/wiki/evidence-sources/#s04' | relative_url }})、[D03]({{ '/wiki/evidence-sources/#d03' | relative_url }})。

## 如何读页面上的视频指标

| 指标 | 对应阶段 |
| --- | --- |
| capture | 取得源图像或完成转换 |
| encoded | 编码器产生码流 |
| received / decoded | 接收并解码帧 |
| browser callback | 浏览器提交了新的可显示视频帧 |
| latency | 带有明确起止点和时间基准的延迟 |

这些阶段分开记录，方便定位瓶颈。11 月的一轮原生 H.264 任务完成 120 帧编码和 120 帧软件解码；浏览器持续呈现与长期视频会话另有自己的记录。资料：[P01]({{ '/wiki/evidence-sources/#p01' | relative_url }})。

源码：[网页](https://github.com/bitsk/qiyu-debug/tree/48c7debf32affbe53c7bfafb954a8acbd49cbf54/frontend)、[Rust 后端](https://github.com/bitsk/qiyu-debug/tree/48c7debf32affbe53c7bfafb954a8acbd49cbf54/backend)、[接口索引](https://github.com/bitsk/qiyu-debug/blob/48c7debf32affbe53c7bfafb954a8acbd49cbf54/api/routes.json)、[原生视频源码](https://github.com/bitsk/qiyu-debug/tree/48c7debf32affbe53c7bfafb954a8acbd49cbf54/video)。构建用法以仓库 README 为准。
