---
layout: default
title: 从 Debian 到 SteamOS
---

# 从 Debian 到 SteamOS

持久 Debian 为 A081 提供了可维护的 Linux 底座：主线内核、原厂固件、设备服务、图形与调试工具在同一个安装中逐步完善。SteamOS 在此基础上使用独立 rootfs，接入原生 ARM64 Steam 和 SteamVR。

## 持久 Linux 底座

最初的诊断根目录用于启动和驱动接入。进入系统开发后，工程在保留 Android 文件的前提下，把 userdata 调整为 80 GiB，其余约 143.739 GiB 建为 ext4 osstore，用于系统、更新和回退材料。

Debian trixie arm64 采用 debootstrap：主机准备 foreign stage，设备原生完成第二阶段。initramfs 挂载 osstore，再 switch_root 到持久 Debian。首次 systemd 集成还补齐了 CONFIG_FILE_LOCKING，随后 sysusers、udev 与系统服务能正常工作。资料：[H06]({{ '/wiki/evidence-sources/#h06' | relative_url }})、[H07]({{ '/wiki/evidence-sources/#h07' | relative_url }})。

## SteamOS 用户态与 A081 内核组合

SteamOS 基础来自固定版本的 Steam Frame 官方 ARM64 recovery。准备流程校验 ZIP、磁盘镜像及目标分区摘要，导出保留所有者、权限与 xattrs 的 rootfs；硬件侧继续使用 A081 内核、DTS 和设备固件。

独立 SteamOS 根目录位于 osstore 的 systems/steamos/rootfs，Debian 则保留在 systems/debian/rootfs。正常开机进入 Android；recovery 入口默认 Debian，选择 SteamOS 通过一次性 rootfs 标记完成。进入 SteamOS 后，再由独立选项决定维护桌面或 VR 主界面。资料：[S01]({{ '/wiki/evidence-sources/#s01' | relative_url }})、[S05]({{ '/wiki/evidence-sources/#s05' | relative_url }})。

## 桌面、Steam 与设备服务

维护桌面由普通用户、logind、KWin 和 Xwayland 组成。KWin 加载私有 Mesa/GBM 组合，Steam 客户端使用匹配的 ARM64 运行库；Frame 专用硬件服务和挂载配置按 A081 调整。

桌面侧统一使用 NetworkManager 管理网络，PipeWire 使用 A081 UCM 音频路由。内核补齐 madvise、membarrier 和 FUSE 等用户态依赖，使 CEF、launcher service 及辅助运行库可以在目标环境工作。初始里程碑是 Steam 登录界面、Big Picture、维护桌面自启动及 Debian/Android 往返。资料：[S01]({{ '/wiki/evidence-sources/#s01' | relative_url }})。

## SteamVR 会话如何进入

原生 ARM64 SteamVR 2.17.10 通过 Monado provider 获取实际设备状态。Gamescope 提供 Xwayland 环境，SteamVR compositor 经私有 Turnip 直显，Welcome/Dashboard 与 Steam 网页部分使用匹配的系统图形路径。会话管理器负责停启桌面、准备私有注册表、启动跟踪/运行时并在退出后恢复维护环境。资料：[S02]({{ '/wiki/evidence-sources/#s02' | relative_url }})、[S05]({{ '/wiki/evidence-sources/#s05' | relative_url }})。

Steam 客户端与主界面接通是系统集成成果；游戏和兼容层使用属于后续独立阶段。

## rootfs 保留更多系统的入口

[qiyu-rootfs](https://github.com/bitsk/qiyu-rootfs) 把系统拆为三个可组合维度：

```text
systems/<system>/        基础来源与准备适配器
devices/<model>/         设备描述与文件 overlay
profiles/<profile>.json  功能组合
components.lock.json     精确组件提交与公共接口摘要
```

Debian 和 SteamOS 是首批配方。新增系统接入统一适配器，新增型号复用组件与装配方式；维护、VR、调试 profile 选择需要的功能。基础归档与设备 overlay 分开保存，组合 manifest 记录实际来源和摘要。

入口：[rootfs 组合工具](https://github.com/bitsk/qiyu-rootfs/blob/b9a20b3186a54faea64edc4d2fbf9ecac488222d/tools/rootfs.py)、[SteamOS 配方](https://github.com/bitsk/qiyu-rootfs/tree/b9a20b3186a54faea64edc4d2fbf9ecac488222d/systems/steamos)、[Debian 配方](https://github.com/bitsk/qiyu-rootfs/tree/b9a20b3186a54faea64edc4d2fbf9ecac488222d/systems/debian)。构建用法见[构建与发布]({{ '/wiki/build-and-release/' | relative_url }})。
