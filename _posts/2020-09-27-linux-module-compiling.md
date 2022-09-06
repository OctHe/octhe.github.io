---
layout: post
title: "Kernel Module Compilation on Ubuntu 20.04"
usemathjax: true
---

# Introduction

Linux kernel composes of hundreds of modules.
The user can compile and load modules according to the requirement.
Additionally, independently compiling the module will save lots of time because compile the whole kernel usually needs three or four hours.

This is a brief introduction about how to independently compile each kernel module with the kernel source code.


# Prerequisites

There are some packets that are necessary to compile the kernel. 
All these packets can be get from *apt*. 
I have follow the [Ubuntu wiki](https://wiki.ubuntu.com/Kernel/BuildYourOwnKernel), and run the following command

Specifically, first open the */etc/apt/source.list* with *sudo* and uncomment add commands with *deb-src* in this file.
After saving the file, run *sudo apt update* to update the packet list.
Then, run the following command to install all the dependents.
```
sudo apt-get build-dep linux linux-image-$(uname -r)

sudo apt-get install libncurses-dev flex bison openssl libssl-dev \
dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf
```

The next step is to download the kernel source code. 
One can download the newest kernel from [the official website](https://www.kernel.org/).
However, in this website is difficult to download the old version. 

Instead, we choose to download the source code by using *apt*.
```
sudo apt install linux-source-5.4.0
```
Then the corresponding version of the source code can be downloaded. 
Our version is 5.4.0, which is the kernel version of Ubuntu 20.04.
The director of this source code can be found in */usr/src/linux-source-5.4.0*.
The source code is a compressed file, and run `sudo tar jvxf linux-source-5.4.0.tar.bz2` to uncompress it.

# Compile a module

Compile a module is very simple.
Linux kernel uses *Kbuild* to compile the kernel. 
We can use this tool by using *make* like other programs.
*Kbuild* is at the root directory of the source code (i.e., at ./linux-source-5.4.0/ in my case).
To compile a module, we first need to know the subdirectory of that module, and use *make* command at the root directory of the source code.

```
Note: compile the module also need to at the ./linux-source-5.4.0/ directory, 
or the compiling process would lack some files.
```

As an example, we use the compilinig process of *iwlwifi* module, which is the wireless Linux driver of Intel wireless network card.

The source code of this module is at */usr/src/linux-source-5.4.0/linux-source-5.4.0/drivers/net/wireless/intel/iwlwifi/* in my case. 
To compile it, we use 

```
make oldconfig && make prepare
make # This command is to compile the kernel
     # which is the prerequisit of the module compilation
sudo make M=./drivers/net/wireless/intel/iwlwifi modules
```

It worth to note that this command is inputted at */usr/src/linux-source-5.4.0/linux-source-5.4.0/*.
After the compiling process is over, we can find a file named *iwlwifi.ko* at the *iwlwifi* directory.

To install it, we can use the following command
```
cd ./drivers/net/wireless/intel/iwlwifi
insmod iwlwifi.ko
```
`insmod` means "insert module".
This is a command to install Linux kernel module.

If *iwlwifi* module has been installed in the kernel, you need to remove it from the kernel with `sudo modprobe -r iwlwifi`.
Then, run the `insmod` to install the compiled module.

# Some command about the Linux module

All the usage are an example of the *iwlwifi* module.

## lsmod

`lsmod` is a command to list all installed module in the kernel.
It contains some information such as name, dependencies, etc.

Usage: `lsmod`

## modinfo

`modinfo` can display details about a specific module.
It can display the information of either a installed module or a compiled module.

Usage:

`modinfo iwlwifi`

This command display the information of the installed module *iwlwifi*, not the compiled module.

`modinfo iwlwifi.ko`

This command display the information of the compiled module, so the command need to be run at the *iwlwifi/* directory.

## modprobe

`modprobe` is usually used to install or remove a module.

Usage: 

`sudo modprobe -r iwlwifi`

Remove the iwlwifi module.

`sudo modprobe iwlwifi`

Install the iwlwifi module.


## insmod

`insmod` can install a customized module.
If the module with the same name has been installed, `sudo modprobe -r iwlwifi` is needed to remove the pre-installed module.

Usage:

`insmod iwlwifi.ko`
