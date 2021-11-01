---
layout: post
title: "Source Code Analysis of ath9k_htc Driver"
usemathjax: true
---

# Introduction

[ath9k_htc](https://wireless.wiki.kernel.org/en/users/drivers/ath9k_htc) driver is a wireless driver for IEEE 802.11n devices with USB port.
It can support AR9271 USB devices.
Atheros provides a open source version of the firmware, i.e., [open-ath9k-htc-firmware](https://github.com/qca/open-ath9k-htc-firmware).

This document is a simple analysis about the RX pipeline of ath9k_htc.

ath9k_htc is based on other ath drivers, you can find the drivers and all relations with
   
    $lsmod | grep ath
    ath9k_htc              77824  0
    ath9k_common           36864  1 ath9k_htc
    ath9k_hw              483328  2 ath9k_htc,ath9k_common
    ath                    36864  3 ath9k_htc,ath9k_common,ath9k_hw
    mac80211             1024000  2 ath9k_htc,iwlmvm
    cfg80211              888832  6 ath9k_htc,ath9k_common,iwlmvm,ath,iwlwifi,mac80211

That means ath9k_htc is based on other ath drivers.
If someone wants to install ath9k_htc driver, he must install ath9k, ath9k_hw, ath9k_common first.

The description of all drivers can found in [Atheros Linux wireless drivers](http://linuxwireless.sipsolutions.net/en/users/Drivers/Atheros/__v22.html).

# Download

The most importance is to download the correct version of ath9k_htc driver. 
If the downloaded driver is mismatched with the Linux kernel version, the *insmod* command will report an error with invalid module format.

In my way, *apt* can be used to download the Linux kernel source code with the current kernel version  by using

    apt source linux-image-unsigned-$(uname -r)

After changing into the kernel file, the source code of ath9k_htc is in 
    
    /driver/net/wireless/ath/ath9k/

# Source Code

All htc related codes are listed in the following files.

    htc_drv_init.c
    htc_drv_main.c
    htc_drv_txrx.c
    htc_drv_beacon.c  
    htc_drv_gpio.c    
    htc_hst.c
    hif_usb.c
    htc_drv_debug.c

In particular, *htc_drv_main.c* is the entry point of HTC driver with *module_init* and *module_exit*.
Other files related to receive data is *htc_drv_txrx.c*, *htc_hst.c*, and *hif_usb.c*

# Reception Pipeline

AR9271 chips connect to the device with USB port, so it uses USB subsystem in Linux kernel to transmit and receive data.
As a USB device, it uses USB Request Block (URB) to receive data.

All functions about URB reception are in *hif_usb.c* file.
In specific, the following functions is the main process for the data reception.

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

In *ath9k_hif_usb_alloc_rx_urbs*, it allocates a usb bulk URB for signal reception, the callback function is *ath9k_hif_usb_rx_cb*. 
It will submit the URB to the USB core and call the callback function.
If you are new to Linux USB, for a detailed description about [*usb_fill_bulk_urb*](https://manpages.debian.org/testing/linux-manual-4.8/usb_fill_bulk_urb.9.en.html) and [*usb_submit_urb*](https://manpages.debian.org/testing/linux-manual-4.9/usb_submit_urb.9), please refer to [Linux USB API](https://www.kernel.org/doc/html/v4.15/driver-api/usb/index.html).

In *ath9k_hif_usb_rx_cb*, it will process skb, if it has new data, the data will pass to *ath9k_hif_usb_rx_stream*. After that, it will call itself by resubmitting the URB. 
The resubmit ensure that USB core will repeatedly poll the buffer to receive data.

In *ath9k_hif_usb_rx_stream*, it will packetize the data in buffer. Each packet will push into *ath9k_htc_rx_msg* in *htc_hst.c* file.

*ath9k_htc_rx_msg* will handle different types of packets.
For a data packet, it will call *endpoint->ep_callbacks.rx*, which is also a callback function. 
The callback handler is defined in *htc_drv_txrx.c*, which is named as *ath9k_htc_rxep*.

Finally, *ath9k_htc_rxep* will check that whether the buffer is valid, and call *tasklet_schedule*.
[Tasklet](http://books.gigatux.nl/mirror/kerneldevelopment/0672327201/ch07lev1sec3.html) is a interrupt in Linux Kernel.
In this tasklet, ath9k_htc defines a callback for reception named as *ath9k_rx_tasklet*, which is the final process of signal reception. 
The data will pass to mac80211 subsystem by calling [*ieee80211_rx*](https://elixir.bootlin.com/linux/latest/ident/ieee80211_rx).
