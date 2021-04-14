+++
title = "Pecarian teks dengan elasticsearch"
categories = [
    "belajar",
    "algolia",
    "golang",
]
tags = [
    "golang",
    "full-text search",
]
date = "2020-06-16"
draft = true
+++

Bagaimana sih cara kita melakukan pencarian text dari data yang kita miliki? 
Biasanya kalau diRDBMS seperti Postgre atau mysql kita bisa menggunakan query misal `SELECT name FROM products WHERE name ILIKE '%%'`
Tapi cara itu banyak sekali batasan.

## Apa itu elasticsearch

- mesin pencari berbasis lucene library
- skema bebas menggunakan JSON
- berbasis Rest API
- Berjalan Di Java
- Mendukung dibanyak bahasa pemrograman
- Pencarian hampir mendekati realtime (cepat sekali)
- Standarnya mengindex semua field

## Install dengan Docker

```bash
docker pull docker.elastic.co/elasticsearch/elasticsearch:7.7.1
```

### Single node cluster

```bash
docker run -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.7.1
```

Jika instalasi benar, bisa coba untuk membuka `http://localhost:9200/` atau liat pemeriksaan kesehatan `http://localhost:9200/_cat/health`

## Mencoba API Elasticsearch

### Membuat index

### Memasukan data

### Query data

### Ubah data

## Implementasi dengan golang

### Membuat koneksi ke Elasticsearch

### Membuat index dengan golang

### Memasukan data dengan golang

### Mengambil data dengan golang

### Mengubah data dengan golang

### Menghapus data dengan golang

Referensi:

- [Install elastic search with docker](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html#docker)
- [How to implement Elasticsearch in Go](https://www.freecodecamp.org/news/go-elasticsearch/)
- [What Is Elasticsearch?](https://mentormate.com/blog/what-is-elasticsearch/?utm_source=Quora&utm_medium=Social&utm_term=2-1-17%20Fulltext%20Elasticsearch%20Blog%20Promo%20JC)
- [Introduction in the Elasticsearch REST client](https://www.youtube.com/watch?v=1QXPiFKaS3k)
- [23 Useful Elasticsearch Example Queries](https://dzone.com/articles/23-useful-elasticsearch-example-queries)
- http://dogdogfish.com/guide/building-a-search-engine-for-e-commerce-with-elasticsearch/
- https://www.airpair.com/elasticsearch/posts/elasticsearch-robust-search-functionality#7-data-ingestion-strategies

