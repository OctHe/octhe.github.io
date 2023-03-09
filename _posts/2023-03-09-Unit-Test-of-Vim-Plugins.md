---
layout: post
title: "Unit Test for Vim Plugin"
---

Vim is an awesome editor with powerful plugin ecosystems.
Most of the time, the plugin can be directly used by downloading them into the directory.
The plugin manager, which is also a plugin, can add the other plugins in to the runtime directory of vim.

This post is not about how to install and use plugins, but is related to how to write a plugin.
Specifically, test is the basic function if someone wants to write their custom plugin.
Test the source code of a well-known plugin is also a good way to learn the idea of other developers' plugins.

The goal of this post is to give a brief introduction on test Vim plugins like test a command line.

## Environment

The plugin language of writing plugins is Vimscript.
We need at least two vim processes, one for edit, and the other for running test.

## Test Vim Plugin

Although vim is an editor, it provides a non-interactive mode to run Vim scripts without open the window.
We only need to redirect the output of vim to other command with pipeline in shell like this

    vim -u NONE --not-a-term -c "echoconsole \"hello world \"  | q" >> vim2stdout.txt

where `-u`, `-c`, `--not-a-term` are vim options.
The detail can be found in `vim --help`.
`echoconsole` is a vim command can be used to output a message, which is "hello world" in this case, to the terminal, and vim quit.
The output is redirect to a file vim2stdout.txt.
The file can be open with
    
    vim vim2stdout.txt

and the string "hello world" can be found in the file, with some other strings.

In addition, we can write a script to run a test case, and redirect the output to the `awk` tool, such as

    vim -u -NOME --not-a-term -S <run_test_file> <test_case> | awk <pattern>

It need to note that the must are `echoconsole` and `quit` in the <run_test_file>.

## Debug Vim plugin

Debug the unit test of vim plugin can be implemented by the `clientserver` feature of Vim.
There is a good plugin [breakbts](https://github.com/vim-scripts/BreakPts).

