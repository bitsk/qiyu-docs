---
layout: default
title: 开发资料与来源
---

# 开发资料与来源

本附录保存本次历史整理使用的资料索引。正文优先介绍功能和实现过程，需要追溯时可按编号找到原始记录。路径相对本地 A081 工程归档；原始文件未随本站公开，页面不会把这些路径包装成下载链接。

[完整来源摘要与摘选字段]({{ '/evidence/history-sources.json' | relative_url }}) 记录 SHA-256、大小与 JSON Pointer。公开代码使用固定提交链接，组件版本另见[源码发布记录]({{ '/wiki/source-publication/' | relative_url }})。

以下为 r1 整理时所读快照。历史文件中可能保留较早阶段的状态，正文按对应事件和子系统记录说明，不把某一个旧字段当作全部工程的现状。

### H01 · 原厂依据与主线接入
{: #h01}

归档路径：`reports/PORTING.md`

SHA-256：`f068614fdcc01c7737aca92ba73767242fa379e8de334b33cd261cec17e9523f`

### H02 · PSCI 电源域与早期日志
{: #h02}

归档路径：`reports/early-evidence-results.json`

SHA-256：`75f6528c5301dd83230b5955a5f4c24b2b7601f7a6bfdb4b5158fcc20288256f`

### H03 · CPU、温度与 USB 控制台
{: #h03}

归档路径：`reports/core-bringup-results.json`

SHA-256：`1e5d564bc50ad939782ab47e55f87f78cd12b21f5e5061de58b4192aeb159568`

### H04 · Adreno 指令执行
{: #h04}

归档路径：`reports/gpu-bringup-results.json`

SHA-256：`e7e0f2578d414bcf5f3a5143edd3d4157b2a2b466cc7058d320a339af9ad6ca3`

### H05 · 显示 PHY 与画面恢复
{: #h05}

归档路径：`TARGET.json`

SHA-256：`83aba8cf99a2af083c47411dfe55b04673f841319a5cd1a5cf410f3dca85f686`

### H06 · osstore 与保留 Android 数据
{: #h06}

归档路径：`plans/OSSTORE-STATE.json`

SHA-256：`2c5fd8e4bdcc1bdf58f03761b174f530ae7bb2c7be788feb59baf8b59ef5ff0c`

### H07 · Debian 持久启动
{: #h07}

归档路径：`reports/device/debian-r1/completion.json`

SHA-256：`91fdc7f583bdc48c40f1fa091acbe18adf6d6148c5e38eaa9b8df7c73bcfa9eb`

### H08 · Debian 无线
{: #h08}

归档路径：`reports/device/debian-radio-profiles-r1/results.json`

SHA-256：`481f0d6f03b4c71f5f621ac19afa442875a9ade706a335371418d13cc6f7da8e`

### H09 · fbcon、Mesa 和 PRIME 输出
{: #h09}

归档路径：`reports/device/fbcon-debian-r1/results.json`

SHA-256：`041dd8e37d8cede664cd4898471809f37437b0b7bd730c998c63b56968714091`

### H10 · GPU devfreq 与 runtime PM
{: #h10}

归档路径：`reports/device/gpu-pm-debian-r1/results.json`

SHA-256：`c0e0382b012018be73c455f525dfc3bae6a26d78033ec34ea52245969cfd4295`

### H11 · SEE/IIO 采样与时钟
{: #h11}

归档路径：`reports/device/imu-rate-debian-r1/results.json`

SHA-256：`94050a71c84036aa5a21e6c8be7ebbc7314d5244397f71571a153035abd6ae7c`

### H12 · 四路相机读出与 DMA header
{: #h12}

归档路径：`reports/device/camera-barrier-r1/results.json`

SHA-256：`5652278e68e50c119133d7655c26e26111ec021b1dd4d06812e64e73ded828e1`

### H13 · 双麦与 UCM
{: #h13}

归档路径：`reports/device/audio-dmic-init-r1/results.json`

SHA-256：`144b9dff9ed2512662a923042920f21bd4eb469c26e2c52494277b5c0a6e7e10`

### H14 · 距离传感器接口
{: #h14}

归档路径：`reports/device/proximity-r1/results.json`

SHA-256：`94846ce8a31252f085fae57d07d7fef566244da020b3595b0e4c472929ec3bf8`

### H15 · 电池与充电
{: #h15}

归档路径：`reports/device/power-debian-r1/results.json`

SHA-256：`14c1e0f4782adec0c7fd59ee297283bce0d4e56a43cdb9dfcde200a4e02fe925`

### R01 · QVR/CDSP 桥接过程
{: #r01}

归档路径：`xr-desktop/system/tracking/head/README.md`

SHA-256：`8bf65859fdb657e65b34953f484c4ec5c2b7b669ac434aad035f43f062f7ee30`

### R02 · 原生 XR 桌面与性能记录
{: #r02}

归档路径：`xr-desktop/README.md`

SHA-256：`9676dba354270830cf1ab6ed33f5b051924ad2cf884e75909d8b1ebda7e3969d`

### R03 · 原生 XR 桌面后续
{: #r03}

归档路径：`reports/device/xr-system-r1/results.json`

SHA-256：`a789f23ac03c7187f80d639bead1f40a63687b38f2a1b6e509fd47047ee07484`

### S01 · SteamOS 独立系统与桌面
{: #s01}

归档路径：`reports/device/steamos-mix-r1/results.json`

SHA-256：`05a0749a1e9a2dc3dfa39f7da1213c2f0dba7f61c6b6d4b0ba54eee4ce91f6e9`

### S02 · 初始 ARM64 SteamVR
{: #s02}

归档路径：`reports/device/steamvr-mix-r1/results.json`

SHA-256：`c4f39d0fa818c4ac61d29ae8d89d983c8a788942d7dd3a1efac9cd9ce1c3f895`

### S03 · QVR 头部与 QCFT 长时输入
{: #s03}

归档路径：`reports/device/steamvr-head6dof-r1/results.json`

SHA-256：`00101a741da3f39ed6d2efe1976b1260b9770143340363f55987997648042fcf`

### S04 · Venus 与远程视频
{: #s04}

归档路径：`reports/device/steamos-venus-r1/results.json`

SHA-256：`dd515525911138a608522a416f8f8c95cd43f08680c8e0aa237cfc5a2365862c`

### S05 · SteamVR 主界面与会话
{: #s05}

归档路径：`steamos-mix/README.md`

SHA-256：`df70400d2cee363e9233d48a67a4309a7ec59f0bc6feda7574122e727df81085`

### S06 · 原厂光学校正候选
{: #s06}

归档路径：`reports/device/steamos-optics-r1/results.json`

SHA-256：`230d20bceb57902d529183e3a72cf9d5c3c2025d24340b0b4f3a880d6e588248`

### R04 · Nordic、SOF 门控与 SDK 修复
{: #r04}

归档路径：`reports/nordic-stock-driver-analysis.md`

SHA-256：`6e2a0d47cf52f08f3237ab59dae468c95ab604c93fa9c301e89af297a8e5a6fd`

### R05 · 手柄正式入口与位置输出
{: #r05}

归档路径：`reports/device/controller-stock-reset-r1/results.json`

SHA-256：`d2b86f2b237c1d0d7125d71f2ddc7759b37ad2ce30f304eecbaca9fb3b81d978`

### R06 · 正式会话包来源
{: #r06}

归档路径：`build/controller-production-runtime-r1/package.json`

SHA-256：`d08ac5f4d9f6f0ab195be97bf14216857354f94ae4396f5b65c2e6bd2651cbf8`

### D01 · 相机调试预览
{: #d01}

归档路径：`reports/device/debug-cameras-r1/results.json`

SHA-256：`5d685a727a893319963bb993ec8b1ec1ec6e420d5c624618f5a617a1ffc3e23c`

### D02 · Spatial 调试界面
{: #d02}

归档路径：`reports/device/debug-spatial-ui-r1/results.json`

SHA-256：`fe0e758541ddd004427dc7272b1ff7dfedfb4452ccdf2b5b2377dc74f212fad7`

### D03 · Rust、分相位视频与正常开机
{: #d03}

归档路径：`reports/device/phase-video-r1/normal-start/results.json`

SHA-256：`aa577cb48adc9d11184140c79f5a3f38d6fa468d753c4182ec06f4e4850515e5`

### T01 · 原厂与性能温控档
{: #t01}

归档路径：`reports/device/thermal-profiles-r1/results.json`

SHA-256：`b5ac7b0914c1e3f252cc2223e7bdb896171940610e6af30a870d85640b2045fa`

### P01 · Turnip 渲染优化
{: #p01}

归档路径：`reports/device/steamvr-render-next-r1/results.json`

SHA-256：`250fbe8297c7336f7e5655d86335c4876a509eeb973dccda945f832eb195c0ee`

### P02 · 运行时偏移与重启持久化
{: #p02}

归档路径：`TARGET.json`

SHA-256：`83aba8cf99a2af083c47411dfe55b04673f841319a5cd1a5cf410f3dca85f686`

### M01 · 组件源码发布与固定版本
{: #m01}

归档路径：`build/qiyu-project-publish-r1/qiyu-docs/releases/r1-components.json`

SHA-256：`59acdb9099d4c048374f9d979aeaa1c5a721c5d4f3dd02c6f9efac4fc00e7b14`
