## Package Manager

- apt
- zypper
- dnf
- pacman
- Flatpak
- AppImage

### openSUSE

openSUSE uses `zypper` as the default package manager.
It is based on RPM, which is similar to Fedora.
Besides installation of the built programs, zypper also provides an option for the source code.
As an example, the download and build process for the `hostapd` is

    sudo zypper source-install hostapd
    # or
    #   sudo zypper si hostapd
    cd /usr/src/packages/       # cd to the default directory for source code
    ls SOURCES
    ls SPECS
    sudo zypper in rpmbuild     # The build tool of the download source code
    sudo rpmbuild -ba SPECS/hostapd.spec
                                # -ba means to perform a full build. 
    ls RPMS/x86_64              # The built program in the x86 architecture

    sudo rpmbuild -ba --noclean SPECS/hostapd.spec
                                # It does not remove the extracted source code
    ls BUILD                    # The extracted source code

### Flatpak

Flatpak is a cross-platform package manager that run applications in a sandbox.
It can download apps from FlatHub, which is the official repository of Flatpak.

    flatpak install <package name/ID>

Different from apt/zypper/dnf/pacman, you cannot run the applications from flatpak directly.

    flatpak run <package name/ID>

## Shell

GL give common alias about ls and git for both bash and zsh.

### Bash

Bash is the default shell for most Linux distribution.
GL includes the follow additional configurations for bash
- Prompt 
    - [bash-git-prompt](https://github.com/magicmonty/bash-git-prompt)
- History: Default *<ctrl-r>* for interactive search history
- Keybinding: Built-in `bind`
- Directory
    - fold_pwd
    - [rupa/z](https://github.com/rupa/z)
- Completion
    - Bash-completion

### zsh

zsh is not the default shell in most Linux distribution.
In Debian, the follow command can be used to install zsh

    sudo apt install zsh

In openSUSE, it uses `zypper` as the manager, so 

    sudo zypper install zsh

It support (or will support) the follow features, which are under-development.

- Plugin manager
    - [zinit](https://github.com/zdharma-continuum/zinit)
- Prompt
    - [agnoster](https://github.com/agnoster/agnoster-zsh-theme)
- History: Default *<ctrl-r>* for interactive search history
- Keybinding: Built-in `bindkey`
- Directory
    - [zsh-z](https://github.com/agkozak/zsh-z): Better z completion for zsh
- Completion
    - [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)
    - [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting.git)

### Swap keys

Usually, if you wants to swap Caplock and left Ctrl since the latter is more useful, some desktop provides an GUI tool such as gnome-tweak in GNOME or the setting in KDE that support this function.
For the desktops that do not have an GUI tool, a shell method is to use xmodmap and the GUI helper xkeycaps.
After change the keys in xkeycaps, write out the configuration file of xmodmap in the HOME diretory, and add the follows in the shell

    xmodmap ~/.xmodmap-`uname -n`

xmodmap will trigger the key swap when open a terminal.

## Editor

Graphicless focuses on a terminal-based coding environment.
It is benefited by the powerful plugin ecosystem of vim and integrates open-source language servers, compilers, debuggers and other useful tools.
It also contains the configuration of other editors, such as emacs (in the feature) and micro editor.

### Neovim

In stall neovim by using the package manager, for example

    sudo apt install neovim

It contains follow modules, in which contains multiple plugins with Neovim-specific lua plugins.

- Plugin manager
    - Lazy
- View
- Text
- Search
- Lint
    - [nvim-treesitter](https://github.com/nvim-treesitter/nvim-treesitter)
    - [nvim-lspconfig](https://github.com/neovim/nvim-lspconfig)
- Completion
- Runner
- Version control system

### Vim

- Plugin manager
    - [vim-plug](https://github.com/junegunn/vim-plug)
    - dein
- View
    - Built-in `:colorscheme desert`
    - [vim-airline](https://github.com/vim-airline/vim-airline)
- Text
    - [vim-commentary](https://github.com/tpope/vim-commentary)
    - [vim-surround](https://github.com/tpope/vim-surround)
    - [tagbar](https://github.com/preservim/tagbar)
- Search
    - [NERDTree](https://github.com/preservim/nerdtree)
    - [fzf](https://github.com/junegunn/fzf)
- Lint: Require linters or LSPs
    - [tagbar](https://github.com/preservim/tagbar)
    - [neomake](https://github.com/neomake/neomake)
    - [ALE](https://github.com/dense-analysis/ale)
    - Format: Require `astyle` or LSPs
        - Built-in `gg=G` and `gq`
        - [vim-autoformat](https://github.com/vim-autoformat/vim-autoformat)
- Completion
    - [auto-pairs](https://github.com/LunarWatcher/auto-pairs)
    - YouCompleteMe
    - [UltiSnip](https://github.com/SirVer/ultisnips): Snip engine
    - [vim-snippet](https://github.com/honza/vim-snippets): Database
- Runner
    - Built-in `:make`
    - [vimspector](https://github.com/puremourning/vimspector)
    - [markdown-preview.nvim](https://github.com/iamcco/markdown-preview.nvim)
- VCS

YouCompleteMe, and UltiSnip require python3 support in vim.
In addition, YouCompleteMe also relies on python3 packet.
It can be installed by

    sudo apt install python3-dev clangd make    # clangd is the backend of YcuCompleteMe
    sudo pip3 install pyflacks                  # If pip3 is not installed, install it with sudo apt install python3-pip

clangd requires [bear](https://github.com/rizsotto/Bear) to automatically generate `complie_commands.json` file for make-based projects.
To install it

    sudo apt install bear

### Emacs

Emacs default uses GUI.
To open it in shell, use
    emacs -nw

### Micro Editor

Micro is a easily used terminal editor with intuitive key bindings as well as modern build-in features, including command line, auto-pair and linter.

Install it in Debian (Test on Debian 12)

    sudo apt install micro

A universal installation method for most Linux distributions

    mkdir ~/bin
	cd ~/bin
	curl https://getmic.ro | bash

The configuration files are in `~/.config/micro/*`

The recommended plugins are follows
- linter (default installation)
- filemanager
- wc
- quickfix

The linter requires out-of-box support for each language (see, `help linter` in the command line mode).
Enter the command line mode by typing <C-e>, run

	plugin install filemanager wc quickfix

### Backend

The backend for lint, completion, compiling, and debug.

- Linter
    - Python
        - pyflacks
- Compiler/Interpreter
    - make
        - bear
    - cmake
    - c/c++
        - gcc
        - clang
    - Python
        - cpython
        - pypy
    - Tex
        - texinfo
        - pdftex
        - xetex
        - bibtex
- Debugger
    - gdb
    - ddd
    - lldb
- Language server
    - C/C++
        - clangd
        - ccls
    - Python
    - Bash
        - shellcheck
- Version control
    - git

## Advanced Setting

### gnome-tweaks

gnome-tweaks gives advanced settings for gnome.
It support swap of Caplock and Ctrl.
Download it in Debian with

    sudo apt install gnome-tweaks

This swap is only useful in gnome-based GUI.
Windows has a similar tool that named as [PowerToys](https://github.com/microsoft/PowerToys).


## Terminal emulator

- terminator

### Terminator

Therminal emulator is the prerequisite of shell.
An open-source terminal emulator is *terminator*.
Install it with

    sudo apt install terminator

The terminal emulator provides color and font support for shell.
For example, the [agnoster theme](https://github.com/agnoster/agnoster-zsh-theme) of zsh requires the powerline font.
To install it, run

    sudo apt install fonts-powerline

And then set the fonts in the terminator.

