# bundle-dylibs
Command line tool to make standalone apps out of dependent apps on macOS, by packing the extra dynamic libraries into the application bundle.

## Warning
No liability! Use this tool at your own risk!
It is allways recommended to make a copy of the app you want ot work on with this tool first, and apply it to the copy. Not the original!

## Prerequisite
- Xcode Command Line Tools (CLT) must be installed before running this tool.

## About
This tool can be used to make a standalone, hence transferable -between different macOS systems- app out of otherwise dependent app.
The dependency mentioned here comes from the extra dynamic libraries an app depends on, which are not present by default on macOS. eg. `libgnutls`.
You can have such a situation when you build an app from source which depends on extra dynamic libraries, eg. the [GNU Emacs](https://www.gnu.org/software/emacs/).
I use this tool to make a standalone `Emacs.app` out of the one which we get by building it from the official source.

Prevoiusly I was using [David Caldwell's Emacs For Mac OS X](https://emacsformacosx.com/), which did provide me with a good service for a long time, but unfortunately, on newer macOS versions due to the enhanced security features, like app-based access control to folders like `Desktop`, etc. it become a bit of pain.
That's because David's approach was to distribute a more universal build, which comes with multiple binaries pre-built for different versions of macOS and a `ruby` launcher script.
This `ruby` launcher script is the culprit, why we need to grant access to the `ruby` interpreter and not the `Emacs.app` on recent macOS versions, when using `Emacs.app` from Emacs For Mac OS X.
The launcher script makes it also slower to launch the application compared to launching directly a binary.

Due to these reasons, and due to the fact it's really easy to build Emacs from source, this tool was born.

## Example usage
### Build GNU Emacs from source and make it a standalone app
```sh
brew install gnutls
brew install jansson
curl -s -O https://ftp.gnu.org/gnu/emacs/emacs-27.2.tar.gz
# you might want to check the signature of the downloaded tarball before you continue
tar xvzf emacs-27.2.tar.gz
cd emacs-27.2
./configure
make
make install
cp -a nextstep/Emacs.app ~/Applications
bundle-dylibs ~/Applications/Emacs.app
```

After this you can move the `Emacs.app` to `/Applications` and use it normally.
Now it should be standalone and is ready to be distributed to other macOS systems, without those dependent libraries pre-installed to them.

## References
### Inspired by
- https://github.com/renard/emacs-build-macosx/blob/master/build-emacs
- https://github.com/trojanfoe/xcodedevtools/blob/master/copy_dylibs.py
