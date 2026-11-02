---
layout: default
title: r1 源码发布与验证
---

# r1 源码发布与验证

八个代码仓库已公开发布；工程架构、Wiki 和 Blog 在独立 qiyu-docs。kernel 主分支恢复至最后一个内核相关提交 `d590c72abf42`，移除了误放的跨仓库规划，原两次文档提交在本仓库 history/ 留档。内核基底、子系统提交栈、型号构建配置和原 Release 均保留。

| 代码仓库 | 本次固定提交 |
| --- | --- |
| [qiyu-kernel](https://github.com/bitsk/qiyu-kernel) | [`d590c72abf42`](https://github.com/bitsk/qiyu-kernel/commit/d590c72abf42c88e868eb15844fa56931e7e47bc) |
| [qiyu-device-service](https://github.com/bitsk/qiyu-device-service) | [`fa2fc4267d82`](https://github.com/bitsk/qiyu-device-service/commit/fa2fc4267d82acfd13a26efece6296c791434529) |
| [qiyu-head-tracking](https://github.com/bitsk/qiyu-head-tracking) | [`3ba08d9d24a2`](https://github.com/bitsk/qiyu-head-tracking/commit/3ba08d9d24a2cc1d265624003923d8565c977fcf) |
| [qiyu-controller-tracking](https://github.com/bitsk/qiyu-controller-tracking) | [`f92ee08d919f`](https://github.com/bitsk/qiyu-controller-tracking/commit/f92ee08d919fca3c191da84b022456145e3a254e) |
| [qiyu-vr-runtime](https://github.com/bitsk/qiyu-vr-runtime) | [`c0414dde83da`](https://github.com/bitsk/qiyu-vr-runtime/commit/c0414dde83da89cbc3a2e0ba8d589bd40bf3d50b) |
| [qiyu-vr-session](https://github.com/bitsk/qiyu-vr-session) | [`0ba812ff2b5a`](https://github.com/bitsk/qiyu-vr-session/commit/0ba812ff2b5a68851999e3e604d5ea4c463da38c) |
| [qiyu-debug](https://github.com/bitsk/qiyu-debug) | [`48c7debf32af`](https://github.com/bitsk/qiyu-debug/commit/48c7debf32affbe53c7bfafb954a8acbd49cbf54) |
| [qiyu-rootfs](https://github.com/bitsk/qiyu-rootfs) | [`b9a20b3186a5`](https://github.com/bitsk/qiyu-rootfs/commit/b9a20b3186a54faea64edc4d2fbf9ecac488222d) |

## 导入与构建

- device-service / head-tracking 从配套的 camera capture-export 源树导出，公共设备头集中维护；采集库仍嵌入同一生产者。AArch64 CMake 完成设备 ABI 库、radio/Nordic 工具、头部程序/插件和观察 CLI 构建。
- controller-tracking 的 pose worker 与 wire 取自配套生产源码，ARM32 Bionic SDK helper 使用 NDK r21e 和外部提供的原厂 C++ ABI 库完成编译链接；SDK、SNPE 和固件未上传。原有辅助 C 源有初始化括号警告，编译成功。
- vr-runtime 固定 Monado `d0e85c8a4630b7da99ffec190b9f50956bc11969`，维护独立补丁。准备器导出干净上游、应用补丁、引用头部/手柄仓库的四个公共头；全部 14 个下游文件与冻结源码逐字节核对，再交叉构建 `driver_monado.so`。本次目标是 Linux SteamVR provider，未补齐或验证所有 OpenXR/compositor 路径。
- vr-session 导入现有生产会话脚本、计划与后续呈现偏移集成源码，完成打包与 Python 编译检查。保留原有 /opt 安装语义，完整安装需要其他组件和设备侧 vendor 库。
- debug 的网页、Rust 后端、API 索引和原生视频源码同仓。Cargo.lock 固定依赖，WebRTC release 后端完成 AArch64 构建，网页资源随二进制打包；IMX 与 OpenVR 时序观测 helper 也独立构建通过。video/ 的编码/转换源码保留历史配方，尚未统一为一个便携构建入口。
- rootfs 实现 Debian / SteamOS 适配器、A081 overlay、maintenance/vr/debug profile、精确组件提交锁定与组合 manifest。两种系统的 plan 和基于明确标记为合成测试基底的组装路径已运行，包含实际交叉编译设备工具；此次未重新下载/准备完整 rootfs。SteamOS 基础归档保持字节不变，overlay 单独打包；Debian foreign-stage 需要目标架构第二阶段。

rootfs 的 components.lock.json 固定选用提交和公开头摘要；固件、专有库、设备凭据与构建产物均未提交到源码仓库。

## 验收与回退

源码分库和主机编译不能替代设备、物理呈现或佩戴验收。这次没有刷写分区、安装设备服务、改变已验证系统或晋升基线。原私有 [qiyu-kernel-snapshot-r1](https://github.com/bitsk/qiyu-kernel-snapshot-r1) 和旧 Release 保留；公开 [内核 Release](https://github.com/bitsk/qiyu-kernel/releases/tag/a081-7.3.0-r1) 也未改写。
