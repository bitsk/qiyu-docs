---
layout: default
title: 让真实跟踪、Dashboard 和硬件视频一起工作
description: QVR 相机/IMU 头姿接入 SteamVR，主界面与 Venus H.264 链路逐步整合。
---

SteamVR 初期先通过 IIO 融合姿态建立运行路径，接下来把默认头部数据源推进到原厂 QVR：真实四路相机与 IMU 驱动 CDSP 输出，Monado provider 再把结果交给 SteamVR。

## 把跟踪生产者接到应用侧

数据流经过相机元数据配对、共享缓冲、QVR 输入与带时间的姿态协议。曝光和增益也成为实际开发参数，适当输入让 QVR 的有效输出进入 OpenVR。生产者与消费者之间保留质量、时间和坐标定义，使接口可以独立维护。

Gamescope/Xwayland、SteamVR、Dashboard 和 Steam VR Gamepad UI 被组织进同一会话。实际内屏开始显示双眼 Welcome 语言页与 Dashboard 下栏，退出则由会话管理恢复维护桌面。

## 接上硬件编码

远程观察需要能传递真实画面。Venus 接入补齐内核 V4L2 MEM2MEM、设备树和固件，用户态把 NV12 送入 H.264 编码，再与 WebRTC 连接。

这使远程画面从维护型 CPU 读回逐步走向硬件编码主路径。采集、编码、接收、解码和浏览器回调各自保留统计，后续优化可以针对具体阶段。

继续阅读：[VR 运行时]({{ '/wiki/vr-runtime/' | relative_url }})、[调试与视频]({{ '/wiki/debug-and-video/' | relative_url }})。资料：[S03]({{ '/wiki/evidence-sources/#s03' | relative_url }})、[S04]({{ '/wiki/evidence-sources/#s04' | relative_url }})、[S05]({{ '/wiki/evidence-sources/#s05' | relative_url }})。
