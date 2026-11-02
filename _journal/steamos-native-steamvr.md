---
layout: default
order: 5
title: 把 SteamOS 和原生 SteamVR 接到 MIX
description: 独立 ARM64 rootfs、匹配的桌面图形栈与分离 GPU/KMS 的直显路径。
---

SteamOS 的接入复用了已有 Linux 硬件底座。基础用户态来自固定版本的 Steam Frame 官方 ARM64 recovery，A081 继续使用自己的主线内核、设备树和设备固件；独立 rootfs 放到 osstore，与 Debian 并存。

## 先恢复一个可操作的系统

rootfs 准备过程校验归档、镜像和目标分区，保留所有者、权限与 xattrs。设备侧再调整 Frame 专用服务、挂载、网络和音频配置，接入 A081 的硬件服务。

桌面通过普通用户、logind、KWin 和 Xwayland 运行。KWin 使用私有 Mesa/GBM 组合，Steam 使用匹配的 ARM64 库。配合 madvise、membarrier、FUSE 等内核配置，登录界面、Big Picture 与原生 launcher service 逐步运行起来。

## 把内屏交给 SteamVR

原生 ARM64 SteamVR 2.17.10 使用 Monado provider 获取设备姿态。A081 的 GPU 与 KMS 节点分离，而原始 Frame Turnip 路径无法直接枚举这块内屏，因此增加了显式选择 A081 KMS 的适配：GPU 负责渲染和同步，KMS 负责实际 atomic/pageflip 输出。

图形库按用途装到私有目录，桌面、Steam 网页和直显会话各用匹配组合。双眼定位图把左右眼颜色和方向带到实际内屏，后续 Welcome 和 Dashboard 也沿着同一条输出路径接入。

继续阅读：[SteamOS 集成]({{ '/wiki/steamos/' | relative_url }})、[显示与 GPU]({{ '/wiki/display-and-gpu/' | relative_url }})。资料：[S01]({{ '/wiki/evidence-sources/#s01' | relative_url }})、[S02]({{ '/wiki/evidence-sources/#s02' | relative_url }})、[S05]({{ '/wiki/evidence-sources/#s05' | relative_url }})。
