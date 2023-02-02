## Welcome to Shiyue's GitHub Pages

This is the source code of Shiyue He's blog.
The blog can access with [https://octhe.github.io/](https://octhe.github.io/).

## Installation

This blog is based on [github page](https://pages.github.com/) and [jekyll](https://jekyllrb.com/).
It requires Ruby, RubyGems, and bundler.

The following command can install the prerequisites

    sudo apt install ruby-full build-essential zlib1g-dev

Sometimes gem is hard to access in some regions.
It is recommended to change the source link.
As an example, 

    sudo gem source --add https://gems.ruby-china.com/ --remove https://rubygems.org/

The follow installs jekyll and bundler

    sudo gem install jekyll bundler

## Upgrade

Bundler and Jekyll requires the correct version of Ruby.
If Ruby is updated within the system upgrade, bundler and Jekyll both need to reinstall from gem.
For example, when Ubuntu 22.04 is upgraded from Ubuntu 20.04, Ruby 3.0 would be installed, but bundler and Jekyll both requires Ruby 2.7.
So the two programs would not processing correctly anymore.

To fix this problem, the direct approach is to reinstall them with gem.

## Jekyll Themes

The blog is based on [mediator](https://jekyllthemes.io/theme/mediator) theme.
Download the theme and copy it into my own git directory by

    git clone https://github.com/dirkfabisch/mediator

## Usage

The blog can be used from source with `bundle`

    bundle exec jekyll server


