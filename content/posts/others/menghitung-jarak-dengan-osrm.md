+++
title = "Menghitung jarak dengan OSRM"
categories = [
    "belajar",
]
tags = [
    "osrm",
]
date = "2020-06-10"
draft = true
+++

Kebetulan dikantor ingin ada improvement pada aplikasinya. Improvementnya adalah menhitung jarak dari lokasi pembeli ke warung yang terdekat dengannya.
Ya kita sering lihat pada aplikasi ojol, Kalo kita mau pesan makanan disana kita bisa melihat perekiraan jarak dari lokasi kita dengan penjualnya.

Ada banyak solusi untuk menyelesaikannya ada yang bentuknya [Mapping API](https://www.analyzo.com/search/mapping-apis/209) atau Routing Engine, Seperti:

1. [OSRM (Open Source Routing Machine)](http://project-osrm.org/)
2. [valhalla](https://github.com/valhalla/valhalla)
3. [pgRouting](https://pgrouting.org/)

Ditulisan kali ini kita akan mencoba Routing Engine, Produk yang digunakan adalah OSRM.

## Instalasi OSRM

Instalasi OSRM cukup mudah dengan menggunakan docker.
Imagenya dari [https://hub.docker.com/r/osrm/osrm-backend/](https://hub.docker.com/r/osrm/osrm-backend).
Tapi sebelum itu siapin mapsnya dulu, silakan download di website ini [http://download.geofabrik.de/](http://download.geofabrik.de/).

```bash
curl -O https://download.geofabrik.de/asia/indonesia-latest.osm.pbf
``