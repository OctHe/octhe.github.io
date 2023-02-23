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

    sudo gem source --add <https://mirror.com> --remove https://rubygems.org/

where `<https://mirror.com>` is the host of a mirror.
The follow installs jekyll and bundler

    sudo gem install jekyll bundler

## Jekyll Themes

The blog is based on [mediator](https://jekyllthemes.io/theme/mediator) theme.
Download the theme and copy it into my own git directory by

    git clone https://github.com/dirkfabisch/mediator

## Usage

The blog can be used from source with `bundle`

    bundle exec jekyll server

## Upgrade

Bundler and Jekyll requires the correct version of Ruby.
If Ruby is updated within the system upgrade, bundler and Jekyll both need to reinstall from gem.
For example, when Ubuntu 22.04 is upgraded from Ubuntu 20.04, Ruby 3.0 would be installed, but bundler and Jekyll both requires Ruby 2.7.
So the two programs would not processing correctly anymore.

To fix this problem, the direct approach is to reinstall bundle with gem.

    sudo gem install bundler

Then, bundle can install all required packages by `bundle install` except *webrick* because webrick is no longer in the standard library of bundle.
However, I have a network issue when run `bundle add webrick`.
It shows "Could not reach host index.rubygems.org. Check your network connection and try again."
The straightforward method is to set a mirror by

    bundle config mirror.https://rubygems.org <https://mirror.com>

where `<https://mirror.com>` is the host of a mirror.
However, it still does not work for me, while the mirror still can be reached by ping, curl, and the browser with ipv4.
So the problem is strange, but I fix it with root command.

    sudo bundle config mirror.https://rubygems.org <https://mirror.com>
    sudo bundle install
    sudo bundle add webrick

I also close ipv6 because it seems that bundle only supports ipv4, but it is not sure if this process works.
