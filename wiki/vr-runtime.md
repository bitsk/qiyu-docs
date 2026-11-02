---
layout: default
title: VR 运行时开发
---

# VR 运行时开发

VR 用户态的开发连接两端：底层是相机、IMU、DSP 和原始手柄数据，上层是 OpenXR/OpenVR 应用与合成器。工程复用 Monado、原厂 QVR/CDSP 和手柄 SDK，把主要工作集中在平台接口、坐标、时间、共享缓冲及会话组织上。

## 从桌面原型到真实设备

XR 桌面最初使用 Openbox/tint2、TigerVNC、SDL2/LibVNCClient 和 OpenXR quad layer，在 amd64 QEMU 中验证界面与桌面交互。随后切到 A081 的原生 Vulkan、Monado、IIO 姿态和真实显示输出：桌面内容成为 OpenXR 图层，由设备 compositor 输出到内屏。这个过程把应用、运行时和设备图形路径连接了起来。资料：[R02]({{ '/wiki/evidence-sources/#r02' | relative_url }})、[R03]({{ '/wiki/evidence-sources/#r03' | relative_url }})。

## 头部跟踪：IIO 与 QVR

早期 SteamVR provider 先消费 IIO 融合姿态，形成可运行的 3DoF 路径；此后接入实际相机/IMU 驱动的 QVR 输出。QVR 的核心继续在原厂 CDSP 固件中运行，Linux 侧通过 FastRPC、共享 DMA 和文件服务提供所需环境。

接通顺序从 open/get_version 开始，随后是初始化、共享映射、start、IMU/相机输入和 pose 输出。原厂固件需要的独立 root-PD 文件服务、signed user-PD 及旧版映射接口分别补齐。头部服务把结果转成带时间、质量与坐标说明的协议，Monado/OpenVR provider 再发布给 SteamVR。资料：[R01]({{ '/wiki/evidence-sources/#r01' | relative_url }})、[S03]({{ '/wiki/evidence-sources/#s03' | relative_url }})。

## 手柄：原厂视觉与 Bionic helper

手柄链路由 Nordic 原始传输、手柄 IMU/按键、相机短曝光和原厂 SDK 组成。原厂视觉侧已有亮点检测、LED 模型匹配、P3P/PnP 和 EKF 融合，因此开发重点是恢复它在 Linux 上需要的输入和运行环境。

原厂 SDK 是 ARM32/Bionic，SteamVR provider 则在 ARM64/glibc 中工作。独立 helper 负责加载 SDK，再通过固定协议交换输入与姿态。配套工作包括双手无线时间、实际 SOF 相位、图像缓冲释放、头姿和 SNPE/DSP 依赖；camera2 SOF 的 GPIO92/93 门控及匹配的 DSP 运行库是重要实现节点。r1 已接通双手基本位置输出，并整合到正式开机入口。资料：[R04]({{ '/wiki/evidence-sources/#r04' | relative_url }})、[R05]({{ '/wiki/evidence-sources/#r05' | relative_url }})。

## 光学与坐标

光学校正复用原厂 QVR 的模型与参数，Monado 侧实现缩放、径向畸变和 RGB 修正。头部、相机、手柄及眼睛各有自己的坐标系；tracker-to-eye、安装方向和时间定义由接口合同表达。原厂模型形成可运行候选后，仍需结合实际佩戴进一步调整。资料：[S06]({{ '/wiki/evidence-sources/#s06' | relative_url }})。

## 会话组织与功能仓库

```text
主线内核 / 固件
       ↓
统一设备采集 ───→ 调试预览与遥测
       ↓
头部 QVR/CDSP     手柄 SDK helper
       └──────┬──────┘
          VR provider
               ↓
       SteamVR / OpenXR 应用
```

会话管理贯穿启动、生产者/消费者连接、超时处理、正常 shutdown 和桌面恢复。正式入口包含 tracking supervisor、controller relay、运行计划和记录工具，由 qiyu-vr-session 维护；分仓不会强制每个模块新增一个常驻进程。资料：[R06]({{ '/wiki/evidence-sources/#r06' | relative_url }})。

| 仓库 | 代码入口 |
| --- | --- |
| qiyu-device-service | [采集、时钟与输入接口](https://github.com/bitsk/qiyu-device-service/tree/fa2fc4267d82acfd13a26efece6296c791434529/include/qiyu/device) |
| qiyu-head-tracking | [QVR 桥接与头部服务](https://github.com/bitsk/qiyu-head-tracking/tree/3ba08d9d24a2cc1d265624003923d8565c977fcf/src) |
| qiyu-controller-tracking | [SDK helper 与 ABI 适配](https://github.com/bitsk/qiyu-controller-tracking/tree/f92ee08d919fca3c191da84b022456145e3a254e/src) |
| qiyu-vr-runtime | [Monado 来源](https://github.com/bitsk/qiyu-vr-runtime/blob/c0414dde83da89cbc3a2e0ba8d589bd40bf3d50b/upstream.json)、[下游补丁](https://github.com/bitsk/qiyu-vr-runtime/tree/c0414dde83da89cbc3a2e0ba8d589bd40bf3d50b/patches) |
| qiyu-vr-session | [会话与资源生命周期](https://github.com/bitsk/qiyu-vr-session/tree/0ba812ff2b5a68851999e3e604d5ea4c463da38c/src) |

各阶段的软件接通、物理标定与使用体验范围，简要保留在对应开发记录和[来源附录]({{ '/wiki/evidence-sources/' | relative_url }})中。
