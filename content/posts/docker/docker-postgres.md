+++
title = "Install postgre pakai docker"
categories = [
    "belajar",
]
tags = [
    "docker",
    "postgre",
]
date = "2020-03-27"
+++

Docker saat ini sudah menjadi normal yang baru dalam pengembangan perangkat lunak.
Docker mempermudah kita sebagai pengembang dalam melakukan instalasi aplikasi.
Teringat jaman dulu waktu awal-awal mengembangkan perangkat lunak, 
Harus menginstall sepertangkat pake aplikasi seperti XAMP atau LAMP untuk mempermudah instalasi.

Sekarang tidak perlu lagi, cukup menggunakan docker kita bisa menginstallnya.

## Installing Postgre SQL

Untuk mendapatkan docker images container cukup mudah dengan code.

```bash
docker pull postgres
```

kode diatas akan mendownload postgre versi terakhir, kalau ingin versi yang spesifik bisa menggunakan code ini:

```bash
docker pull postgres:9.6.17-alpine
```

kode tambahan `:9.6.17-alpine` adalah versi dari postgre yang ingin diinstall. Untuk melihat versi selengkapnya bisa lihat di [https://hub.docker.com](https://hub.docker.com/_/postgres?tab=tags).

## Menyimpan data yang ada dipostgre

Yang namanya database pasti akan ada data yang disimpan, kita tidak boleh menyimpan data didalam kontainer. Kenapa?
Kontainer itu sifatnya sementara dia dapat dimatikan dan digantikan. Sedangkan datanya tidak boleh hilang.
Cara untuk menyimpan data dalam hardisk komputer kita dengan menggunakan [volumes](https://docs.docker.com/storage/volumes/)

Bikin folder tempat dimana data akan disimpan

```bash
mkdir docker/volumes/postgres
```

Jalankan kontainer

```bash
docker run -p 5432:5432 \
    --name pgsql-docker \
    -v $HOME/docker/volumes/postgres:/var/lib/postgresql/data \
    -e POSTGRES_PASSWORD=postgres  \
    -d postgres
```

Untuk mengaksesnya mudah saja hanya perlu akses menggunakan data source name

```bash
psql -h localhost -U postgres -d postgres
```

## Menginstall PgAdmin sebagai GUI postgre pada docker

Kalau sebelumnya kita mengakses dengan psql atau konek menggunakan source code aplikasi kita.
Sekarang kita coba menginstal GUI untuk postgre yaitu PgAdmin. Caranya mirim seperti menginstall postgre.

Didownload dulu PgAdminnya

```docker 
docker pull dpage/pgadmin4
```

Lalu jalan kan kontainer

```bash
docker run -p 80:80 \
    -e 'PGADMIN_DEFAULT_EMAIL=user@domain.com' \
    -e 'PGADMIN_DEFAULT_PASSWORD=SuperSecret' \
    -d dpage/pgadmin4
```

Akses `localhost` pada browser maka kita akan ditampilkan halaman login, dimana user dan passwordnya sudah kita buat ketika menjalankan pgadmin

```bash
-e 'PGADMIN_DEFAULT_EMAIL=user@domain.com' \
-e 'PGADMIN_DEFAULT_PASSWORD=SuperSecret' \
```

Untuk mengakses databasenya ada sedikit perbedaan, kita tidak bisa menggunakan `localhost` 
seperti ketika kita mencoba konek menggunakan psql yang ada dikomputer kita.
Kita dapat mengakses dengan menggunakan ip dari container, caranya

```bash
docker inspect pgsql-docker
```

maka akan mengasilkan list berupa json, kita fokus saja pada bagian `NetworkSettings`

```json
"NetworkSettings": {
    ...
    "IPAddress": "172.17.0.2",
    ...
}
```

Kita dapat menggunakan ip `172.17.0.2` sebagai host pada PgAdmin. 
Dan sebenarnya sudah ada dokumentasinya [disini](https://www.pgadmin.org/docs/pgadmin4/development/container_deployment.html)

Selesai.
