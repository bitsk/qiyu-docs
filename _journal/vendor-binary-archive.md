---
layout: default
title: 原厂依赖从镜像提取迁到独立资产仓库
---

原厂固件和 SDK 依赖新增私有 qiyu-vendor 仓库。首批 A081 包含 247 个文件，按板级固件、QVR/CDSP、手柄 ARM32 SDK/Bionic 和动态 DSP 主机插件四组发布为版本化 Release 资产；rootfs 固定资产与逐文件摘要，构建直接读取下载后的缓存。

每台设备的 persist 校准单独保管，源码生成的 helper/shim 继续由组件仓库负责。工作副本中与原始记录不同的 SNPE skeleton 被排除，采用原始清单匹配版本；没有可靠载荷的手柄 MCU 固件不列入包内。

[下载与装配说明]({{ '/wiki/vendor-assets/' | relative_url }}) 记录具体入口和验证边界。该存储迁移不修改已有设备基线、内核历史或已发布内核 Release。

后续更新：同日按用户要求将 qiyu-vendor 公开，保留已发布资产和摘要。当前使用方式见上述指南。
