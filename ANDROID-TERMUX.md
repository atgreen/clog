## Using Termux on Android devilces for Common Lisp and CLOG Developent

Install Termux
https://f-droid.org/en/packages/com.termux/

```
pkg upgrade
pkg install openssh
pkg install emacs
pkg install zstd
pkg install libsqlite
```

To use _ECL_ use the following (sbcl below):

```
pkg install clang
pkg install ecl

curl -o ql.lisp http://beta.quicklisp.org/quicklisp.lisp
ecl
```
then run in ecl:

```
(load "ql.lisp")
(quicklisp-quickstart:install :path "~/.quicklisp")
(ql:add-to-init-file)
(ql:quickload :quicklisp-slime-helper)
```

To use _SBCL_ use the following instead:


```
curl -OL "https://github.com/bohonghuang/sbcl-termux-build/releases/download/2.3.3/sbcl-2.3.3-arm64-termux.tar.zst"
unzstd -c "sbcl-2.3.3-arm64-termux.tar.zst" | tar -xf -
cd "sbcl-2.3.3"
sh install.sh
```

```
curl -o ql.lisp http://beta.quicklisp.org/quicklisp.lisp
sbcl --no-sysinit --no-userinit --load ql.lisp \
       --eval '(quicklisp-quickstart:install :path "~/.quicklisp")' \
       --eval '(ql:add-to-init-file)' \
       --quit
sbcl --eval '(ql:quickload :quicklisp-slime-helper)' --quit
```

Both _SBCL and _ECL_ continue here:

Add to ~/.emacs.d/init.el

```
(load (expand-file-name "~/.quicklisp/slime-helper.el"))
(setq inferior-lisp-program "sbcl")
```

start emacs

M-x slime
```
(ql:quickload :clog)
```

A failure will occur for sqlite on sbcl, choose {use-value}
("/data/data/com.termux/files/usr/lib/libsqlite3.so")

```
(ql:quickload :clog/tools)
(clog-tools:clog-builder)
```