## Knowledge Base

This is the source code of a personal knowledge base.
The blog can access with [https://octhe.github.io/](https://octhe.github.io/).

## Installation

This blog is based on [github page](https://pages.github.com/) and [jekyll](https://jekyllrb.com/).
It requires Ruby, RubyGems, and bundler.

The following command can install the prerequisites

    sudo apt install ruby-full build-essential zlib1g-dev

Sometimes gem is hard to access in some regions.
It is recommended to change the source link.
As an example, 

    sudo gem source --add <https://mirror.com> --remove https://rubygems.org/

where `<https://mirror.com>` is the host of a mirror.
The follow installs jekyll and bundler

    sudo gem install jekyll bundler

## Jekyll Themes

The blog uses the default [minima](https://just-the-docs.com/) theme.

This [GitHub page](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll) give a detailed reference about how to create the website.


## Usage

The blog can be used from source with `bundle`

    bundle exec ~/bin/jekyll serve --livereload

## Upgrade

Bundler and Jekyll requires the correct version of Ruby.
If Ruby is updated within the system upgrade, bundler and Jekyll both need to reinstall from gem.
For example, when Ubuntu 22.04 is upgraded from Ubuntu 20.04, Ruby 3.0 would be installed, but bundler and Jekyll both requires Ruby 2.7.
So the two programs would not processing correctly anymore.

To fix this problem, the direct approach is to reinstall bundle with gem.

    sudo gem install bundler

