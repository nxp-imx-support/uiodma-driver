<div align="center">

# UIO DMA Driver for Ara240 DNPU

[![License badge](https://img.shields.io/badge/License-GPL--2.0--only-blue)](./LICENSE)
[![Kernel badge](https://img.shields.io/badge/Type-Out--of--tree_Kernel_Module-green)]()
[![Platform badge](https://img.shields.io/badge/Platform-x86_|_ARM-orange)]()

</div>

Linux kernel driver providing userspace I/O (UIO) access to DMA-capable memory regions.

---

## Overview

The UIO DMA driver is an out-of-tree Linux kernel module that enables userspace applications to allocate and manage DMA (Direct Memory Access) buffers through the UIO (Userspace I/O) framework. This allows zero-copy data transfers between hardware devices and userspace applications.

### DMA Buffer Management for NPU

The UIO DMA driver enables:
- Zero-copy data transfer to Ara240 NPU
- Efficient input tensor allocation
- Direct memory access for inference results
- Reduced latency for AI/ML pipelines
---

## Architecture

This repository contains **two versions** of the driver:

### 1.- `uiodma/` - Standard x86 Version

- **Platform**: x86/x86_64 architectures
- **Cache Model**: Cache-coherent systems
- **Use Case**: Standard PC/server platforms with hardware cache coherency

### 2️.- `uiodma_cache_management/` - NXP i.MX Version 

- **Platform**: ARM-based NXP i.MX SoCs (i.MX 8M Plus, i.MX 95, etc.)
- **Cache Model**: Implements explicit cache management operations
- **Use Case**: Embedded ARM platforms requiring manual cache synchronization
 
**Features**:
  - Manual cache flush/invalidate operations
  - Proper handling of non-coherent DMA
  - Optimized for ARM memory subsystems

> 💡 **Important**: Always use `uiodma_cache_management/` for NXP i.MX devices to ensure data coherency between CPU and DMA operations.

---

##  Prerequisites

### Build Requirements

- Linux kernel headers matching your running kernel
- GCC compiler
- Make build system
- Root/sudo privileges for module installation

#### Install kernel headers (example for Debian/Ubuntu-based systems):

```bash
sudo apt-get update
sudo apt-get install linux-headers-$(uname -r)
```

#### For NXP i.MX Yocto builds:

Ensure kernel headers are available in your BSP. The kernel version should match your target device (e.g., 6.12.49-2.2.0 for recent i.MX releases).

---

## Build Instructions

### For x86 Platforms

```bash
cd uiodma/
make clean
make
```

### For NXP i.MX Platforms (ARM)

#### Option 1: Native build on i.MX device

```bash
cd uiodma_cache_management/
make clean
make
```

#### Option 2: Cross-compilation on host PC

```bash
source <toolchain-path>/environment-setup-armv8a-poky-linux
cd uiodma_cache_management/
make clean
make KERNEL_SRC=<path-to-kernel-source>
```

---

## Installation

### For x86 Platforms

#### Load the module manually:

```bash
sudo insmod uiodma/uiodma.ko
```

### For NXP i.MX Platforms (Runtime SDK Integration)

#### ⚠️ **Critical**: SDK Integration Path

When using with **rt-sdk-ara2** (Ara240 Runtime SDK) on i.MX devices, the module **must** be placed at:

```
/usr/share/rt-sdk-ara240/driver/uiodma.ko
```

#### Step 1: Deploy to SDK path

```bash
scp uiodma_cache_management/uiodma.ko root@<i.mx_ip_addr>:/usr/share/rt-sdk-ara240/driver/
```

>  **Note**: The Ara240 Runtime SDK expects the UIO DMA driver at this specific location. Do not modify the path unless you also update the Ara240 SDK configuration.

---
## ⚖️ License

This driver is licensed under **GPL-2.0-only**.

---

**Note**: This is an out-of-tree kernel module. It is not part of the mainline Linux kernel and must be maintained separately for each kernel version.