+++
title = "Release It!, 2nd Edition"
categories = [
    "book-summary",
]
tags = [
    "books",
    "fundamental",
    "learn-from-mistakes",
    "Will Larson",
]
date = "2021-10-19"
draft = true
+++

## Introduction

Buku ini menceritakan tentang contoh dari kasus-kasus nyata yang pernah dialami oleh penulis. Penulisnya adalah [Michael T. Nygard](https://twitter.com/mtnygard?lang=gl) adalah seorang professional programmer dan architecht yang sudah berpengalaman lebih dari 15 tahun.

## Living in Production

Lo orang yang seperti apa, ketika

Lo sedang berkerja didalam suatu proyek, merasa sepertinya semua fitur sudah lengkap bahkan sudah ditest. ketika itu lo sudah bisa bernapas lega dan sudah selesai. 

Atau lo ?

Sebagai orang yang selalu bertanya.

- Fitur sudah lengkap tapi apakah sudah siap diproduksi?
- Apakah sistem lo siap untuk di deployed?
- Bisa gak dijalankan oleh pengguna tanpa lo?
- Apakah lo mulai merasa bahwa lo akan dihadapkan dengan panggilan telepon darurat dan peringatan ditengah malam?

Akan ada pertanyaan yang harus dijawab ketika aplikasi yang dibuat sudah berjalan diproduction.

Meskipun kita sudah membuat perencanaan terbaik. Keagalan masih mungkin terjadi.

### Aiming for the Right Target

Betujuan untuk target yang tepat.

Software design sekarang sama seperti desain mobil diawal tahun 90an. Terputus dari dunia nyata. dimana bentuknya cangih dan bagus tetapi berumur pendek.

Kebanyakan software juga begitu. 

Kita butuh software didesain secara individual dan seluruh ekosistem saling bergantung, untuk beroperasi dengan biaya rendah dan kualitas tinggi.

### The Scope of the challenge

Ruang lingkup tantangan.

Tantangan software jaman dulu akan diukur dalam satuan puluhan atau ratusan dengan paling banyak beberapa lusin pengguna secara bersamaan.

Jaman sekarang kita bahkan bisa melihat user lebih banyak dari total populasi manusia diseluruh dunia. Lihat saja facebook.

Kebutuhan uptime juga meningkat, ada istilah terkenal "five nines" uptime, 99.999 pecent. semua diharapkan bisa buka terus 24/7. 

Meningkatknya cakupan tantangan ini untuk membangun perangkat lunak dengan cepat dan murah, bagus untuk pengguna murah untuk dioperasikan dan terus menuntut untuk ditingkatkan dan teknik design secara terus menerus.

### A Million Dollars Here, a Million Dollars There

Banyak yang dipertaruhkan disini: Kesuksesan proyek lo, Opsi Saham atau pembagian keuntungan lo, Kelangsungan hidup perusahaan lo dan bahkan pekerjaan lo.

Keputusan desain dan arsitektur juga merupakan keputusan finansial. Pilihan-pilihan ini harus dibuat dengan memperhatikan biaya implementasi dan biaya hilirnya (downstream). Perpaduan sudut pandang teknis dan keuangan adalah salah satu tema penting disini.

### Use the Force

Keputusan awal lo akan membuat dampak terbesar pada akhirnya.

Keputusan awal ini bisa jadi adalah keputusan yang  paling sulit untuk diubah nanti.

Keputusan awal tentang batas sistem dan dekomposisi kedalam subsistem  ini akan dikristalisasi kedalam struktur tim, alokasi dana,struktur management program dan bahkan kode.

Ironisnya keputusan awal ini yang yang paling sedikit diinformasikan, ngeri.

Sepertinya desainer harus "use the force" untuk melihat masa depan dan memilih desain yang paling kuat. Karen alternatif berbeda sering kali memiliki biaya implementasi yang serupa tapi biaya siklus hidupnya berbeda, penting untuk mempertimbangkan setiap efek dari keputusan pada availabilitu, kapasistas dan fleksibilitas.

### Pragmatic Architecture 

Ada dua jenis arsitek, arsitek menara gading dan arsitek pragmatis.

Arsitek menara gading hanya memperdulikan hype tanpa kebutuhan. tidak ada alasan yang jelas dalam menentukan keputusan, mereka juga tidak memperdulikan kebutuhan pembuat kode.

Jadilah arsitek pragmatis yang semua keputusannya berdasarkan data,berkerja sama dengan pembuat code. dan cenderung membahas issue seperti kebutuhan CPU, kebutuhan bandwith dan keuntungan dan kerugian dari hyperthreading dan CPU binding.

Arsitek pragmatis terus menerus memikirkan dinamika perubahan

- Bagaimana kita bisa mendeploy tanpa downtime?
- Metrik apa saja yang perlu dikumpulkan?
- Bagaimana menganisanya?
- Bagianmana dari sistem yang membutuhkan perbaikan?

### Wrapping Up

Membangun sistem tidak bisa sebarangan banyak faktor yang dipertaruhkan, banyak orang yang terlibat dalam keputusan yang diambil. Jadilah arsitek yang pragmatis yang mengerti bagaimana sistem seharusnya berkerja, kapan harus diganti, kapan harus ditambah.

## Part 1. Ciptakan Stabilitas
### 2. Case Study: The Exception That Grounded an Airline
![[Pasted image 20211014161811.png]]
Ini adalah kisah nyata, tapi nama, tempat dan tanggal telah diubah untuk melindungi kerahasiaan orang dan perusahaan yang terlibat.

Ada ada satu maskapai yang sistemnya down hingga tiga jam. Karena ada satu kesalahan kecil.
Menyebabkan aktifitas penerbangan terhenti dan kerugian hingga ribuan dolar.
Kesalahan kecil ini terjadi hanya di satus sistem tapi menyebar kebanyak sistem, karena kegagalannya ada dicore functionnya.

- Bagaimana kita mencegah bug didalam satu sistem dapat mempengaruhi yang lainnya?

Kita tidak boleh membiarkan bug meyebabkan rantai kegagalan.

### 3. Stabilkan sistem lo!

Software yang baru dirilis seperti anak fresh graduate. penuh semagat dan optimis yang tiba tiba menghadapi kenyataan pahit diluar sana. banyak hal hal yang tidak pernah diajarkan di kampusnya.

Software baru haruslah sinis, selalu berekspektasi jika hal hal buruk terjadi dan tidak terkejut ketika itu terjadi.
Selalu tidak percaya sehingga selalu belajar dan melindungi dirinya dari kegagalan.

Seperti halnya yang bisa dipelari dari kasus sebelumnya, terlalu percaya diri dengan sistem baru dengan teknologi baru dan arsitektur yang canggih hingga akhirnya sistem mati selama hampir tiga jam hanya karena bugs yang kecil seperti itu.

Stabilitas yang baik tidak selalu membutuhkan biaya. Saat membangun arsitektur, desain dan bahkan implementasi banyak keputusan yang penting atas kestabilan sistem.

Desain yang sangat stabil biasanya membutuhkan biaya yang sama dengan desain yang tidak stabil.

#### Apa itu stabilitas?

Kalo bicara tentang stabilitas, kita perlu tau dulu beberapa istilah.

Transaction adalah unit abstrak yang diproses oleh sistem.
Transaction disini bukan transaction database ya. Satu unit kerja pada sistem bisa saja ada beberapa transaction database.
Misalnya, transaksi yang umum pada e-commerce, pada saat pelanggan memesan, banyak sekali transaksi yang terjadi. Verifikasi Pembayaran, Perubahan stok pada gudang, dll.

Sistem yang kuat akan terus memperoses transaksi itu, bahkan ketika ada "kejutan" sementara (misalnya flash sale)) dan tekanan terus menerus atau kegagalan komponen yang memnggangu pemrosesan. Inilah yang dimaksud "Stabilitas" oleh kebanyakan orang. Bukan hanya server atau aplikasi yang tetap aktif berjalan tapi pengguna masih bisa menyelesaikan transaksinya.

#### Memperpanjang rentang hidup anda

Bahaya utama dari umur panjang sistem adalah memory leak dan data growth. Mereka akan mematikan sistem lo di production.
Dan mereka jarang ketauan pada saat testing oleh QA.

Disini penulis bertanya, jadi bagai mana lo menemukan bugs kaya gini? Dia bilang satu satunya cara ya tangkap bugs itu seelum ada diproduction dan jalankan longevity test. 
Longevity test yang dimaksud adalah load test, yang dijalankan secara terus menerus.

#### Mode kegagalan

#### Stoping Crack Propagation

#### Chain of Failure

#### Wrapping Up

### 4. Stability Antipatterns

### 5. Stability Patterns

## Part 2. Design For Production

### 6. Case Study: Phenomenal Cosmic Powers, Itty-Bitty Living Space

### 7. Foundations

### 8. Processes on Machines

### 9. Interconnect

### 10. Control Plane

### 11. Security

## Part 3. Deliver Your System

### 12. Case Study: Waiting for Godot

### 13. Design for Deployment

### 14. Handling Versions

## Part 4. Solve Systemic Problems

### 15. Case Study: Trampled by Your Own Customers

### 16. Adaptation

### 17. Chaos Engineering