---
layout: post
title: "Install Multiple OSs"
---

Usually I prefer to install multiple different Linux systems on one machine.
I have tried Ubuntu, OpenSUSE, EndeavourOS as well as windows.
All of them include a GUI installer, making the installing easier.
In most time, someone does not need to install the OS in the hardware, if he only want to try it at first.
He can choose a virtual machine.
But in my opinion, installing OS in real hardware is not too difficult, because of the GUI installer.

This post wants to summarize the content about Live USB and Disk layout.

## Live USB

The first thing of installation OSs is having a Live USB that contains multiple ISOs.
To achieve this, I uses [ventoy](https://www.ventoy.net/en/index.html) in my USB.
Using ventoy is extremely easy.
I just download it, and install it on my USB flash.
Now, my USB can be used for backup other files, and it is a Live ISO for installtion.
After that, I have download all I wanted ISO and copy them into my Ventoy flash.

The only drawback of ventoy is the grub may have a incorrect boot configuration.
This makes the OpenSUSE can be boot with recovery mode but cannot boot with the normal mode.
To avoid this, just edit the grub (type `e` when boot) and delete the command `rdinit=/vtoy/vtoy`.

## Disk Partition

Most Linux distributions provides a GUI installer, so the installation is quite easy.
The only thing worth noting is carefully about the disk partition if you don't want to lost any data.
Usually, I uses a partition for the home directory to avoid restalling the OSs.
All the home directories of all OSs are listed in the same partition with different folders.
My partition is as follows

    nvme0n1
    |-- nvme0n1p1 (512 MB)
        |-- /boot/efi
    |-- nvme0n1p2 (16 GB)
        |-- /swap
    |-- nvme0n1p3 (500 GB)
        |-- /home
            |-- /home/user_ubuntu
            |-- /home/user_suse
            |-- /home/user_arch
            |-- ... 
    |-- nvme0n1p4 (100 GB)
        |-- /       # root for Ubuntu
    |-- nvme0n1p5 (100 GB)
        |-- /       # root for OpenSUSE
    |-- ...

The most importation thing is: DON'T FORMAT the home directory in nvme0n1p3 and the /boot/efi directory in nvme0n1p1 when install a new OS.
The above partition let me reinstall all my systems without affecting the user data.

## Update grub

After a new installation, it is better to update the grub at any OSs.
The follow command can be used only once at one of the OSs.
In Ubuntu, use
    
    sudo grub-mkconfig -o /boot/grub/grub.cfg
    sudo grub-install /nvme0n1

In OpenSUSE, use
    
    sudo grub2-mkconfig -o /boot/grub/grub.cfg
    sudo grub2-install /nvme0n1

## Reinstallation

Most time I don't reinstall my OSs, but sometimes the OS cannot be boot due to strange issues.
In this time, I reinstall the OS with the same partition and user name as the old version.
For example, the root of the new ubuntu will be listed in nvme0n1p4, and the /home will be listed in nvme01n1p3.
The user name is 'user_ubuntu', so I can reuse the configuration of the old OS.
