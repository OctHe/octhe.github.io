---
title: Kernel
layout: default
---

# Kernel 

## Building the Kernel

This is a brief introduction about build a customized linux kernel.
The compiling process has been successfully run on the distribution of Ubuntu 20.04 (Focal Fossa), and the release of the kernel is "linux-5.8.7".

The Linux kernel can be download from the [website](https://www.kernel.org/).
The downloaded files are under the "linux-5.8.7" directory.

### Prerequisites

There are some packets that are necessary to compile the kernel. 
It is hard to obtain all the packets manually.
Fortunately, we can get the information from both the [Linux kernel documentations](https://www.kernel.org/doc/html/latest/kbuild/kbuild.html) and the Ubuntu [BuildYourOwnKernel Wiki](https://wiki.ubuntu.com/Kernel/BuildYourOwnKernel).
Run the following command

@example
sudo apt-get build-dep linux linux-image-$(uname -r)

sudo apt-get install libncurses-dev flex bison openssl libssl-dev \
dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf
@end example

It is worth to note that directly following [BuildYourOwnKernel Wiki](https://wiki.ubuntu.com/Kernel/BuildYourOwnKernel) has some problems.
When run the command

@example
sudo apt-get build-dep linux linux-image-$(uname -r)
sudo apt-get build-dep linux linux-image-$(uname -r)
@end example

it reports an error as follows

@example
E: You must put some 'deb-src' URIs in your sources.list
@end example

Please add the contents

@example
deb-src http://cn.archive.ubuntu.com/ubuntu/ focal-updates main restricted
deb-src http://cn.archive.ubuntu.com/ubuntu/ focal-updates main restricted
@end example

Then uncomment all commands with "deb-src" in the `/etc/apt/sources.list` file, and run `sudo apt update`. 
The error is fixed. 

### Build and install the linux kernel

After installing the prerequisites, the building process is simple enough.
The linux kernel has built-in Makefiles.
In specific, the first process is to config some modules of the kernel. 
Since Linux kernel supports hundreds of modules, it is hard to config them one-by-one.
In stead,  run `make oldconfig` to config our the kernel.
This command tell the compiler that use the configuration for the new kernel the same as the configuration for the current kernel.
All the new features of the new kernel will be set as the default value, so type "enter" for all these options is enough.

Note: 
The new kernel has been successfully compiled by following these steps. 
The installed kernel has shown in the grub. However, the OS cannot load by the grub. 
It blocked and shows "loading initial ramdisk". 
The OS cannot load with the newest kernel, while it can load with an older version. 
This problem may be caused by a kernel bug for Intel devices.
The details can be found in [this page](https://askubuntu.com/questions/1374282/stuck-on-loading-initial-ramdisk-after-kernel-upgrade).
Otherwise, building the kernel in the [Ubuntu way](https://wiki.ubuntu.com/Kernel/BuildYourOwnKernel) can fix this problem.

However, during the building process, if the error occurs with `llvm-strip:not found`.
This message is due to the command `llvm-strip` does not exist in `/usr/bin/` directory.
On the other hand, in the directory we can find a command named as `llvm-strip-10`.
To fix this, we can create a symbolic link (i.e., soft link) with

`sudo ln -s /usr/bin/llvm-strip-10 /usr/bin/llvm-strip`

The command will build the kernel with all the chosen modules, so it will take some hours to complete the compiling.

After the compile complete, the following command will install the kernel

@example
sudo make install
sudo make modules_install
@end example

The final step is to update grub, so the new kernel can be found when reboot the system.
@example
sudo update-grub
sudo grub-install
@end example

### Remove Kernel

The built kernel contains the following files:

@example
/boot/vmlinuz*KERNEL-VERSION*
/boot/initrd*KERNEL-VERSION*
/boot/System-map*KERNEL-VERSION*
/boot/config-*KERNEL-VERSION*
/lib/modules/*KERNEL-VERSION*/
/var/lib/initramfs-tools/*KERNEL-VERSION*/
@end example

It can be manually removed by `sudo rm -rf <kernel_files>` command.

## Kernel Modules

Linux kernel composes of hundreds of modules.
The user can compile and load modules according to the requirement.
Additionally, independently compiling the module will save lots of time because compile the whole kernel usually needs three or four hours.

### Prerequisites

There are some packets that are necessary to compile the kernel. 
All these packets can be get from apt. 
Please follow the [Ubuntu wiki](https://wiki.ubuntu.com/Kernel/BuildYourOwnKernel) (if use Ubuntu), and run the commands.

Specifically, first open the "/etc/apt/source.list" with `sudo` and uncomment commands with "deb-src" in this file.
After saving the file, run `sudo apt update` to update the packet list.
Then, run the following command to install all the dependents.

@example
sudo apt-get build-dep linux linux-image-$(uname -r)

sudo apt-get install libncurses-dev flex bison openssl libssl-dev \
dkms libelf-dev libudev-dev libpci-dev libiberty-dev autoconf
@end example

The next step is to download the kernel source code. 
One can download the newest kernel from [the official website](https://www.kernel.org/).
However, in this website is difficult to download the old version. 

Instead, the aternative method is to download the source code by using "apt".

`sudo apt install linux-source-5.4.0`

Then the corresponding version of the source code can be downloaded. 
The kernel version of Ubuntu 20.04 is 5.4.0.
The  of this source code can be found in "/usr/src/linux-source-5.4.0".
The source code is a compressed file, and run `sudo tar jvxf linux-source-5.4.0.tar.bz2` to uncompress it.

### Build the Modules

Build a module or modules is very simple.
Linux kernel uses "Kbuild" to compile the kernel. 
"Kbuild" is at the root directory of the source code (i.e., at ./linux-source-5.4.0/ in my case).
Build the module needs to at the ./linux-source-5.4.0/ directory.

As an example, we use the compilinig process of "iwlwifi" module, which is the wireless Linux driver of Intel wireless network card.

The source code of this module is at `/usr/src/linux-source-5.4.0/linux-source-5.4.0/drivers/net/wireless/intel/iwlwifi/` in this case. 

@example
make oldconfig && make prepare
make # This command is to compile the kernel
     # which is the prerequisit of the module compilation
sudo make M=./drivers/net/wireless/intel/iwlwifi modules
@end example

After the compiling process is complete, a file named as "iwlwifi.ko" at he iwlwifi directory can be found.

To install it, use the following command

@example
cd ./drivers/net/wireless/intel/iwlwifi
insmod iwlwifi.ko
@end example

`insmod` means "insert module".
This is a command to install Linux kernel module.

If the iwlwifi module has been installed in the kernel, the old modules can be removed from the kernel with `sudo modprobe -r iwlwifi`.
Then, run the `insmod` command to install the compiled module.

### Command about Module

All the usage are an example of the *iwlwifi* module.

@subsubsection lsmod

`lsmod` is a command to list all installed module in the kernel.
It contains some information such as name, dependencies, etc.

Usage: `lsmod`

@subsubsection modinfo

`modinfo` can display details about a specific module.
It can display the information of either a installed module or a compiled module.

Usage: `modinfo iwlwifi`

This command display the information of the installed module "iwlwifi", not the compiled module.

Usage: `modinfo iwlwifi.ko`

This command display the information of the compiled module.

@subsubsection modprobe

`modprobe` is usually used to install or remove a module.

Usage: `sudo modprobe -r iwlwifi`

Remove the iwlwifi module.

Usage: `sudo modprobe iwlwifi`

Install the iwlwifi module.

@subsubsection insmod

`insmod` can install a customized module.
If the module with the same name has been installed, `sudo modprobe -r iwlwifi` is needed to remove the pre-installed module.

Usage: `insmod iwlwifi.ko`

## mac80211

### Rate Control

Rate control (or rate adaptation) is an important algorithm for wireless devices to select the optimal bit rate under different wireless channel conditions.
In mac80211 subsystem in Linux kernel, rate control is part of the subsystem, and it can be self-designed by each wireless driver.
For example, ath9k driver uses the default rate control (i.e., `https://wireless.wiki.kernel.org/en/developers/documentation/mac80211/ratecontrol/minstrel, minstrel`), while iwlwifi uses a custom rate control named `https://wiki.gentoo.org/wiki/Iwlwifi, "iwl-agn-rs"`.

mac80211 provides a struct `https://docs.huihoo.com/doxygen/linux/kernel/3.7/structrate__control__ops.html, "rate_control_ops"` with multiple callbacks.
To implement a new rate control algorithm, someone must define these callbacks in `rate_control_ops`.
For example, minstrel includes two types of algorithm, one is in the file `rc80211_minstrel.c` (mac80211_minstrel) and the other is in the file `rc80211_minstrel_ht.c` (mac80211_minstrel_ht).

For iwlwifi driver, `rate_control_ops` is defined in `dvm/mac80211.c` (rs_ops) and `mvm.mac80211.c` (rs_mvm_ops_drv), depend on different types of drivers.

`rate.c` defines all rate control functions for different drivers.
Ideally, the developer does not need to change the function in `rate.c`.

Two basic functions in this file is `ieee80211_rate_control_register` and `ieee80211_rate_control_unregister`.
The driver loads different rate control algorithms by calling the two functions.

## ath9k_htc driver

[ath9k_htc](https://wireless.wiki.kernel.org/en/users/drivers/ath9k_htcl) driver is a wireless driver for IEEE 802.11n devices with USB port.
It can support AR9271 USB devices.
Atheros provides an open source version of the firmware, i.e., [open-ath9k-htc-firmware](https://github.com/qca/open-ath9k-htc-firmware).

This document is a simple analysis about the RX pipeline of ath9k_htc.

ath9k_htc is based on other ath drivers, you can find the drivers and all relations with

@example 
    $lsmod | grep ath
    ath9k_htc              77824  0
    ath9k_common           36864  1 ath9k_htc
    ath9k_hw              483328  2 ath9k_htc,ath9k_common
    ath                    36864  3 ath9k_htc,ath9k_common,ath9k_hw
    mac80211             1024000  2 ath9k_htc,iwlmvm
    cfg80211              888832  6 ath9k_htc,ath9k_common,iwlmvm,ath,iwlwifi,mac80211
@end example

The description of all drivers can found in [Atheros Linux wireless drivers](http://linuxwireless.sipsolutions.net/en/users/Drivers/Atheros/__v22.html).

In Ubuntu, "apt" can be used to download the Linux kernel source code with the current kernel version  by using

`apt source linux-image-unsigned-$(uname -r)`

After changing into the kernel file, the source code of ath9k_htc is in 
    
`/driver/net/wireless/ath/ath9k/`

If the downloaded driver is mismatched with the Linux kernel version, the "insmod" command will report an error with invalid module format.

All htc related codes are listed in the following files.

@example
    htc_drv_init.c
    htc_drv_main.c
    htc_drv_txrx.c
    htc_drv_beacon.c  
    htc_drv_gpio.c    
    htc_hst.c
    hif_usb.c
    htc_drv_debug.c
@end example

In particular, "htc_drv_main.c" is the entry point of HTC driver with "module_init" and "module_exit".
Other files related to receive data is "htc_drv_txrx.c", "htc_hst.c", and "hif_usb.c"

AR9271 chips connect to the device with USB port, so it uses USB subsystem in Linux kernel to transmit and receive data.
As a USB device, it uses USB Request Block (URB) to receive data.

All functions about URB reception are in "hif_usb.c" file.
In specific, the following functions is the main process for the data reception.

@example
    hif_usb.c
        |
        |---ath9k_hif_usb_alloc_rx_urbs(struct hif_device_usb *hif_dev)
            |
            |---usb_fill_bulk_urb(urb, hif_dev->udev,
            |             usb_rcvbulkpipe(hif_dev->udev,
            |                     USB_WLAN_RX_PIPE),
            |             skb->data, MAX_RX_BUF_SIZE,
            |             ath9k_hif_usb_rx_cb, rx_buf);
            |
            |---usb_anchor_urb(urb, &hif_dev->rx_submitted);
            |
            |---ret = usb_submit_urb(urb, GFP_KERNEL);
            |
        |---ath9k_hif_usb_rx_cb(struct urb *urb)
        |---ath9k_hif_usb_rx_stream(struct hif_device_usb *hif_dev, struct sk_buff *skb)

    htc_hst.c
        |
        |---ath9k_htc_rx_msg(struct htc_target *htc_handle, 
        |                       struct sk_buff *skb, 
        |                       u32 len, u8 pipe_id)

    htc_drv_txrx.c
        |
        |---ath9k_htc_rxep(void *drv_priv, struct sk_buff *skb, htc_endpoint_id ep_id)
            |
            |---tasklet_schedule(&priv->rx_tasklet)
            |---ath9k_rx_tasklet(struct tasklet_struct *t)
@end example

In `ath9k_hif_usb_alloc_rx_urbs`, it allocates a usb bulk URB for signal reception, the callback function is `ath9k_hif_usb_rx_cb`. 
It will submit the URB to the USB core and call the callback function.
If you are new to Linux USB, for a detailed description about [usb_fill_bulk_urb](https://manpages.debian.org/testing/linux-manual-4.8/usb_fill_bulk_urb.9.en.html) and [usb_submit_urb](https://manpages.debian.org/testing/linux-manual-4.9/usb_submit_urb.9), please refer to [Linux USB API](https://www.kernel.org/doc/html/v4.15/driver-api/usb/index.html).

In `ath9k_hif_usb_rx_cb`, it will process skb, if it has new data, the data will pass to `ath9k_hif_usb_rx_stream`.
After that, it will call itself by resubmitting the URB. 
The resubmit ensure that USB core will repeatedly poll the buffer to receive data.

In `ath9k_hif_usb_rx_stream`, it will packetize the data in buffer. Each packet will push into `ath9k_htc_rx_msg` in `htc_hst.c` file.

`ath9k_htc_rx_msg` will handle different types of packets.
For a data packet, it will call `endpoint->ep_callbacks.rx`, which is also a callback function. 
The callback handler is defined in `htc_drv_txrx.c`, which is named as `ath9k_htc_rxep`.

Finally, `ath9k_htc_rxep` will check that whether the buffer is valid, and call `tasklet_schedule`.
[Tasklet](http://books.gigatux.nl/mirror/kerneldevelopment/0672327201/ch07lev1sec3.html) is an interrupt in Linux Kernel.
In this tasklet, ath9k_htc defines a callback for reception named as `ath9k_rx_tasklet`, which is the final process of signal reception. 
The data will pass to mac80211 subsystem by calling [ieee80211_rx](https://elixir.bootlin.com/linux/latest/ident/ieee80211_rx).

### Customized ath9k

ath9k is a kernel driver that works as a kernel module for Atheros wireless IEEE802.11n NICs.
Compilation of the kernel module is the prerequisite to investigate the rate adaptation algorithm.
However, the driver (and the kernel module) highly depends on the version of the kernel, so it is difficult to compile and install even a simple driver.

This subsection introduces a basic and simple method to build it on the newest Ubuntu 22.04.

First, change to the directory of ath driver, which is in the `./drivers/net/wireless/ath/` if the working directory is the root of the kernel source code.
Note that ath9k driver is based on the ath module, so we better compile is at the same time.

In the directory, build the driver for the current kernel with

`make -C /lib/modules/`uname -r`/build M=$PWD`  

where `$PWD` means print work directory.

Now, the compilation is complete, we can load the modules with 

@example
    insmod ath.ko
    insmod ./ath9k_common.ko
    insmod ./ath9k_hw.ko
    insmod ./ath9k.ko
@end example
 
Use `sudo modprobe -r <MODULE_NAME>` to remove the old versions of them.
