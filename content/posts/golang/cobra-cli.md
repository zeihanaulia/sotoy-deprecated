+++
title = "Cobra CLI"
categories = [
    "golang",
]
tags = [
    "golang",
    "cobra",
]
date = "2021-02-18"
draft = true
+++

[Cobra](https://github.com/spf13/cobra) adalah library yang digunakan untuk membuat CLI application

## Installasi

```bash
go get -u github.com/spf13/cobra
```

## Penggunaan

```bash
$ cobra      

Cobra is a CLI library for Go that empowers applications.
This application is a tool to generate the needed files
to quickly create a Cobra application.

Usage:
  cobra [command]

Available Commands:
  add         Add a command to a Cobra Application
  help        Help about any command
  init        Initialize a Cobra Application

Flags:
  -a, --author string    author name for copyright attribution (default "YOUR NAME")
      --config string    config file (default is $HOME/.cobra.yaml)
  -h, --help             help for cobra
  -l, --license string   name of license for the project
      --viper            use Viper for configuration (default true)

Use "cobra [command] --help" for more information about a command.
```