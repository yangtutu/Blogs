---
title: Device
date: 2021-03-16
description: Device Commands
---

# Device Information

## CPU

``` bash
dmidecode --type processor | grep 'Version' | awk -F':' '{print $2}'
Intel(R) Xeon(R) CPU E5-2678 v3 @ 2.50GHz
Intel(R) Xeon(R) CPU E5-2678 v3 @ 2.50GHz
```

## GPU 

``` bash
lspci | grep -i vga
01:00.0 VGA compatible controller: NVIDIA Corporation Device 2184 (rev a1)
```

``` bash
nvidia-smi --query-gpu=name --format=csv,noheader
GeForce RTX 2070
```

## NPU

``` bash
npu-smi info -m
        NPU ID               Chip ID              Chip Logic ID        Chip Name
        0                    0                    0                    Ascend 910A
        1                    0                    1                    Ascend 910A
        2                    0                    2                    Ascend 910A
        3                    0                    3                    Ascend 910A
        4                    0                    4                    Ascend 910A
        5                    0                    5                    Ascend 910A
        6                    0                    6                    Ascend 910A
        7                    0                    7                    Ascend 910A
```

## Memory

``` bash
lshw -short -C memory
H/W path        Device           Class          Description
===========================================================
/0/0                             memory         64KiB BIOS
/0/35                            memory         16GiB System Memory
/0/35/0                          memory         [empty]
/0/35/1                          memory         [empty]
/0/35/2                          memory         16GiB DIMM DDR3 Synchronous Unbuffered (Unregistered) 1600 MHz (0.6 ns)
/0/35/3                          memory         [empty]
/0/3b                            memory         256KiB L1 cache
/0/3c                            memory         1MiB L2 cache
/0/3d                            memory         6MiB L3 cache
/0/100/1f.2                      memory         Memory controller
```

``` bash
cat /proc/meminfo | grep 'MemTotal'
MemTotal:       16351520 kB
```
