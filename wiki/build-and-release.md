---
layout: default
title: 构建与发布
---

# 构建与发布

代码仓库提供各组件的实际构建入口，qiyu-docs 说明如何组合它们。源码版本首先固定，再构建目标产物，最后由 rootfs manifest 记录所选系统、型号和组件摘要。

## 内核

qiyu-kernel 保留真实 Linux 主线历史，驱动、binding/DTS 和配置按子系统提交。A081 的构建与 rebase 入口在 [devices/a081/MAINTAINING.md](https://github.com/bitsk/qiyu-kernel/blob/main/devices/a081/MAINTAINING.md)；内核专用文档与源码一起维护。

## 用户态组件

| 组件 | 当前入口 | 主要外部输入 |
| --- | --- | --- |
| device-service | CMakeLists.txt | qiyu-kernel 的 UAPI |
| head-tracking | CMakeLists.txt | 相邻 device-service、目标编译器 |
| controller-tracking | tools/build.py | Android NDK、外部 ARM32 C++ ABI 库 |
| vr-runtime | tools/build.py | 固定 Monado 上游、头部/手柄接口、工具链 |
| vr-session | tools/package.py | 会话源码与运行计划 |
| debug | tools/build.py、tools/build_helpers.py | GStreamer/WebRTC sysroot、共享设备接口、OpenVR 头 |

例如，在相邻检出功能仓库后，头部服务可这样交叉构建：

```sh
cmake -S qiyu-head-tracking -B build/head \
  -DCMAKE_BUILD_TYPE=Release \
  -DCMAKE_C_COMPILER=aarch64-linux-gnu-gcc \
  -DCMAKE_CXX_COMPILER=aarch64-linux-gnu-g++ \
  -DQIYU_KERNEL_DIR=/absolute/path/to/qiyu-kernel
cmake --build build/head -j4
```

不同组件可能针对 ARM64/glibc 或 ARM32/Bionic，分别使用其 README 中的依赖和编译方式。SDK、原厂固件与运行库由设备资料或合法来源单独提供，源码仓库保存适配代码、接口与来源说明。

## 固定上游和接口

vr-runtime 的 upstream.json 固定 Monado 提交、补丁摘要以及来自 head/controller 仓库的协议头。构建器导出干净上游、应用补丁并核对下游文件，再生成 provider。公共头由所属组件维护，rootfs 锁文件记录选用版本。

首次公开导入的组件提交见[源码发布记录]({{ '/wiki/source-publication/' | relative_url }})。后续组合以 qiyu-rootfs 的 components.lock.json 为准。

## 系统、型号与功能组合

```sh
python3 qiyu-rootfs/tools/rootfs.py plan \
  --system debian --device a081 --profile debug
python3 qiyu-rootfs/tools/rootfs.py plan \
  --system steamos --device a081 --profile vr
```

prepare 调用对应系统适配器准备基础归档；assemble 接收基础归档和显式产物清单，输出基础归档副本、设备 overlay 与 manifest。Debian foreign-stage 还需要原生第二阶段。新增系统、型号或 profile 使用相同装配入口。

## 发布记录

组合 manifest 固定组件提交、接口摘要和产物 SHA-256。工程里程碑与实现过程写入 Wiki/Blog；具体设备记录的测量范围留在来源附录，已发布标签、Release 与回退材料保持可追溯。

原厂依赖现在由公开 qiyu-vendor Release 提供，qiyu-rootfs 的 devices/<model>/vendor.lock.json 锁定下载内容；tools/vendor.py 先验证整包，再逐文件校验并生成安装映射。缓存准备好后 verify / assemble 均可离线执行。每台设备的 persist 校准单独注入；完整系统和设备验收仍是独立门槛。
