---
layout: default
title: 构建输入与版本锁定
---

# 构建输入与版本锁定

构建以功能仓库的源码和固定版本输入为准。组件选择、原厂依赖和发布产物分别锁定：

- qiyu-rootfs 的 components.lock.json 固定组件提交与接口头摘要。
- devices/a081/vendor.lock.json 固定原厂资产版本、整包与逐文件 SHA-256。
- qiyu-vr-runtime 的 upstream.json 固定 Monado 上游与补丁。
- qiyu-kernel 的型号配置、构建脚本与 Release SHA256SUMS 配套使用。

设备校准、网络凭据和设备身份由使用者在本地单独提供。站点按主题记录实现和验证边界，不公开工作站目录或活动时间。
