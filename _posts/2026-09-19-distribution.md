---
title: Distribution
layout: default
---

# Distribution

Linux contains lots of well-known distributions.
A Linux distribtuion comprises multiple drivers to support different hardware architectures and a collections of softwares in the user sapce.
The follow tables summarize the main different between these awesome distributions and the programs.

| |    Arch   |      Debian      |   Fedora  |   NixOS   |    Void   |
| --- | --- | --- | --- | --- | --- |
|   Init system   |  systemd  |      systemd     |  systemd  |  systemd  |   runit   |
| Package manager |   pacman  |        apt       |    dnf    |    Nix    |    xbps   |
|    C library    |   glibc   |       glibc      |   glibc   |   glibc   |    musl   |
|     Utility     | coreutils |     coreutils    | coreutils | coreutils | coreutils |
|    Installer    |    CLI    | Debian-Installer |  Anaconda | Calamares |    CLI    |

The distributions for embeded devices

| | Openwrt |  Alpine     |
| --- | --- | --- |
|   Init system   |   init  |  OpenRC   |
| Package manager |   opkg  |   apk       |
|     Utility     | Busybox | Busybox     |

## Package Manager

The follow lists common package managers that in different distributions.

- apt
- dnf
- pacman
- zypper
- Flatpak
- AppImage

### dnf

dnf or Dandified YUM is the next-generation of the Fedora package manger yum.
It manages rpm packages.

