# SPDX-License-Identifier: GPL-2.0
#
# Makefile for uiodma kernel module
#
# This module provides a UIO (Userspace I/O) framework driver with DMA buffer
# management capabilities for embedded systems. It supports multiple architectures
# (x86_64, ARM, ARM64) and cross-compilation for embedded targets.
#
# Copyright 2020 Amazon.com, Inc. or its affiliates. All Rights Reserved.
# Copyright 2020 Kinara, Inc.
# Copyright 2026 NXP

# Detect host and target architecture
# Convert armv7l/armv8l to 'arm' and aarch64 to 'arm64'
HOST_ARCH       ?= $(shell uname -m | sed -e s/arm.*/arm/ -e s/aarch64.*/arm64/)
ARCH            ?= $(shell uname -m | sed -e s/arm.*/arm/ -e s/aarch64.*/arm64/)

# Determine kernel source directory
# Use KERNEL_SRC if defined, otherwise use current running kernel's build directory
ifdef KERNEL_SRC
  KERNEL_SRC_DIR  := $(KERNEL_SRC)
else
  KERNEL_SRC_DIR  ?= /lib/modules/$(shell uname -r)/build
endif

# Set cross-compiler for ARM 32-bit builds
ifeq ($(ARCH), arm)
 ifneq ($(HOST_ARCH), arm)
   CROSS_COMPILE  ?= arm-linux-gnueabihf-
 endif
endif

# Set cross-compiler for ARM 64-bit builds
ifeq ($(ARCH), arm64)
 ifneq ($(HOST_ARCH), arm64)
   CROSS_COMPILE  ?= aarch64-linux-gnu-
 endif
endif

# Define kernel module objects
uiodma-obj           := uiodma.o
obj-$(CONFIG_U_DMA_BUF) += $(uiodma-obj)

# Determine the make target based on kernel version
# Kernels < 5.0 use 'modules', >= 5.0 use specific .ko target
ifndef UDMABUF_MAKE_TARGET
  KERNEL_VERSION_LT_5 ?= $(shell awk '/^VERSION/{print int($$3) < 5}' $(KERNEL_SRC_DIR)/Makefile)
  ifeq ($(KERNEL_VERSION_LT_5), 1)
    UDMABUF_MAKE_TARGET ?= modules
  else
    UDMABUF_MAKE_TARGET ?= uiodma.ko
  endif
endif

# Build the kernel module
all:
	$(MAKE) -C $(KERNEL_SRC_DIR) ARCH=$(ARCH) CROSS_COMPILE=$(CROSS_COMPILE) M=$(PWD) obj-m=$(uiodma-obj) $(UDMABUF_MAKE_TARGET)

# Install the kernel module to the system
modules_install:
	$(MAKE) -C $(KERNEL_SRC_DIR) ARCH=$(ARCH) CROSS_COMPILE=$(CROSS_COMPILE) M=$(PWD) obj-m=$(u-dma-buf-obj) modules_install

# Clean build artifacts
clean:
	$(MAKE) -C $(KERNEL_SRC_DIR) ARCH=$(ARCH) CROSS_COMPILE=$(CROSS_COMPILE) M=$(PWD) clean
