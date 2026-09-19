## Knowledge Base

This is the source code of a personal knowledge base.
The blog can access with [https://octhe.github.io/](https://octhe.github.io/).

## Installation

This blog is based on [github page](https://pages.github.com/) and [jekyll](https://jekyllrb.com/).
It requires Ruby, RubyGems, and bundler.

The following command can install the prerequisites

    sudo apt install ruby-full build-essential zlib1g-dev

Sometimes gem is hard to access in some regions.
It is recommended to change the source link by

    sudo gem source --add <https://mirror.com> --remove https://rubygems.org/

where `<https://mirror.com>` is the host of a mirror.
As an example that uses [TUNA](https://mirrors.tuna.tsinghua.edu.cn/help/rubygems/) as the source:

    sudo gem sources --add https://mirrors.tuna.tsinghua.edu.cn/rubygems/ --remove https://rubygems.org/
    sudo gem sources -l # Check whether the mirror is applied

Also, below command replaces the soruce of bundler

    bundler config set --global mirror.https://rubygems.org https://mirrors.tuna.tsinghua.edu.cn/rubygems

Finally, the last command installs jekyll and bundler in system directory.

    sudo gem install jekyll bundler

The packages will be installed under the user directory without *sudo*.
The directory of the packages can be found by use

    gem env | grep DIRECTORY

## Jekyll Themes

The blog uses the default [minima](https://github.com/jekyll/minima) theme.

This [GitHub page](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll) give details about how to create the website.


## Usage

The blog can be used from source with `bundler`

    # In Debian, set the bundler directory in home, or bundler may not have the permission
    export GEM_HOME="$HOME/.gem"
    bundler install
    bundler exec jekyll serve --livereload

Please ensure the packages installed by gem are in the *$PATH* variable.

## Upgrade

Bundler and Jekyll requires the correct version of Ruby.
If Ruby is updated within the system upgrade, bundler and Jekyll both need to reinstall from gem.
For example, when Ubuntu 22.04 is upgraded from Ubuntu 20.04, Ruby 3.0 would be installed, but bundler and Jekyll both requires Ruby 2.7.
So the two programs would not processing correctly anymore.

To fix this problem, the direct approach is to reinstall bundler with gem.

    sudo gem install bundler

