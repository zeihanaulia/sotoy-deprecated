+++
title = "Go Version Manager"
categories = [
    "golang",
]
tags = [
    "golang",
    "gvm",
    "tips",
]
date = "2021-02-18"
draft = false
+++

Go Version Manager adalah tools yang membantu memperbaharui Go Version.

## Instalasi

```bash
bash < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
```

```zsh
zsh < <(curl -s -S -L https://raw.githubusercontent.com/moovweb/gvm/master/binscripts/gvm-installer)
```

## Penggunaan

```bash
$ gvm 

Usage: gvm [command]


Description:

  GVM is the Go Version Manager


Commands:

  version    - print the gvm version number

  get        - gets the latest code (for debugging)

  use        - select a go version to use (--default to set permanently)

  diff       - view changes to Go root

  help       - display this usage text

  implode    - completely remove gvm

  install    - install go versions

  uninstall  - uninstall go versions

  cross      - install go cross compilers

  linkthis   - link this directory into GOPATH

  list       - list installed go versions

  listall    - list available versions

  alias      - manage go version aliases

  pkgset     - manage go packages sets

  pkgenv     - edit the environment for a package set
```

## Install Go

```bash
$ gvm install go1.16 -B
$ gvm use go1.16 --default
```

## Setup $GOROOT dan $GOPATH

```bash
[[ -s"$HOME/.gvm/scripts/gvm" ]] && source"$HOME/.gvm/scripts/gvm"

export GOPATH=$HOME/go

export GOBIN=$GOPATH/bin

export PATH=${PATH}:$GOBIN
```