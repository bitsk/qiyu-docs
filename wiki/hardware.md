---
layout: default
title: A081 硬件适配
---

# A081 硬件适配

MIX 的 Linux 适配以实际板卡为中心：从原厂运行时 FDT、DTBO、固件和日志恢复硬件连接，再把它们表达为主线驱动、binding 与 DTS。SM8250 参考板及相近设备提供可复用的驱动结构，板级供电、GPIO、复位和面板参数则来自 A081 本身。

主线源码现在由 [qiyu-kernel](https://github.com/bitsk/qiyu-kernel) 维护。A081 是其中一个型号；下游修改按子系统组织，便于更新 Linux 基底。

## 从能执行到能调试

第一步是确认新内核确实执行到了 PID 1。USB 和 UFS 尚未接通时，工程先通过有界诊断、RAM 日志接力和原厂内核导出获得启动信息。日志揭示，许多看似分散的探测等待共同指向 PSCI CPU 电源域。启用 CPU idle 与 PSCI domain 驱动后，RPMh 资源依赖继续推进，USB gadget 和六个 UFS LUN 得以注册。

CPU 调频还需要补上 EPSS L3 interconnect provider。完成后，8 个 CPU、三个频率 policy、25 个 TSENS 温度区与 USB 双向控制台构成后续适配的基础。资料：[H02]({{ '/wiki/evidence-sources/#h02' | relative_url }})、[H03]({{ '/wiki/evidence-sources/#h03' | relative_url }})。

## 接入的主要子系统

| 子系统 | 实现方式与开发成果 |
| --- | --- |
| 存储与维护 | 主线 UFS、USB ACM，随后增加 osstore 上的持久 rootfs 与恢复入口 |
| CPU 与温控 | qcom-cpufreq-hw、OSM L3、TSENS、cooling devices；后续提供原厂/性能两档系统策略 |
| GPU | DRM/MSM、Adreno 650/GMU、原厂签名固件、Mesa 硬件渲染、PRIME 扫描输出 |
| 内屏 | 双 DSI、DSC、面板电源和 PHY，最终连接到桌面与 SteamVR 直显 |
| 电池与充电 | ADC5、FG4、SMB5 与 Type-C 5 V sink；标准 power_supply/thermal 接口 |
| 无线 | ath11k 的 PCI/MHI 路径、原厂固件，hci_qca 蓝牙与 Debian 系统服务 |
| 相机 | 4×OV7251、2×IMX471 的板级供电、CCI、CSI、MCLK、复位与 CAMSS |
| IMU 与距离 | SLPI/SSC 的 SEE 协议桥接到 IIO，保留传感器采样时间和标准单位 |
| 手柄传输 | GENI SPI + Nordic，独占字符设备、IRQ 时间与固定布局 ioctl |
| 音频 | WCD9385 偏置、VA 数字麦克风、ALSA/UCM 双通道录音 |
| 板级控制 | 按键输入、风扇 PWM、调压器与外设供电控制 |

双麦录音已形成 ALSA/UCM 使用路径；扬声器供电、磁力计及一些物理标定仍在后续工作中。这个总览描述各模块的工程进展，精确接口和实现以所属源码仓库为准。资料：[H04]({{ '/wiki/evidence-sources/#h04' | relative_url }})、[H08]({{ '/wiki/evidence-sources/#h08' | relative_url }})、[H09]({{ '/wiki/evidence-sources/#h09' | relative_url }})、[H11]({{ '/wiki/evidence-sources/#h11' | relative_url }})、[H12]({{ '/wiki/evidence-sources/#h12' | relative_url }})、[H13]({{ '/wiki/evidence-sources/#h13' | relative_url }})、[H14]({{ '/wiki/evidence-sources/#h14' | relative_url }})、[H15]({{ '/wiki/evidence-sources/#h15' | relative_url }})、[T01]({{ '/wiki/evidence-sources/#t01' | relative_url }})。

## 电源与系统策略

电源适配经历了“可读取”到“可控制”的过程：ADC/电池遥测先提供观测，充电器接入 Type-C 电流能力，再由 thermal 与 cpufreq/devfreq 管理运行状态。GPU 的轻载—重载—轻载路径已经体现为 305→587→305 MHz，空闲时 GPU/GMU 可进入 runtime suspend，新的提交再恢复工作。资料：[H10]({{ '/wiki/evidence-sources/#h10' | relative_url }})。

温控策略后来独立成两档。原厂档使用 schedutil 与 simple_ondemand；性能档使用 performance governor，并把大核首级温度阈值从 75°C 调到 85°C。GPU 阈值维持 95°C，独立 critical 保护保留。策略文件与 systemd 服务归 rootfs 维护，内核继续提供标准控制接口。资料：[T01]({{ '/wiki/evidence-sources/#t01' | relative_url }})。

## 源码入口

- [A081 DTS](https://github.com/bitsk/qiyu-kernel/blob/d590c72abf42c88e868eb15844fa56931e7e47bc/arch/arm64/boot/dts/qcom/sm8250-iqiyi-a081.dts)：板级连线与资源。
- [qcom-see](https://github.com/bitsk/qiyu-kernel/blob/d590c72abf42c88e868eb15844fa56931e7e47bc/drivers/iio/imu/qcom-see.c)：SEE 到 IIO。
- [Nordic 驱动](https://github.com/bitsk/qiyu-kernel/blob/d590c72abf42c88e868eb15844fa56931e7e47bc/drivers/misc/a081-nordic.c)：原始传输与内核接口。
- [A081 系统配置](https://github.com/bitsk/qiyu-rootfs/tree/b9a20b3186a54faea64edc4d2fbf9ecac488222d/devices/a081)：音频、传感器与温控配套。

显示和相机的实现过程分别见[显示与 GPU]({{ '/wiki/display-and-gpu/' | relative_url }})、[相机与传感器]({{ '/wiki/cameras-and-sensors/' | relative_url }})。
