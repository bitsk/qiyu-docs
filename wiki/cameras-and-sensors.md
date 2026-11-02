---
layout: default
title: 相机、IMU 与共享采集
---

# 相机、IMU 与共享采集

A081 的跟踪输入由四颗 OV7251、两颗 IMX471 和经 SLPI/SSC 管理的 IMU 组成。适配的重点从基础接线推进到统一采集、带身份的帧缓冲和可用于算法输入的时间信息。

## 六颗相机的接入

| 角色 | 传感器 | 板级与采集工作 |
| --- | --- | --- |
| 追踪 slot 0～3 | 4×OV7251 | 主线 sensor + CAMSS，四路并发，RAW10/Y10P 到用户态 Y8 |
| 透视 slot 4～5 | 2×IMX471 | 供电、CCI、PHY、MCLK 与基础双路出图 |

先逐组验证接线与真实图像，再推进四路 OV7251 并发。共享 GPIO/FSIN 配置使四路读出由各自运行转为约 74.97657 Hz；记录中的稳态 SOF 差异约为一个 QTimer tick。这里描述硬件读出时间，实际曝光关系另由曝光控制和传感器时序解释。资料：[H12]({{ '/wiki/evidence-sources/#h12' | relative_url }})。

四路追踪与两路透视分别形成了可用采集路径。当前资料中的“六个相机角色”包括独立验证的透视输入，不代表已完成六路同时运行的组合。

## 图像与 QCFT 元数据

为了把实际缓冲区和时间对应起来，CAMSS 增加 DMA SOF header 与 QCFT 元数据队列。QCFT v1 每条 160 字节，携带 stream_id、图像 sequence/index、SOF/EOF、MONOTONIC/BOOTTIME 转换及相关质量信息。用户态按身份配对图像和元数据，再提交给跟踪后端。

夹读时钟对会受到调度影响。后续实现最多三次有界重采完整时钟 tuple，仍保留 `<5000 ns` 的选择条件，从而在真实 SteamVR 负载下取得更多可用输入。资料：[H12]({{ '/wiki/evidence-sources/#h12' | relative_url }})、[S03]({{ '/wiki/evidence-sources/#s03' | relative_url }})。

## IMU：从 SEE 到 IIO

IMU 由 SSC 管理。qcom-see 通过 QRTR/QMI SEE 发现加速度计和陀螺仪，把数据送到标准 IIO buffer。用户态得到 XYZ、SI 单位和样本时间戳，普通请求频率覆盖 25/50/100/200/500 Hz，也有 batch 和 flush 接口。

时钟实现由早期首包映射改为夹取 AP counter 与 IIO clock，再扣除 SEE 样本年龄。这使启动时间和批量到达不再被误当成实际采样时刻。采样记录保留测得的时间间隔，后续算法直接消费这些时间戳。资料：[H11]({{ '/wiki/evidence-sources/#h11' | relative_url }})。

## 头显与手柄共用采集

原厂的 tracking 和 ctrl-tracking 是两组输入角色，使用同一套四路相机。用户态构成两个 1280×480 Y8 横向 mosaic，每个 mosaic 含两路 640×480 图像。曝光按短、短、长分时组织，手柄使用短曝光输入，QVR 按自身规则筛选头部跟踪帧。

统一采集层保留图像身份、时间和缓冲生命周期，算法通过共享缓冲或 IPC 消费。相机预览走 producer tap，因此无需为了打开调试网页再次争用追踪相机。手柄校时还用到 camera2 SOF 驱动的 GPIO92/93 相位信号，这成为后续手柄定位接通的关键。资料：[R04]({{ '/wiki/evidence-sources/#r04' | relative_url }})、[R05]({{ '/wiki/evidence-sources/#r05' | relative_url }})、[D01]({{ '/wiki/evidence-sources/#d01' | relative_url }})。

源码入口：[共享采集与设备接口](https://github.com/bitsk/qiyu-device-service/tree/fa2fc4267d82acfd13a26efece6296c791434529/include/qiyu/device)、[相机输入合同](https://github.com/bitsk/qiyu-head-tracking/blob/3ba08d9d24a2cc1d265624003923d8565c977fcf/api/camera-input-contract.json)、[IMU 输入合同](https://github.com/bitsk/qiyu-head-tracking/blob/3ba08d9d24a2cc1d265624003923d8565c977fcf/api/imu-input-contract.json)。
