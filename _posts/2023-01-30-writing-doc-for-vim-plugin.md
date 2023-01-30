---
layout: post
title: "Help Doc for Vim Plugin"
---

# Vim and Plugin 

[Vim](https://www.vim.org/) is a well-known configurable editor that works well on terminal.
Plugin is an important part to support the extensibility of Vim.
Plugin can be found in the ./plugin directory and the help document is under the ./docs directory. 
This blog is an example that writes the help doc of Vim Plugin. 

# Title and Tag

The first line of the help document is the title and a simple introduction to the plugin.
For example, `*example.txt*` is the title of the help document.
The `*` symbol around the title means a tag.
With this symbol, the user can type `:help example` to show this documentation.

# Section and Content

`=` and `-` are two symbols to indicate the sections and subsections of the help document.
The first section is usually the content, that can be writing as

    ==============================================================================
    CONTENTS                                                       *clam-contents*

        1. Usage ........................... |ExampleUsage|
        2. Mappings ........................ |ExampleMappings|

    ==============================================================================

Similarly, the `*` symbol is a tag.
In addition, the word in `|` means a link that can go to the corresponding tag by typing `<c-]>` in normal mode.

# Example

This is a basic example of the help document.

    *example.txt*  Introduction to the plugin
    Author: 
    License:

    ==============================================================================
    CONTENTS                                                       *clam-contents*

        1. Usage ........................... |ExampleUsage|
        2. Mappings ........................ |ExampleMappings|
        3. Configuration ................... |ExampleConfiguration|
        4. Contributing .................... |ExampleContributing|
        5. Changelog ....................... |ExampleChangelog|

    ==============================================================================
    1. Usage                                                        *ExampleUsage*

    That's the introduction about the usage of the plugin.

    ==============================================================================
    2. Mappings                                                  *ExampleMappings*

    That section contains the mapping of the plugin.

    ==============================================================================
