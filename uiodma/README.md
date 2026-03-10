# UIO DMA Driver for x86 Architecture

## Overview

This directory contains the Ara2 UIO DMA kernel driver module for x86 architecture.

## Prerequisites
Before building the driver, ensure you have: make command and kernel headers installed on your host

### Compile the driver
To compile uiodma for x86 host run the below commands:
```bash
cd uiodma/
make clean
make
```

## Output
After successful compilation, the following kernel module will be generated:


- `uiodma.ko`
