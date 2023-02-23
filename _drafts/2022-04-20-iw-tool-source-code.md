---
layout: post
title: "Source Code of iw"
usemathjax: true
---

# Introduction

iw is a configuration tool for mac80211 in Linux kernel with netlink


# Code tree of main.c

    main
        version
        nl80211_init
            nl_socket_alloc
            nl_socket_free
            nl_socket_set_buffer_size
        usage
            __usage_cmd
            usage_options
        __handle_cmd
            error_handler
            finish_handler
            ack_handler
            register_handler
            valid_handler
            phy_lookup
        phy_lookup
        nl80211_cleanup

    TOPLEVEL
        print_help

    handle_cmd
        __handle_cmd

    usage_cmd
        __usage_cmd
