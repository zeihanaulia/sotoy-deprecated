+++
title = "Cara menggunakan docker"
categories = [
    "belajar",
]
tags = [
    "docker",
]
date = "2020-04-12"
+++

Sebelumnya pernah bikin tulisan bagaimana [install postgre sql menggunakan docker]({{< ref "/posts/docker/docker-postgres.md" >}}).
Kali ini mau coba ginmana caranya menggunakan docker dan [docker hubnya](https://hub.docker.com).
Docker hub adalah salah satu layanan dari docker dimana kita dapat mencari dan membagi container images.
Layanan ini bisa digunakan secara gratis dan berebayar. Cek aja [disini](https://docs.docker.com/docker-hub/) kalo mau tau lebih lanjut.

Tulisan kali ini ditujukan untuk dokumentasi cara menggunakan docker hub.
Anggap kita sudah menginstall docker dikomputer kita masing masing. 
Kalo belom ada [unduh](https://www.docker.com/products/docker-desktop) aja.

## Cara login menggunakan akses token

Login docker desktop ada beberapa cara.

1. Menggunakan GUI
2. Menggunakan cli dengan username dan kata sandi
3. Menggunakan access token

### Menggunakan GUI

Kalo menggunakan GUI dengan cara klik icon ikan paus dengan kotak kotak diatasnya dan nanti disana ada `Sing In \ Create Docker ID`.
Klik aja tulisan `Sing In \ Create Docker ID`, nanti akan muncuk pop up form untuk mengisi docker id dan kata sandi.
Kalau belum punya docker ID jelas ya, bisa pencet [hub.docker.com](https://hub.docker.com/). 
Nanti akan diarahkane ke website untuk melakukan registrasi Docker ID.
Kalo udah punya ya tinggal masukin aja.



### Menggunakan CLI username dan kata sandi

Kalo ini cukup ketik 

```bash
 docker login -u <username>
 Password:
```

Kalau dienter akan diminta kata sandinya.

### Menggunakan access token.

Dua cara diatas masih menggunakan kata sandi, Apa untungnya menggunakan [access token](https://docs.docker.com/docker-hub/access-tokens/).
Di Dokumentasinya sudah dijelasin apa keuntungannya. Dibantuin buat bahasa indonesia deh.

Beberapa keuntungan menggunakan akses token pribadi dibandingkan menggunakan kata sandi:

- Anda dapat menyelidiki kapan akses token digunakan terakhir kali, dan menonaktifkan atau menghapusnya jika Anda menemukan aktivitas yang mencurigakan.
- Saat masuk dengan akses token, Anda tidak dapat melakukan aktivitas admin di akun, termasuk mengubah kata sandi.

Udah tau kan keuntungannya. Ini cocok untuk digunakan pada akun organisasi, tapi akun pribadi juga bisa. Caranya.

Pertama langsung arahin aja ke halaman akses token ada di `Username > Account Setting > Security` atau klik aja [https://hub.docker.com/settings/security](https://hub.docker.com/settings/security).

Klik `New Access Token`. Masukan deskripsi dari akses tokennya. Kalo udah nanti akan dikasih akses token.

```md
To use the access token from your Docker CLI client:
1. Run docker login --username <username>
2. At the password prompt, enter the personal access token.
   112ebce2-7340-4568-9dc5-6b0c8e3df5a9
```

Mestinya kode akses tokennya disimpan saja dikomputer kita. Karena kita tidak bisa melihat kembali akses tokennya ketika ditutup.
Kalau lupa, harus membuat akses token baru.

Kita bikin aja folder `accesstoken`, lalu isinya kita beri nama deskripsi dari akses token yang barusan dibuat. contoh.

```bash
mkdir accesstoken; cd accesstoken
echo "112ebce2-7340-4568-9dc5-6b0c8e3df5a9" > description-token-docker
cat ./description-token-docker | docker login --username <username> --password-stdin
Login Succeeded
```

Bagaimana jika tokennya kita `inactive`.

```bash
docker logout
cat ./description-token-docker | docker login --username <username> --password-stdin
Error response from daemon: Get https://registry-1.docker.io/v2/: unauthorized: incorrect username or password
```

Keren abis, kita juga bisa liat kapan terakhir kali digunakan, ada didashboardnya,

```bash
LAST USED
Apr 12, 2020   13:56:23
```

## Build otomatis



## Menjalankan test secara otomatis ketika push code ke repo

## Cobain hooks

## Cobain docker hub webhooks

## Sumber

- [Provide a password using STDIN](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin)
- [Automate Developer Workflows and Increase Productivity with Docker Hub: DockTalk](https://www.youtube.com/watch?v=C0rURibxI5M&list=PLpGUO-XMGLv-KKNZUW5LSSj5nuXF_K53_&index=4&t=0s)