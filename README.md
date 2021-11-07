# bundle-dylibs
Command line tool to make standalone apps out of otherwise dependent apps on macOS, by packing the non-standard dynamic libraries into the application bundle.

## Warning
No liability! Use this tool at your own risk!
It is allways recommended to make a copy of the app you want to work on, and apply the changes to the copy first.

## Prerequisite
- Xcode Command Line Tools (CLT) must be installed before running this tool.

## About
This tool can be used to make a standalone, hence transferable -between different macOS systems- app out of otherwise dependent app.
The dependency mentioned here comes from the extra dynamic libraries an app depends on, which are not present by default on macOS. eg. `libgnutls`.
You can have such a situation when you build an app from source which depends on extra dynamic libraries, eg. the [GNU Emacs](https://www.gnu.org/software/emacs/).

## Background
Prevoiusly I was using David Caldwell's [Emacs For Mac OS X](https://emacsformacosx.com/), which did provide me with a good service for a long time, but unfortunately, on newer macOS versions due to the enhanced security features, like app-based access control to folders like `Desktop`, `Documents`, etc. it become a bit confusing for the unaware user to grant access to `ruby` after a fresh install/update.
That's because David's approach was to distribute a universal app, which comes with multiple binaries pre-built for different versions of macOS, and a `ruby` launcher script.
This `ruby` launcher script is the culprit, why we need to grant access to the `ruby` interpreter and not the `Emacs.app` on recent macOS systems. With this, Emacs is also slower to launch compared to a pure binary.
Don't get me wrong, it's great what David has had done. It's just that I personally don't need multiple binaries bundled, and a slower launching mechanism, and due to the fact it's really easy to build Emacs from source, this tool was born.

## Example usage
### Build GNU Emacs from source and make it a standalone app
```sh
brew install gnutls
brew install jansson
brew install xz

curl -s -O https://ftp.gnu.org/gnu/emacs/emacs-27.2.tar.xz
tar xf emacs-27.2.tar.xz
cd emacs-27.2
./configure
make
make install

cp -a nextstep/Emacs.app ~/Applications
bundle-dylibs ~/Applications/Emacs.app
```

This last one is for staging. After checking and positive testing, you can move the `Emacs.app` to `/Applications` and use it normally.
Now it should be standalone and ready to be distributed to other macOS systems.

## Inspired by
- https://github.com/renard/emacs-build-macosx/blob/master/build-emacs
- https://github.com/trojanfoe/xcodedevtools/blob/master/copy_dylibs.py