[COPR](https://copr.fedorainfracloud.org) is an extra repository for Fedora.
NeuroFedora team move the softwares to official Fedora repositories.
Add this repo into the dnf repo list by running
`sudo dnf copr enable neurofedora/neurofedora-extra`

The source files of an rpm package are in the *.src.rpm.

    # Download the src.rpm
    dnf download --source <package name>
    # List the infomation in the *src.rpm
    rpm -ql *.src.rpm
    # Extract the *src.rpm
    rpm2cpio *.src.rpm | cpio -idv
    # Install source code from *src.rpm to home directory
    rpm -i *src.rpm
    # Rebuild the source package if it is patched
    rpmbuild -bb /spec/directory/package.spec

The default directory for the rpm macros is under`/usr/lib/rpm/macros`.

### zypper

openSUSE uses `zypper` as the default package manager.
It is based on RPM, which is similar to Fedora.
zypper uses repository for packages.
`zypper repos` lists repositories.
Search all installed packages in a target repo, use `zypper search -i -r <repo>`.

Besides installation of the built programs, zypper also provides an official repository for the source code.
As an example, the download and build process for the `hostapd` is

    sudo zypper source-install hostapd  # or `sudo zypper si hostapd`
    cd /usr/src/packages/       # cd to the default directory for source code
    ls SOURCES
    ls SPECS
    sudo zypper in rpmbuild     # The build tool of the download source code
    sudo rpmbuild -ba SPECS/hostapd.spec  # -ba means to perform a full build.
    ls RPMS/x86_64              # The built program in the x86 architecture
    sudo rpmbuild -ba --noclean SPECS/hostapd.spec    # It does not remove the extracted source code
    ls BUILD                    # The extracted source code

openSUSE also provides a command-line tool for package download, building and packaging.
The name of the tool is osc, which means openSUSE commander.
To use it, run

`sudo zypper install osc`

### Flatpak

Flatpak is a cross-platform package manager that run applications in a sandbox.
It can download apps from FlatHub, which is the official repository of Flatpak.

`flatpak install <package name/ID>`

Different from apt/zypper/dnf/pacman, you cannot run the applications from flatpak directly.

`flatpak run <package name/ID>`

## Init Process

In Linux, the first process is usually *init* or *systemd*.
They both work to manage the initialization of the system before the login shell.
The PID of them is 1, which means the first process after the kernel.

### System V and init

init is the initial script in System V project.
In a early version of a Linux distribution, it usually uses init as the initalization system.
It finds the configuration file in `/etc/inittab` and runs the scripts in `/etc/rc` according to the runlevel.
Different runlevels means different modes of init.
It will run the corresponding runlevel scripts in `/etc/rcN.d/`, where N means the runlevel.
The value of N is from 0 to 6.
All scripts under these directories are a symbolic link to a file in `etc/init.d/`.
The file started with "S" means "start it", while "K" means "kill it".
The feature of init is run all script in a fixed order one-by-one, so it is easy to find the error, while costs lots of time.

### systemd

In contrast, systemd concurrently runs all scripts.

In a system that initialized with systemd, the init file, which is `/usr/sbin/init` will be a symbolic to the `/lib/systemd/systemd`.
The default configuration file of systemd is `/usr/lib/systemd/system/default.target`, which is also a symbolic link to `/usr/lib/systemd/system/graphical.target`.
You can find the default target with

`systemctl get-default`

## Live CD

This section discribes how to create a custom Live CD in Fedora with `livemedia-creator`.

    # Install mock, which provides an independent compose environment.
    sudo dnf install mock
    # Init mock
    mock -r <live_cd_name> --init
    # Install packages into the composed environment
    mock -r <live_cd_name> --install lorax-lmc-novirt vim-minimal pykickstart livecd-tools
    # chroot into the environment
    mock -r <live_cd_name --shell --enable-network --isolation=simple

After that, the user has been changed into the compose environment, and the packages for composion have been installed.
The next step is get the configuration files for the target ISO.
These files are named as kickstart files, and `.ks` is the file extension.
The kickstart files are in the [fedora kickstarts project](https://pagure.io/fedora-kickstarts.git).
Download the files and move it into the mock environment.

The home directory of <live_cd_name> is under the `/var/lib/mock/<live_cd_name>/root/builddir`.
The project can be directly copied into the builddir of the compose environment.
Another method is to copy the project with the command

`mock -r <live_cd_name> --copyin <project name> /builddir`

The kickstart files in the project are templates, it should be resolved by flattening in the compose environment.

`ksflatten --config <template.ks> -o flat-<template.ks>`

In X64 system, compose the Fedora 42 system lacks `shim-ia32` package.
It should be installed manually.
To do that, add the package name, i.e. `shim-ia32` after the `%packages` in the flatten kickstart file.

Finally, run the livemedia-creator

`livemedia-creator --ks flat-<template>.ks --no-virt --resultdir /var/lmc --project <project_name> --make-iso --volid <volume ID> --iso-only --iso-name <live_cd>.iso --releasever <version> --macboot`

The target ISO is under the `var/lmc`.

The official page is [How to create and use a Live CD](https://fedoraproject.org/wiki/Livemedia-creator-_How_to_create_and_use_a_Live_CD).

## Linux To Go

Linux To Go (LTG) is a method to install Linux in external USB disk.
The main advantage of LTG is to boot your owe system in multiple computers that have same architecture (for example, x86 system).

The LTG is based on Fedora since it has stable version, so it does not need to be updated frequently.
The boot method is UEFI, so allocate a partition with 1 GiB to support UEFI.
In addition, install the bootloader to the USB disk.
After that, the installation process is the same as a normal installation in any disk.

# Multi-OS

Install multiple OSs is simple since lots-of OSs have a GUI installer.
In most time, someone does not need to install the OS in the hardware, if he only want to try it at first.
This section summarizes the content about Live USB and Disk layout.

## Live USB

The first thing of installation OSs is having a Live USB that contains multiple ISOs.
[Ventoy](https://www.ventoy.net/en/index.html) is an open-source tool that can load multiple Live USBs
The USB with Ventoy also can be used for backup files.

The only drawback of ventoy is the grub may have a incorrect boot configuration.
This makes the installed OS can be boot with recovery mode but cannot boot with the normal mode.
To avoid this, just edit the grub (type `e` when boot) and delete the command `rdinit=/vtoy/vtoy`.

## Disk Partition

Most Linux distributions provides a GUI installer, so the installation is quite easy.
The only thing worth noting is carefully about the disk partition without formating the partition and lossing data.
Usually, all the home directories of all OSs can be listed in the same partition with different folders.
An example partition can be as follows

- nvme0n1
    - nvme0n1p1 (512 MB)
        - /boot/efi
    - nvme0n1p2 (16 GB)
        - /swap
    - nvme0n1p3 (500 GB)
        - /home
            - /home/user_ubuntu
            - /home/user_suse
            - /home/user_arch
            - ...
    - nvme0n1p4 (100 GB)
        - /       # root for Ubuntu
    - nvme0n1p5 (100 GB)
        - /       # root for OpenSUSE
    - ...

The most important thing is: DON'T FORMAT the home directory in nvme0n1p3 and the /boot/efi directory in nvme0n1p1 when install a new OS.
This partition can provide the capability to reinstall all OSs without affecting the user data.

## Update grub

After a new installation, it is better to update the grub at any OSs.
The follow command can be used only once at one of the OSs.
In Debian, use

`sudo grub-mkconfig -o /boot/grub/grub.cfg`
`sudo grub-install /nvme0n1`

In OpenSUSE, use

`sudo grub2-mkconfig -o /boot/grub/grub.cfg`
`sudo grub2-install /nvme0n1`

## Reinstallation

Most time there is no need to reinstall my OSs, but sometimes the OS cannot be boot due to strange issues.
In this time, the OS can be reinstalled with the same partition and user name as the old version.
For example, the root of the new ubuntu will be listed in nvme0n1p4, and the /home will be listed in nvme01n1p3.
The user name is 'user_ubuntu', so the configuration of the old OS can be reused for the new OS.
