
This is the source code of OctHe's personal blog.
The blog can access with [https://octhe.github.io/](https://octhe.github.io/).

This blog is based on [github page](https://pages.github.com/) and [texinfo](https://www.gnu.org/software/texinfo/).

The following command can install the prerequisites

    sudo apt install makeinfo

in Debian or 

    sudo dnf install texinfo

in Fedora.

The blog can be built from the texinfo source to html.

    makeinfo --html --no-split --output=. cottage.texi

