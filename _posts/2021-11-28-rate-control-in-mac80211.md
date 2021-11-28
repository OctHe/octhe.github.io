---
layout: post
title: "Source Code of Rate Control in mac80211"
usemathjax: true
---

# Introduction

Rate control (or rate adaptation) is an important algorithm for wireless devices to select the optimal bit rate under different wireless channel conditions.
In mac80211 subsystem in Linux kernel, rate control is part of the subsystem, and it can be self-designed by each wireless driver.
For example, ath9k driver uses the default rate control (i.e., [minstrel](https://wireless.wiki.kernel.org/en/developers/documentation/mac80211/ratecontrol/minstrel)), while iwlwifi uses a custom rate control named ["iwl-agn-rs"](https://wiki.gentoo.org/wiki/Iwlwifi).

# Interface

mac80211 provides a struct [*rate_control_ops*](https://docs.huihoo.com/doxygen/linux/kernel/3.7/structrate__control__ops.html) with multiple callbacks.
To implement a new rate control algorithm, someone must define these callbacks in *rate_control_ops*.
For example, minstrel includes two types of algorithm, one is in the file *rc80211_minstrel.c* (mac80211_minstrel) and the other is in the file *rc80211_minstrel_ht.c* (mac80211_minstrel_ht).

For iwlwifi driver, *rate_control_ops* is defined in dvm/mac80211.c (rs_ops) and mvm.mac80211.c (rs_mvm_ops_drv), depend on different types of drivers.

# Rate Control Functions

*rate.c* defines all rate control functions for different drivers.
Ideally, the developer does not need to change the function in *rate.c*.

Two basic functions in this file is *ieee80211_rate_control_register* and *ieee80211_rate_control_unregister*.
The driver loads different rate control algorithms by calling the two functions.
