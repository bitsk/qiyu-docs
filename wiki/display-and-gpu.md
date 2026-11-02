---
layout: default
title: 显示与 GPU
---

# 显示与 GPU

A081 的图形链路跨过三个层次：Adreno 执行渲染，DPU/双 DSI 驱动内屏，桌面或 VR 合成器组织最终画面。工程从最小 GPU 命令开始，逐步推进到 Mesa、KMS、SteamVR 和呈现调度。

## 先接通 GPU 与内屏

早期 Adreno 验证通过 DRM/MSM 创建 GEM 缓冲区，提交 CP_MEM_WRITE，再等待 fence 并回读结果。这一步确认 GPU 命令、地址空间和同步通路。随后 Debian 的 kmscube 采用真实 Mesa FD650 渲染，dma-buf 经 PRIME 交给 DPU；文字控制台也通过 DRM fbdev emulation 接到同一块内屏。资料：[H04]({{ '/wiki/evidence-sources/#h04' | relative_url }})、[H09]({{ '/wiki/evidence-sources/#h09' | relative_url }})。

内屏接入需要同时恢复面板供电、双 DSI、DSC 压缩参数、时钟与 PHY。排查画面异常时，工程用 DPU 硬件纯色绕过 framebuffer 读取，缩小了问题范围；再对照原厂活动寄存器检查 DSC、lane 和时钟。最终定位到 PLL bias 引用计数与 SYSTEM_MUXES 初始化不一致，补齐真实初始化后恢复可见画面。资料：[H05]({{ '/wiki/evidence-sources/#h05' | relative_url }})。

## 从扫描缓冲到双眼画面

| 表示 | 尺寸或用途 |
| --- | --- |
| 内屏物理扫描缓冲 | 1920×3664，已用模式为 72 Hz |
| 双眼横向内容 | 3664×1920 |
| 单眼面板区域 | 1832×1920 |
| 维护桌面 | KWin 处理逻辑方向，提供常规桌面操作 |
| SteamVR 直显 | compositor 输出双眼内容，再交给真实 KMS/DSI |

方向和布局由应用、合成器与扫描输出共同决定，不能只修改一处宽高。早期定位图以左右眼颜色、十字和不对称标记核对显示路径，之后才接入 Welcome 页面与 Dashboard。资料：[S02]({{ '/wiki/evidence-sources/#s02' | relative_url }})、[S05]({{ '/wiki/evidence-sources/#s05' | relative_url }})、[P01]({{ '/wiki/evidence-sources/#p01' | relative_url }})。

## 为什么需要分离 GPU/KMS 的 Turnip

Frame 的原始 Turnip 路径只枚举 GPU primary，无法直接取得 A081 分离的 msm-kms 内屏。适配在显式选定 A081 KMS 节点时，把 GPU 渲染/同步与 KMS 扫描输出连接起来，沿用真实 dma-buf、atomic/pageflip 和 present wait。

部署采用私有图形目录：维护桌面使用与已验证设备路径匹配的 Mesa/GBM，SteamVR 直显使用私有 Turnip，Steam/CEF 网页部分保留各自匹配的运行库。这样可以单独维护直显补丁和运行库组合。资料：[S02]({{ '/wiki/evidence-sources/#s02' | relative_url }})、[S05]({{ '/wiki/evidence-sources/#s05' | relative_url }})。

## 后续渲染优化

11 月的优化聚焦查询等待与显示缓冲：保留已有 depth-cache 处理，把查询轮询等待设为 20 µs，显示路径最少使用三个 image，同时保留 GPU 和呈现完成检查。

在固定姿态的 SteamVR Dashboard/语言页场景、关闭采集的对照中，OpenVR 帧序号速率从约 48.60 提升到 50.23～51.28 Hz，compositor CPU 时间下降约 43%～51%。这些数字对应记录中的固定场景，游戏性能与物理显示速率需要各自测量。会话层另把 -11 ms 的运行时偏移持久化，并在 compositor 重启后重新应用。资料：[P01]({{ '/wiki/evidence-sources/#p01' | relative_url }})、[P02]({{ '/wiki/evidence-sources/#p02' | relative_url }})。

源码见 [Turnip 补丁与构建配方](https://github.com/bitsk/qiyu-vr-runtime/tree/c0414dde83da89cbc3a2e0ba8d589bd40bf3d50b/graphics)；系统如何选择这些库见[SteamOS 集成]({{ '/wiki/steamos/' | relative_url }})。
