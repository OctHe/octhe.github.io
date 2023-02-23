
---
layout: post
title: "Linuxx Desktop Subsystem"
usemathjax: true
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
