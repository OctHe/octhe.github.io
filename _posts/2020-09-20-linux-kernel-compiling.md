---
layout: post
title: "How to compile the newest linux kernel on Ubuntu 20.04"
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
I have follow the [Ubuntu wiki](https://wiki.ubuntu.com/Kernel/BuildYourOwnKernel), and run the following command
```
sudo apt-get install libncurses-dev flex bison openssl libssl-dev \
dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf
```
It is worth to note that the wiki says that the above command only install part of the prerequisites, but after installing them, I can compile the kernel successfully. 
However, directly following [Ubuntu wiki](https://wiki.ubuntu.com/Kernel/BuildYourOwnKernel) has some problems.
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

# Build the linux kernel

After installing the prerequisites, the compiling is simple enough.
The linux kernel has built-in Makefiles, so we can compile the kernel with the *make* program.

In specific, we need to config some modules of the kernel. 
Since Linux kernel supports hundreds of modules, it is hard to config them one-by-one.
In stead, we only run `make xconfig` to config our linux kernel.
This command tell the compiler that we use the configuration for the new kernel the same as the configuration for the current kernel.

Then, we only need to input *make* under the *linux-5.8.7* directory.
The compile will build the kernel with all the chosen modules, so it will take some hours (3 hours in my laptop) to complete the compiling.

# Build the wireless driver

(TBD)

# Independently make the module
(TBD)
