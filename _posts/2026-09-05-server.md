---
title: Server
layout: default
---

# Server

## Termux

Termux is an open-source Linux emulator in android.
It uses pkg as the package manager.
By downloading packages, Termux supports network servers.

### ssh

ssh in Termux uses 8022 port by default.
To connect it, use 
`ssh -p 8022 <username>@@<IP>`

## Network File System (NFS)

Network file system (nfs) is a protocol that supports share files in network.

### NFS Server

Downlaod nfs server with apt

`sudo apt install nfs-kernel-server`

First, add the follow codes in the `/etc/exports`.
The shared disk can be mounted by other systems in network.

`/mnt/share 192.168.3.0/24(rw,sync,no_subtree_check)`

In addtion, if another disk is used, add the disk into `/etc/fstab`, so that it can be auto-mounted.

### NFS client

Download the nfs client with dnf

`sudo dnf install nfs-utils`

Add the file system into fstab

`<file system> <dir> <type> <options> <dump> <pass>`

Mount the nfs directory

`mkdir share`
`mount -v -t nfs <remote dir> share`
