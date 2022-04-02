---
layout: post
title: "Newest Linux Kernel Compilation on Ubuntu 20.04"
usemathjax: true
---

# Introduction

This is a brief introduction about the first prcess of linux kernel development -- Compile a customized linux kernel.
The compiling process has been successfully run on the distribution of Ubuntu 20.04 (Focal Fossa), and the release of the kernel is *linux-5.8.7*.

The Linux kernel can be download from the [website](https://www.kernel.org/).
The downloaded files are under the *linux-5.8.7* directory in my case.

# Prerequisites

There are some packets that are necessary to compile the kernel. 
It is hard to obtain all the packets manually.
Fortunately, we can get the information from both the [Linux kernel documentations](https://www.kernel.org/doc/html/latest/kbuild/kbuild.html) and the Ubuntu [BuildYourOwnKernel wiki](https://wiki.ubuntu.com/Kernel/BuildYourOwnKernel).
In summary, all these packets from *apt*. 
I have followed the [Ubuntu wiki](https://wiki.ubuntu.com/Kernel/BuildYourOwnKernel), and run the following command
```
sudo apt-get build-dep linux linux-image-$(uname -r)

sudo apt-get install libncurses-dev flex bison openssl libssl-dev \
dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf
```

It is worth to note that directly following [Ubuntu wiki](https://wiki.ubuntu.com/Kernel/BuildYourOwnKernel) has some problems.
When we run the command
```
sudo apt-get build-dep linux linux-image-$(uname -r)
```
it reports an error as follows
```
E: You must put some 'deb-src' URIs in your sources.list
```
I have followed the wiki to add the contents

```
deb-src http://cn.archive.ubuntu.com/ubuntu/ focal-updates main restricted
deb-src http://cn.archive.ubuntu.com/ubuntu/ focal-updates main restricted
```
I have checked my */etc/apt/source.list* file, and make sure that the URLs have been added, but it still does not work.

Then I uncomment all commands with *deb-src* in the *sources.list* file, and run *sudo apt update*. 
The error is fixed. 

# Build and install the linux kernel

After installing the prerequisites, the compiling is simple enough.
The linux kernel has built-in Makefiles, so we can compile the kernel with the *make* program.

In specific, we need to config some modules of the kernel. 
Since Linux kernel supports hundreds of modules, it is hard to config them one-by-one.
In stead, we only run `make xconfig` to config our linux kernel.
This command tell the compiler that we use the configuration for the new kernel the same as the configuration for the current kernel.


Note: 
The new kernel has been successfully compiled by following these steps. 
The installed kernel has shown in the grub. However, the OS cannot load by the grub. 
It blocked and shows "loading initial ramdisk". 
The OS cannot load with the newest kernel, while it can load with an older version. 
This problem may be caused by a kernel bug for Intel devices.
The details can be found in [this page](https://askubuntu.com/questions/1374282/stuck-on-loading-initial-ramdisk-after-kernel-upgrade).
Otherwise, building the kernel in the [Ubuntu way](https://wiki.ubuntu.com/Kernel/BuildYourOwnKernel) may fix this problem.
But I still not try it now.

Then, we only need to input *make* under the *linux-5.8.7* directory.
The compile will build the kernel with all the chosen modules, so it will take some hours (3 hours in my laptop) to complete the compiling.

After the compile complete, we use the following command to install
```
sudo make install
sudo make modules_install
```

The final step is to update grub, so the new kernel can be found when reboot the system.
```
sudo update-grub
sudo grub-install
```

# Remove complied kernel

The complied kernel with `make install` contains the following files:

    /boot/vmlinuz*KERNEL-VERSION*
    /boot/initrd*KERNEL-VERSION*
    /boot/System-map*KERNEL-VERSION*
    /boot/config-*KERNEL-VERSION*
    /lib/modules/*KERNEL-VERSION*/
    /var/lib/initramfs-tools/*KERNEL-VERSION*/

It can be manually removed by *sudo rm -rf* command.
