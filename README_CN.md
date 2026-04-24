# OP-TEE Trusted OS (STM32MP2平台)

本仓库包含OP-TEE项目的安全侧实现源代码，专为STM32MP2系列处理器优化。

## 项目简介

OP-TEE (Open Portable Trusted Execution Environment) 是一个开源的可信执行环境实现，为ARM处理器提供安全服务。本项目是OP-TEE针对STMicroelectronics STM32MP2系列多核处理器的移植版本。

## 支持的硬件平台

### STM32MP25系列
- **STM32MP257F-DK** - ST官方Discovery开发板
- **STM32MP257F-EV1** - ST官方评估板
- **正点原子STM32MP257DAK 1+8G** - 正点原子STM32MP257开发板 (1GB RAM + 8GB eMMC)

### STM32MP23系列
- **STM32MP235F-DK** - ST官方Discovery开发板

### STM32MP21系列
- **STM32MP215F-DK** - ST官方Discovery开发板

## 正点原子STM32MP257DAK 1+8G板子支持

本项目已支持正点原子STM32MP257DAK 1+8G开发板，该板子基于STM32MP257DAK3处理器，具有以下特性：

- **处理器**: STM32MP257DAK3 (双核Cortex-A35 + Cortex-M33)
- **内存**: 1GB DDR4 RAM
- **存储**: 8GB eMMC
- **接口**: 以太网、USB、HDMI、MIPI-DSI、CAN、SPI、I2C、UART等

### 设备树配置

正点原子STM32MP257DAK板子使用外部设备树配置，设备树文件位于：
```
CA35/DeviceTree/STM32MP257DAK3/optee-os
```

### 构建配置

使用以下命令构建针对正点原子STM32MP257DAK板子的OP-TEE：

```bash
# 设置环境变量
export EXTDT_DIR=<外部设备树目录路径>

# 构建OP-TEE (安全服务模式)
make -f Makefile.sdk.stm32mp2 CFG_EMBED_DTB_SOURCE_FILE=stm32mp257f-dk.dts

# 构建OP-TEE (最小系统服务模式)
make -f Makefile.sdk.stm32mp2 CFG_EMBED_DTB_SOURCE_FILE=stm32mp257f.dts CFG_STM32MP_PROFILE=system_services
```

### 内存配置

正点原子STM32MP257DAK 1+8G板子的内存配置：
- **DDR起始地址**: 0x80000000
- **DDR大小**: 1GB (0x40000000)
- **TZDRAM起始地址**: 0x82000000
- **TZDRAM大小**: 32MB (0x02000000)

## 构建说明

### 依赖项

1. **工具链**: aarch64-ostl-linux-gcc
2. **设备树**: STM32MP257设备树文件
3. **FIP工具**: 用于生成Firmware Image Package

### 构建步骤

```bash
# 1. 克隆仓库
git clone <repository-url>
cd stm32mp2-optee-os

# 2. 设置环境变量
export CROSS_COMPILE=aarch64-ostl-linux-
export ARCH=arm

# 3. 构建OP-TEE
make -f Makefile.sdk.stm32mp2 all

# 4. 构建FIP镜像
make -f Makefile.sdk.stm32mp2 fip
```

### 配置选项

- **CFG_STM32MP_PROFILE**: 选择服务配置
  - `secure_and_system_services`: 完整安全服务 (默认)
  - `system_services`: 仅系统服务

- **CFG_EMBED_DTB_SOURCE_FILE**: 指定设备树文件
  - `stm32mp257f-dk.dts`: STM32MP257F-DK板子
  - `stm32mp257f-ev1.dts`: STM32MP257F-EV1板子

## 主要特性

- **安全启动**: 支持ARM TrustZone技术
- **加密服务**: 支持AES、RSA、SHA等加密算法
- **安全存储**: 支持安全存储和密钥管理
- **SCMI协议**: 支持系统控制和管理接口
- **远程处理器管理**: 支持Cortex-M33协处理器管理
- **可信用户界面**: 支持TUI安全显示

## 相关文档

- [OP-TEE官方文档](http://optee.readthedocs.io)
- [STM32MP2参考手册](https://www.st.com/en/microcontrollers-microprocessors/stm32mp2-series.html)
- [正点原子STM32MP257开发板资料](https://www.alientek.com/)

## 许可证

本项目基于BSD-3-Clause和GPL-2.0-or-later双许可证发布。

## 支持与反馈

如有问题或建议，请通过以下方式联系：
- 提交Issue到本仓库
- 访问[OP-TEE安全公告](https://github.com/OP-TEE/optee_os/security)

---

**注意**: 使用前请检查[安全公告](https://github.com/OP-TEE/optee_os/security)中的已知安全问题。