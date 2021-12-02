+++
title = "Site Reliability Engineering: Measuring and Managing Reliability"
categories = [
    "belajar",
]
tags = [
    "recommender"
    "machine-learning",
]
date = "2021-11-29"
draft = true
+++

## Apa perbedaan antara DevOps dan SRE

DevOps adalah kumpulan dari praktik dan budaya yang didesain untuk memecah batasan antara developers, operators dan bagian lain pada suatu organisasi.
Pratkiknya dipecah menjadi 5 bagian. 
Pertama, mengurangi silo organisasi dengan meruntuhkan batasan diseluruh tim dengan cara kolaborasi yang menyeluruh.
Kedua, menerima kegagalan dengan normal, maksudnya komputer pada dasarnya tidak dapat diandalkan sehingga kita jangan berharap kesempurnaan.
Ketiga, menerapkan perubahan bertahap, tidak hanya kecil tapi perubahan secara inkrimental akan lebih mudah ditinjau.
Misalnya, lebih mudah untuk melakukan rollback jika ada bug pada production.
Keempat, memanfaatkan tooling dan otomatisasi
Kelima,  mengukur apapun adalah hal yang kritikal untuk menjadi sukses, tanpanya kita tidak akan tau apa yang perlu diperbaiki dan apa yang sudah sukses.

Lalu apa itu SRE.
Kalau kita memahami DevOps adalah suatu filosofi maka SRE adalah cara untuk mencapai filosofi itu.
Jadi kalau devops adalah inteface maka bisa dibilang SRE mengimplement DevOps.

Kalau kita bicara tentang mengurangi silo organisasi, kita bisa membuat environment yang sama antara development dan production.
Kalau bicara tentang kegagalan dengan normal, SRE mempraktikan budaya postmortem dan blameless dan juga memastikan kegagalan tidak akan terjadi untuk kedua kalinya.
Dan kita menerima kegagalan seperti biasa dengan konsep error budget atau seberapa banyak sistem diizinkan keluar dari spesifikasi.
Lalu tentang perubahan secara bertahap, kita dapat merilis ke sebagian kecil pengguna sebelum kita pindahkan ke semua pengguna.
Lalu tentang memanfaatkan tooling, kita bisa mengukur berapa banyak pekerjaan kita dan sebisa mungkin diotomatisasi.
Yang terakhir tentang mengukur, kita melakukan pengukuran ke semuanya, mengukur jumlah kerja, mengukur reability, mengukur kesehatan system kita.

Jadi SRE dan DevOps bukanlah dua metode yang saling berkompetisi, 
melainkan teman dekat, yang dirancang untuk membantu meruntuhkan hambatan organisasi untuk deliver software yang lebih baik dan cepat.

## Kenapa SLO peneting buat organisasi lo

### Bagaimana SLO membantu bisinis membuat keputusan
### Bagaimana SLO membantu untuk membuat fitur lebih cepat
### Bagaimana SLO membantu menyeimbangkan operational dan project

## Membuat SLO untuk berkerja dengan organisasi lo
## Bagaimana cara mengukur reability
## Seharusnya service seberapa reliable
## Kapan kita perlu membuat service menjadi lebih reliable. Error Budged
