---
layout: post
title: "Linuxx Desktop Subsystem"
---

Linux desktop subsystem is one of the most complicated subsystem.
This post gives part of the desktops and their relations.
x11 and Wayland are two communication protocol that relates a display server and the client.
Most of the graphic desktop and libraries are based on them.
While Wayland is newer than x11, most applications cannot be used on it.

    x11/Wayland
    |-- GTK
        |-- GNOME
            |-- GNOME shell
            |-- Other applications
        |-- xfce
            |-- Window manager
            |-- Desktop manager
            |-- Session manger
            |-- Panel
            |-- Other applications
    |-- QT
        |-- KDE Plasma
        |-- Deepin
    |-- Tilling window manager
        |-- i3 (x11)
        |-- xmonad (x11)
        |-- sway (wayland)

## Virtual console and GUI

Linux desktop usually has two types of interfaces.
One of them is virtual console and the other is GUI.
In Ubuntu, type `<ctrl>+<alt>+F{1-6}` can change between virtual console and GUI.
For example, `<ctrl>+<alt>+F2` can change to the GUI, and `<ctrl>+<alt>+F3` is the virtual console.
Virtual console is the default interface from Linux kernel and does not support Unicode.
By default, the startup program, i.e., systemd in Ubuntu, will initialize the GUI desktop.
However, if someone want to open it in manual, he can type `sudo startx` to open the GUI in the virtual console.
