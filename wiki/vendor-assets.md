---
layout: default
title: 原厂二进制归档
---

# 原厂二进制归档

[bitsk/qiyu-vendor](https://github.com/bitsk/qiyu-vendor) 是公开资产仓库，Git 保存来源清单和打包工具，二进制放在 [固定 Release](https://github.com/bitsk/qiyu-vendor/releases/tag/a081-vendor-r1)。公开代码仓库只记录版本、资产 ID 和摘要；正常构建不再反复提取整份原厂镜像。

## A081 首批快照

| 包 | 内容 | 文件数 | 压缩字节 |
| --- | --- | ---: | ---: |
| firmware | GPU、ADSP/CDSP/SLPI、Wi-Fi、Bluetooth 与 regulatory 数据 | 67 | 14,375,070 |
| qvr | 原厂 DSP runtime、SNPE skeleton、型号默认 config6 | 13 | 14,302,876 |
| controller | 原厂手柄 SDK、ARM32 Bionic 依赖闭包、型号默认配置 | 164 | 40,594,836 |
| controller-dsp | SNPE 运行时动态加载的主机插件 | 3 | 14,499 |

快照 `a081-vendor-r1` 表示归档版本；未核实的 OEM 固件版本保持 unknown。前三包从既有本地文件逐项核对原始清单，动态插件另外记录 stock SDK 原目录、ELF DT_NEEDED 与 SHA-256，Git 里不存二进制；新增版本使用新标签，不覆盖旧资产。每个型号以 devices/<model>/catalog.json 维护清单和来源，按功能拆包。

一次核对发现工作目录同名 SNPE skeleton 已与原始记录不同，因此归档选用原始清单匹配的未修改版本 `7e58d46e89b9…`，不把候选修改副本当作原厂文件。自产 helper、shim 与开源 runtime 由所属源码仓库构建。

每台设备的 persist、相机/控制器校准不进入型号通用包；它们单独备份并注入。尚未取得可靠载荷的 ctrl.zip / board.zip 不推测收录。这些包不是整份设备备份，也不包含用户数据或构建出的候选 helper。

## 下载、验证、装配

在 qiyu-rootfs 仓库：

```sh
# 当前下载工具使用已登录的 gh；不在命令或源码中传令牌。
python3 tools/vendor.py fetch --output /new/vendor-cache
python3 tools/vendor.py verify --cache /new/vendor-cache

# 可只取板级固件，供 maintenance profile 使用。
python3 tools/vendor.py fetch --bundle firmware --output /new/firmware-cache

# 本地缓存与本次原生构建产物一起组装，保留基础归档元数据。
python3 tools/rootfs.py assemble --system steamos \
  --base-archive /prepared/rootfs.tar.zst \
  --vendor-cache /new/vendor-cache \
  --artifacts /private/native-artifacts.json --output /new/bundle
```

完整归档共 247 个文件，约 69 MB。Release 的 vendor.lock.json 为四包完整描述。公开源码侧的锁文件位于 devices/a081/vendor.lock.json，包含固定 Release/资产 ID、压缩包 SHA-256、大小、逐文件 SHA-256 和安装目标。下载后先核对整包，再按清单解包并逐文件核验；verify / assemble 不需要镜像或网络。manifest 明确记录 vendor_included；未输入资产缓存的配置包不代表完整 VR rootfs。

qiyu-controller-tracking 可用 `tools/build.py --ndk /path/to/ndk --vendor-cache /new/vendor-cache --output /new/controller-build` 直接使用已归档的 ARM32 libc++。head-tracking 读取 qvr/runtime/qcom/ 与 qvr/config/config6.xml；rootfs 安装目标在锁文件记录，设备校准仍单独提供。

## 验证边界

本次已完成冷下载、逐文件校验、使用下载所得 libc++ 的 ARM32 编译和合成基底 rootfs 装配；未执行原厂二进制、刷写分区、更新设备或晋升基线。完整设备验收独立记录。来源/摘要一致不等于制造商签名认证，仓库公开不构成额外的二进制许可授予。

r1，按用户要求将 qiyu-vendor 与现有 Release 公开。历史 Release 资产和摘要保持不变，当前仓库与 rootfs 锁文件均标记为 public。
