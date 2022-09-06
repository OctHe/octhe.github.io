
---
layout: post
title: "Customized ath9k on Ubuntu 22.04"
usemathjax: true
---

# Introduction

ath9k is a kernel driver that works as a kernel module for Atheros wireless IEEE802.11n NICs.
Compilation of the kernel module is the prerequisite to investigate the rate adaptation algorithm.
However, the driver (and the kernel module) highly depends on the version of the kernel, so it is difficult to compile and install even a simple driver.

This post is to introduce a basic and simple method to compile it on the newest Ubuntu 22.04.

# Source code download

The OS version is Ubuntu 22.04, and the kernel version is *5.15-generic*.
The kernel version can be found by running 
    
    uname -r

The goal is to compile ath9k driver for the current kernel.
So, the first thing is to download the right version of the source code.

We do this by using the following command

    apt source linux-image-unsigned-$(uname -r)

# Compilation the driver

First, we should go to the directory of ath driver, which is in the `./drivers/net/wireless/ath/` if the working directory is the root of the kernel source code.
Note that we don't go to ath9k directory since ath9k driver is based on the ath module, so we better compile is at the same time.

In the right directory, the follows is used to compile the driver for the current kernel

    make -C /lib/modules/`uname -r`/build M=$PWD  

where *$PWD* means print work directory.

Now, the compilation is complete, we can load the modules with 

    insmod ath.ko
    insmod ./ath9k_common.ko
    insmod ./ath9k_hw.ko
    insmod ./ath9k.ko
 
If the old version of these modules is loaded before, we need to remove them with *sudo modprobe -r <MODULE_NAME>* to remove them at first.
To check whether these modules are installed, we can list them by

    lsmod | grep ath
