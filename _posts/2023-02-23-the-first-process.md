---
layout: post
title: "The First Process"
usemathjax: true
---

# The Initalization System

In Linux, the first process is usually *init* or *systemd*.
They both work to manage the initialization of the system before the login shell.
The PID of them is 1, which means the first process after the kernel.

# System V and init

init is the initial script in System V project.
In a early version of a Linux distribution, it usually uses init as the initalization system.
It finds the configuration file in `/etc/inittab` and runs the scripts in `/etc/rc` according to the runlevel.
Different runlevels means different modes of init.
It will run the corresponding runlevel scripts in `/etc/rcN.d/`, where N means the runlevel.
The value of N is from 0 to 6.
All scripts under these directories are a symbolic link to a file in `etc/init.d/`.
The file started with "S" means "start it", while "K" means "kill it".
The feature of init is run all script in a fixed order one-by-one, so it is easy to find the error, while costs lots of time.

# systemd

In contrast, systemd concurrently runs all scripts.
Ubuntu and OpenSUSE have been support systemd and their would have other distributions.
In a system that initialized with systemd.
The init file, which is `/usr/sbin/init` will be a symbolic to the `/lib/systemd/systemd`.
The default configuration file of systemd is `/usr/lib/systemd/system/default.target`, which is also a symbolic link to `/usr/lib/systemd/system/graphical.target`.
You can find the default target with

    systemctl get-default

