+++
title = "Cookiecutter: Better Project Templates"
categories = [
    "tooling",
]
tags = [
    "coding productivity"
]
date = "2021-10-26"
draft = true
+++

Kalau kita sudah sering membuat program seperti webservice, mobile app, dll, pasti membosankan sekali ketika melakukan hal yang sama setiap kali memulai proyek. Seperti membuat layout directory, meng-import packages, membuat code inisiasi dan lain lain. Oleh karena itu kita akan coba salah satu tools yang ditulis dengan bahasa python yaitu cookiecutter.


## Apa itu Cookiecutter

[Cookiecutter](https://github.com/cookiecutter/cookiecutter) dibuat pada tahun 2013, dan terus diurusi oleh pengembangnya hingga sekarang.

Dia berbentuk command-line utility yang dapat menghasilkan project diawal, dengan begitu akan mempercepat kita dalam memulai project.
Menggunakan cookiecutter cukup mudah, kita hanya perlu [menginstall cookiecutter](https://cookiecutter.readthedocs.io/en/latest/installation.html) dan membuat templates atau menggunakan templates yang sudah ada jika sudah sesuai dengan kebutuhan kita.


```bash
$ cookiecutter https://github.com/zeihanaulia/cookiecutter-go-service

// windows

$ python -m cookiecutter https://github.com/zeihanaulia/cookiecutter-go-service

```

Pembuatan templatenya pun cukup mudah. Kita hanya perlu *cookiecutter.json*  sebagai config file dan *{{ cookiecutter.project_name }}* sebagai project template di repository git kita, tidak harus di git repository juga sih, menggunakan folder path juga bisa. 

## Mencoba membuat templates

Oke, Sekarang kita buat template project sederhana untuk aplikasi golang.

buat dulu folder `cookiecutter-go-service`.

Lalu buat file *cookiecutter.json* dengan isi seperti ini


```json
{
    "github_username": "zeihanaulia",
    "app_name": "mysimpleservice",
    "project_short_description": "A Golang simple service."
}
```

Dan terakhir buat folder dengan nama *{{ cookiecutter.project_name }}*.

```bash
- /cookiecutter-go-service
-     /{{ cookiecutter.project_name }}
-     cookiecutter.json
```

Jika inisiasi untuk cookiecutter sudah dibuat, kita lanut ke templates service yang mau kita buat.
Struktur dari templates atau layout yang mau kita buat, akan disusun didalam folder ***{{ cookiecutter.project_name }}***.
Struktur yang akan kita buat akan seperti ini.

```bash
- /cmd
- /config
- /internal
- Dockerfile
- Makefile
- README.md
- go.mod
- main.go
```

Kita mulai dari ***README.md***, untuk mendapat inspirasi bentuk README bisa lihat di [awesome-readme](https://github.com/matiassingers/awesome-readme). Kita bisa mengambil data dari ***cookiecutter.json*** dengan menggunakan syntax ***{{cookiecutter.field_config}}***. 
Contohnya seperti ini:

```markdown
<h1 align="center">  {{cookiecutter.app_name}} </h1> <br>
<p align="center">
  {{cookiecutter.project_short_description}}
</p>
```

Setelah itu kita buat file go modules atau ***go.mod*** file. Biasanya berisi package - package yang kita gunakan dalam pembuatan aplikasi. Sebagai contoh kita akan menggunakan [cobra](https://github.com/spf13/cobra) dan [viper](https://github.com/spf13/viper).

```bash
module github.com/{{cookiecutter.github_username}}/{{cookiecutter.app_name}}

require (
	github.com/spf13/cobra v1.2.1
	github.com/spf13/viper v1.9.0
)
```

Dalam pembuatan go module kita memperlukan nama modules yang seringnya kita buat mengikuti path dari repository code kita. Sebagai contoh `github.com/zeihanaulia/mysimpleservice`. Sama seperti readme kita ambil informasi dari config cookiecutter ***{{cookiecutter.github_username}}*** dan ***{{cookiecutter.app_name}}***.