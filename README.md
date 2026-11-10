# Hackintosh EFI: ASUS PRIME H610M-A D4 + Intel Core i5-12400F + AMD Radeon RX 570

[![OpenCore](https://img.shields.io/badge/OpenCore-1.0.6-blue.svg)](https://github.com/acidanthera/OpenCorePkg)
[![macOS](https://img.shields.io/badge/macOS-Sequoia%2015.8.2-brightgreen.svg)](https://www.apple.com/macos/)

*English version below | 中文版在下方*

A stable, daily-driver OpenCore EFI for a 12th-gen Intel Hackintosh. Near-native Apple experience, dual-boot with Windows 11.

---

## Hardware

| Component | Model | Status |
| :--- | :--- | :--- |
| **CPU** | **Intel Core i5-12400F** (12th-gen Alder Lake, 6C/12T) | Native turbo and power management (CPUFriend / RestrictEvents) |
| **Motherboard** | **ASUS PRIME H610M-A D4** | Sensors working (VirtualSMC / SMCSuperIO / ALC897) |
| **GPU** | **AMD Radeon RX 570 8GB** | Native, no extra driver; DP 4K UHD output, Metal acceleration |
| **RAM** | **KingBank DDR4 3200 MHz 16 GB** (single channel) | Working |
| **SSD (macOS)** | **WD Blue SN570 500 GB NVMe** | Native power management (NVMeFix) |
| **SSD (Windows)** | **KingBank 512 GB NVMe** | Windows 11 dual-boot |
| **Wi-Fi** | **Broadcom BCM94360CD** (PCIe native card, Fenvi T919) | Native 802.11ac 5 GHz |
| **Bluetooth** | **BCM20702B0** (onboard USB Bluetooth of BCM94360CD) | AirDrop / Handoff / Sidecar / Apple Watch unlock |
| **Audio** | **Realtek ALC897** (Layout-ID: 11) | Built-in input/output (AppleALC) |
| **LAN** | **Intel I219-V Gigabit** | Native gigabit (IntelMausi) |

## What's Working

- **Graphics**: AMD RX 570, native, DP 4K UHD 60–120 Hz, Metal hardware decode.
- **Apple ecosystem**: AirDrop, Handoff, Sidecar, AirPlay, Apple Watch unlock — all native, thanks to the genuine BCM94360CD.
- **Wi-Fi / Bluetooth**: native, but see the SimpleBCM note below — it is required on Sequoia.
- **Audio / microphone**: built-in headphone/speaker output and mic input switching.
- **USB**: custom `USBPorts.kext` mapping for the ASUS H610M-A, avoiding the 15-port limit.
- **Power management**: NVMeFix, CPUFriend; sleep/wake works with the power button and USB devices.
- **Dual boot**: Windows 11 boots fine from the same OpenCore menu.

## Tuning Highlights

- **CPU spoof**: the 12th-gen i5-12400F is presented to macOS as a 10th-gen CPU (`Kernel` -> `Emulate` `Cpuid1Data`) for power management; Geekbench 6 scores around 2216 single-core / 8090 multi-core.
- **GPU**: RX 570 VRAM clock raised to 1900 MHz (`ATY,MaxMClkKHz`) with a light undervolt via a custom PowerPlay table — performance close to an RX 580.
- **Thermals**: idle around 42-45 C.
- **Sleep/wake**: no power loss, no spurious midnight wake-ups.

## Key Configuration Notes

- **SimpleBCM 1.2.0 must stay enabled.** macOS Sequoia removed `IO80211FamilyLegacy`; without SimpleBCM the Wi-Fi interface disappears entirely.
- **SSDT-GPRW and its GPRW→XPRW rename patch are both disabled.** They must be switched on/off as a pair — disabling only the SSDT while the rename stays on breaks Windows boot with `ACPI_BIOS_ERROR (0xA5)`.
- **SIP is fully enabled** (`csr-active-config` `00000000`).
- **SecureBootModel is `Disabled`; boot-args is `keepsyms=1`.**
- **FileVault is enabled** on the macOS volume.

## BIOS Settings

### Enable

- **Advanced** → **CPU Configuration** → **Intel (VMX) Virtualization Technology**: `Enabled`
- **Advanced** → **System Agent (SA) Configuration** → **VT-d**: `Enabled`
- **Advanced** → **USB Configuration** → **XHCI Hand-off**: `Enabled`
- **Advanced** → **SATA Configuration** → **SATA Mode**: `AHCI`

### Disable

- **Boot** → **Fast Boot**: `Disabled`
- **Boot** → **Secure Boot** → **OS Type**: `Other OS`
- **Advanced** → **APM Configuration** → **Power On By RTC**: `Disabled`

## Before First Boot: Generate Your Own SMBIOS Codes

To protect privacy, the machine codes in `EFI/OC/Config.plist` in this repo are sanitized. **Before your first boot, generate your own with [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS):**

1. Choose model `MacPro7,1`.
2. Open `EFI/OC/Config.plist` with OpenCore Configurator or ProperTree.
3. Fill in `PlatformInfo` → `Generic`:
   - `SystemSerialNumber`
   - `MLB`
   - `SystemUUID`
   - `ROM` (your NIC's MAC address, or `000000000000`)
4. Save, copy the EFI folder to your boot drive's EFI partition, and boot.

## Credits

- [Acidanthera](https://github.com/acidanthera) for OpenCore and the core kexts.
- [Dortania](https://dortania.github.io/OpenCore-Install-Guide/) for the OpenCore Install Guide.

---

# 中文版

# 黑苹果 EFI：华硕 ASUS PRIME H610M-A D4 + Intel i5-12400F + AMD RX 570

[![OpenCore](https://img.shields.io/badge/OpenCore-1.0.6-blue.svg)](https://github.com/acidanthera/OpenCorePkg)
[![macOS](https://img.shields.io/badge/macOS-Sequoia%2015.8.2-brightgreen.svg)](https://www.apple.com/macos/)

12 代 Intel 酷睿黑苹果的稳定日用 EFI，原生体验接近白果，与 Windows 11 双系统共存。

---

## 硬件配置清单

| 部件 | 精确型号 | 驱动状态 |
| :--- | :--- | :--- |
| **处理器** | **Intel Core i5-12400F**（12 代 Alder Lake，6 核 12 线程） | 原生睿频与能耗调度（CPUFriend / RestrictEvents） |
| **主板** | **华硕 ASUS PRIME H610M-A D4** | 传感器正常（VirtualSMC / SMCSuperIO / ALC897） |
| **显卡** | **AMD Radeon RX 570 8GB** | 原生免驱，DP 4K 输出，Metal 硬件加速 |
| **内存** | **金百达 DDR4 3200MHz 16GB**（单通道） | 运行正常 |
| **固态硬盘（macOS）** | **西部数据 WD Blue SN570 500GB NVMe** | 原生电源管理（NVMeFix） |
| **固态硬盘（Windows）** | **金百达 512GB NVMe** | Windows 11 双系统 |
| **无线网卡** | **博通 BCM94360CD**（PCIe 原生免驱卡，Fenvi T919） | 原生 802.11ac 5GHz |
| **蓝牙** | **BCM20702B0**（BCM94360CD 板载 USB 蓝牙） | 隔空投送 / 接力 / 随航 / Apple Watch 解锁 |
| **板载声卡** | **Realtek ALC897**（Layout-ID: 11） | 内建输入输出正常（AppleALC） |
| **板载网卡** | **Intel I219-V 千兆网卡** | 原生千兆（IntelMausi） |

## 功能状态

- **图形加速与显示**：AMD RX 570 原生免驱，DP 4K UHD 60–120Hz 输出，Metal 硬件解码正常。
- **Apple 原生生态**：得益于 BCM94360CD 白果卡，隔空投送、接力、随航、隔空播放、Apple Watch 靠近解锁完全原生可用。
- **Wi-Fi / 蓝牙**：原生，但 Sequoia 下必须看下面 SimpleBCM 的说明。
- **音效与麦克风**：板载耳机/音箱输出、麦克风输入正常切换。
- **USB 端口**：已为华硕 H610M-A 制作专属 `USBPorts.kext` 定制驱动，规避 15 端口限制。
- **电源管理**：NVMeFix、CPUFriend；睡眠唤醒正常，支持电源键和 USB 设备唤醒。
- **双系统**：Windows 11 可从同一 OpenCore 菜单正常启动。

## 调校亮点

- **CPU 伪装**：12 代 i5-12400F 向系统伪装成 10 代 CPU（`Kernel` -> `Emulate` 的 `Cpuid1Data`），用于电源管理；Geekbench 6 跑分单核约 2216、多核约 8090。
- **显卡**：RX 570 显存频率提到 1900 MHz（`ATY,MaxMClkKHz`），配合轻度欠压的 PowerPlay 表，性能接近 RX 580。
- **温度**：待机约 42-45°C。
- **睡眠唤醒**：无掉电、无半夜异常唤醒。

## 关键配置说明

- **SimpleBCM 1.2.0 必须保持启用。**macOS Sequoia 移除了 `IO80211FamilyLegacy`，没有 SimpleBCM，Wi-Fi 接口会直接消失。
- **SSDT-GPRW 和配套的 GPRW→XPRW 改名补丁都是关闭的。**两者必须成对开关，只关 SSDT、不关改名，Windows 会直接 `ACPI_BIOS_ERROR (0xA5)` 蓝屏。
- **SIP 全开**（`csr-active-config` 为 `00000000`）。
- **SecureBootModel 为 `Disabled`；boot-args 为 `keepsyms=1`。**
- **FileVault 已加密**。

## 推荐主板 BIOS 设置

### 需开启

- **Advanced** → **CPU Configuration** → **Intel (VMX) Virtualization Technology**：`Enabled`
- **Advanced** → **System Agent (SA) Configuration** → **VT-d**：`Enabled`
- **Advanced** → **USB Configuration** → **XHCI Hand-off**：`Enabled`
- **Advanced** → **SATA Configuration** → **SATA Mode**：`AHCI`

### 需关闭

- **Boot** → **Fast Boot**：`Disabled`
- **Boot** → **Secure Boot** → **OS Type**：`Other OS`
- **Advanced** → **APM Configuration** → **Power On By RTC**：`Disabled`

## 首次引导前：生成专属于你的三码（SMBIOS）

为了保护个人隐私，本仓库 `EFI/OC/Config.plist` 中的机器三码已被脱敏：序列号与 MLB 为 `GENERATE_YOUR_OWN`，系统 UUID 为全零，ROM 为 `000000000000`。**首次引导前，请务必使用 [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS) 生成你的专属三码：**

1. 机型选择：`MacPro7,1`。
2. 用 OpenCore Configurator 或 ProperTree 打开 `EFI/OC/Config.plist`。
3. 将生成的以下四项填入 `PlatformInfo` → `Generic`：
   - `SystemSerialNumber`（序列号）
   - `MLB`（主板序列号）
   - `SystemUUID`（系统 UUID）
   - `ROM`（MAC 地址，可填网卡物理地址或 `000000000000`）
4. 保存后放入启动盘 EFI 分区即可引导。

## 鸣谢

- [Acidanthera](https://github.com/acidanthera) 团队维护的 OpenCore 及核心 Kexts 驱动。
- Dortania 编写的 [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)。
