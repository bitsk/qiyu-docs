---
layout: default
order: 2
title: 从临时诊断环境走向持久 Debian
description: 保留 Android 数据，建立 osstore，让主线设备支持进入真正的 Linux 系统。
---

驱动接通之后，开发需要一个能保存软件、服务和用户文件的系统。临时诊断根目录逐渐让位于 osstore 上的持久 Debian：同一套主线硬件支持开始服务于常规 Linux 用户态。

## 给多个系统留出位置

设备数据需要保留。核对已有用量后，userdata 保留 80 GiB，其余约 143.739 GiB 建成 ext4 osstore。这里存放 Linux rootfs、更新材料和回退镜像，也为后来独立安装 SteamOS 留出了空间。

Debian trixie arm64 使用 debootstrap 准备：主机下载和解包，设备原生完成第二阶段。initramfs 挂载 osstore，再切换到持久根目录，交给 systemd 启动。

## 把驱动能力变成系统能力

systemd 首次集成暴露出文件锁配置缺口，加入 CONFIG_FILE_LOCKING 后，sysusers 和 udev 等基础服务恢复正常。接下来，无线、设备固件服务、ALSA 工具、图形工具和维护控制台被逐项加入这个安装。

GPU 也从最小指令验证推进到 Mesa 硬件渲染：kmscube 的 dma-buf 经 PRIME 交给 DPU，退出后回到文字控制台。CPU/GPU 调频、温度、电池和充电则逐渐接到标准系统接口。这样，后面的相机、DSP 和 VR 开发可以直接复用一个持久系统。

正常开机仍保留 Android，recovery 提供 Linux 入口；系统之间可以往返，旧镜像与模块继续留作回退。

继续阅读：[从 Debian 到 SteamOS]({{ '/wiki/steamos/' | relative_url }})。资料：[H06]({{ '/wiki/evidence-sources/#h06' | relative_url }})、[H07]({{ '/wiki/evidence-sources/#h07' | relative_url }})、[H08]({{ '/wiki/evidence-sources/#h08' | relative_url }})、[H09]({{ '/wiki/evidence-sources/#h09' | relative_url }})、[H10]({{ '/wiki/evidence-sources/#h10' | relative_url }})、[H15]({{ '/wiki/evidence-sources/#h15' | relative_url }})。
