+++
title = "Membuat modeling microservice"
categories = [
    "belajar",
    "microservice",
]
tags = [
    "microservice",
    "api-gateway",
]
date = "2020-07-08"
draft = false
+++

## Cara membuat batasan pada microservice

### Infomation Hiding

Information hiding ini diperkenalkan oleh [David Parnas](https://blog.acolyer.org/2016/09/05/on-the-criteria-to-be-used-in-decomposing-systems-into-modules/).
Intinya menyembungikan detail sebisa mungkin dari batasan modul.

Keuntungannya:

1. Improved Development Time. = Kenapa? Karena modules dapat didevelop secara independen.
2. Comprehensability = Dapat dimengerti, Kenapa? karena antar module bekomunikasi hanya menggunakan interface masing masing.
3. Flexibility = Kenapa? Karena module bisa diubah secara independent. Tanpa perlu takut perubahan pada module lain.

> The connections between modules are the assumptions which the modules make about each other.
> --David Parnas

Dengan menjaga asumsi selalu kecil, itu lebih mudah untuk kita mengubah module tanpa berimpact ke yang lain.

## Cohesion

Kode yang baik adalah kode yang high cohesion

> Kode yang berubah bersama, akan tetap bersama. Dan hanya ada satu alasan untuk mengubahnya.

- [What does 'low in coupling and high in cohesion' mean](https://stackoverflow.com/questions/14000762/what-does-low-in-coupling-and-high-in-cohesion-mean])
- [Difference Between Cohesion and Coupling](https://stackoverflow.com/questions/3085285/difference-between-cohesion-and-coupling)

## Contoh high cohesion

## Coupling

Kode yang baik adalah kode yang loose copling. Perubahan satu service atau module tidak membutuhkan module lain untuk diubah juga.

## Tipe tipe coupling

- Domain
- Tramp
- Common
- Content

### Domain Coupling

Situasi dimana satu microservice butuh untuk berinteraksi dengan microservice lainnya.

Contoh,

**Service Order Processing**, akan berinteraksi ke **Service Warehouse** untuk memesan barang. Dan Juga,
**Service Order Processing**, akan berinteraksi ke **Service Payment** untuk menerima pembayaran.

Dalam domain coupling ada istilah **Temporal Coupling**. Dimana keduanya harus sama-sama tersedia diwaktu yang sama.

Contoh, HTTP Request secara synchronous.

Misalnya **Service Order Processing** akan memanggil **Service Warehouse**.
Jika tiba-toba **Service Warehouse** mati, maka transaksi akan gagal, Pelanggan kita tidak bisa membeli barangnya.
**Service Order Processing** juga akan mendapat masalah, karena akan memblock dan terus menunggu jawaban dari **Service Warehouse**.

Apa temporal coupling seperti ini buruk? gak juga sih. Cuma perlu perhatian lebih aja.
Atau mekanismenya kita ganti menjadi asyncronous, menggunakan message broker.

### Tramp Coupling

Situasi dimana service melempar data ke service layanan karena dibutuhkan.

Misalnya **Service Order Processing** melempar data yang Berisi data penjualan dan Dokumen Pengiriman ke **Service Warehouse** lalu dilanjutkan ke **Service Pengiriman**.
Semua diproses berdasarkan data dari **Service Order Processing**. 

#### Menjadi masalah

Masalah muncul, karena tim dari **Service Pengiriman** dan **Service Warehouse**, Mengubah format datanya. 
Perubahan tersebut akan berakibat ke **Service Order Processing**.
Tim **Service Order Processing** harus mengubah data sesuai dengan format yang diinginkan **Service Pengiriman** dan **Service Warehouse**.

#### Yaudah ubah format masing masing deh, ikutin aja

Masalah lagi, karena **Service Order Processing**, jadi gak jelas domainnya. Tugas mereka mengurus order masuk, kenapa mengurusi Pengiriman dan Warehouse juga?

#### Inget tentang hiding information.

Tim **Service Order Processing**. Hanya mengirimkan data yang mereka punya saja. Sisanya service masing masing yang olah.
Masih masuk akal kalau **Service Order Processing**, punya data barang yang dibeli dan alamat yang ingin dikirimkan.

### Common Coupling

Situasi dimana 2 micoservice berbagi data. Misal **Service Order Processing** dan **Service Warehouse** mengambil data kota administrasi didatabase yang sama.

Masalah utama pada coupling ini akan terjadi jika struktur datanya berubah. Service yang integrasi ke data tersebut harus ikut melakukan perubahan.
Tapi itu masalah receh lah. gak berimpact terlalu besar. Tapi apa yang terjadi jika yang dishare adalah table order.
**Service Order Processing** mengubah status menjadi *placed*, *paid*, *completed* dan **Service Warehouse** mengubahnya menjadi *picking* dan *shipped*.
Keduanya bertanggung jawab memanage status pada table order.

### Content Coupling