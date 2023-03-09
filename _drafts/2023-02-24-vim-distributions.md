---
layout: post
title: "Vim Distributions: Bring Vim to Newbies"
---

Vim is one of the most famous editors in the world with powerful scalability.
However, Vim is extremely unfriendly for newbies, especially for those who uses graphical user interfaces (GUI).
NeoVim is a new version of Vim.
It is community-driven and deprecates lots of old-fashions in Vim.

Due to the extensibility of Vim and Neovim, lots of developers contribute thousands of plugins for them.
So many plugins give both Vim and Neovim unlimited possibilities, but they also dazzle newbies who want to try these famous editors and don't know how to select a suitable configuration that meet their requirements.

To solve this problem, recently, multiple developer and open source projects provides community-driven out-of-box configurations for Vim.
The relationship between these projects and Vim (or Neovim) is like the relationship between a Linux distribution and the kernel.
Therefore, I call these projects as the Vim distribution, which means they use Vim (or Neovim) as the "editor kernel".
The development of Vim distribution is more easier then before, thanks to the language server protocol (LSP).
The follows are some Vim distributions.

# SpaceVim

[SpaceVim](https://spacevim.org) is inspired by spacemacs.
It is community-driven and provides layers to encapsulated the low-level configuration of Vim/Neovim.
Each layer in SpaceVim indicates a collection of plugins that provides a similar function.
Users can choose different plugins for the layer.
However, although SpaceVim change the <leader> to <Space>, it lacks a local easy-to-use document about all the keybindings (like vim).
SpaceVim provides a new configuration interface that uses `.toml` in the directory `~/.Spacevim.d/`.
SpaceVim is only distribution that can support both Vim and NeoVim.

# LunarVim

[LunarVim](https://www.lunarvim.org/), or lvim in short, is another IDE distribution that only supports Neovim.
It needs npm, node, and cargo as the prerequisites.
LunarVim is also community-driven and provides good support for multiple languages.
It provides a shortcut named as lvim in `.local/bin/`.

# Doom-nvim

{Doom-nvim}(doom-neovim/doom-nvim) is also inspired by a emacs-based project.
It also proposes modules that includes plugins and keybindings.
The prerequisites includes npm and tree-sitter, which are both based on javascript.
If the Linux does not contains the prerequisites, the installation would have some problem.

