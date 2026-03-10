# UIO DMA Driver ARM architectures

## Overview

This directory contains the Ara240 UIO DMA kernel driver module for ARM architectures.

## Prerequisites
If you are cross-compiling make sure you have set the appropriate environment variables for ARCH and CROSS_COMPILE.

### Compile the driver

To compile uiodma for an ARM host run the below commands:
```bash
cd uiodma/
make clean
make KERNEL_SRC=<path-to-kernel-source>
```

## Output
After successful compilation, the following kernel module will be generated:


- `uiodma.ko`


