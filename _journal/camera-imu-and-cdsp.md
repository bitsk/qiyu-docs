---
layout: default
title: 接通相机、IMU 和原厂 CDSP 跟踪核心
description: 让真实图像和惯性数据进入 Linux，并恢复 QVR 所需的 RPC 与共享内存。
---

有了持久 Debian，开发转向 VR 最需要的输入：相机、IMU 和原厂跟踪核心。目标逐渐从“设备能被发现”推进到“图像和惯性样本能成为算法输入”。

## 六颗传感器与统一采集

A081 使用四颗 OV7251 和两颗 IMX471。接入工作包括 CCI、CSI、MCLK、复位与电源，再由 CAMSS 输出真实图像。先逐组恢复出图，再推进四路 OV7251 并发和共享同步控制。

四路追踪相机的读出被组织到共同节奏。软件随后把图像缓冲、DMA SOF header 和逐帧时间信息连接起来，发展为带有帧身份的 QCFT 元数据接口。对跟踪而言，这比只有一个可打开的视频节点更进一步：消费者可以知道自己提交的是哪一帧、对应哪个时刻。

IMU 由 SSC 管理，Linux 侧通过 SEE 协议转成 IIO 数据。保留样本自身的时间戳、单位和批量信息，使它可以与相机输入一起送往用户态。

## 让原厂算法在 Linux 上得到所需环境

QVR 的原厂核心运行在 CDSP。适配先打通版本查询，然后依次恢复初始化、共享映射、真实 IMU/相机输入与姿态输出。独立 root-PD 文件服务、signed user-PD、随机数设备和旧版 FastRPC 映射接口，都在这个过程中补齐。

原厂核心继续承担算法工作，Linux 负责提供一致的数据、共享缓冲和 RPC 环境。这条路线后来成为 SteamVR 头部跟踪的生产者。

继续阅读：[相机与传感器]({{ '/wiki/cameras-and-sensors/' | relative_url }})、[VR 运行时]({{ '/wiki/vr-runtime/' | relative_url }})。资料：[H11]({{ '/wiki/evidence-sources/#h11' | relative_url }})、[H12]({{ '/wiki/evidence-sources/#h12' | relative_url }})、[R01]({{ '/wiki/evidence-sources/#r01' | relative_url }})。
