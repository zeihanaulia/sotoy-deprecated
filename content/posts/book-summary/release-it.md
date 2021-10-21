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

Buku ini menceritakan tentang contoh dari kasus-kasus nyata yang pernah dialami oleh penulis. Penulisnya adalah [Michael T. Nygard](https://twitter.com/mtnygard?lang=gl) adalah seorang professional programmer dan software architect yang sudah berpengalaman lebih dari 15 tahun.

## Living in Production

Ada dua jenis tipikal orang dan lo orang yang seperti apa, ketika

Lo sedang berkerja didalam suatu proyek, merasa sepertinya semua fitur sudah lengkap bahkan sudah ditest. ketika itu lo sudah bisa bernapas lega dan sudah selesai. 

Atau lo ?

Sebagai orang yang selalu bertanya.

- Fitur sudah lengkap tapi apakah sudah siap diproduksi?
- Apakah sistem lo siap untuk di deployed?
- Bisa gak dijalankan oleh pengguna tanpa lo?
- Apakah lo mulai merasa bahwa lo akan dihadapkan dengan panggilan telepon darurat dan peringatan ditengah malam?

Akan ada banyak pertanyaan yang harus dijawab ketika aplikasi yang dibuat sudah berjalan diproduction.

Meskipun kita sudah membuat perencanaan yang terbaik. Kegagalan masih mungkin akan terjadi.

### Aiming for the Right Target

Betujuan untuk target yang tepat.

Desain software jaman sekarang sama seperti desain mobil diawal tahun 90an. Terputus dari dunia nyata. Dimana bentuknya yang cangih dan bagus tetapi berumur pendek.

Kebanyakan software juga begitu. 

Kita butuh software yang didesain secara individual dan seluruh ekosistemnya saling bergantung, untuk beroperasi dengan biaya rendah dan berkualitas tinggi.

### The Scope of the challenge

Ruang lingkup tantangan.

Tantangan software jaman dulu mungkin bisa diukur dalam puluhan atau ratusan pengguna secara bersamaan.

Jaman sekarang kita bahkan bisa melihat user lebih banyak dari total populasi manusia diseluruh dunia. Lihat saja facebook.

Kebutuhan uptime juga meningkat, ada istilah terkenal "five nines" uptime, 99.999 pecent. Dengan kata lain sistem harus selalu bisa digunakan 24/7.

Cakupan tantangan pun meningkat untuk membangun sofware yang berkualitas dengan cepat dan murah, bisa digunakan dan mudah dioperasikan oleh pengguna, dan menuntut untuk ditingkatkan dan didesign secara terus menerus.

### A Million Dollars Here, a Million Dollars There

Banyak yang dipertaruhkan disini, misalnya Kesuksesan proyek lo, Opsi Saham atau pembagian keuntungan lo, Kelangsungan hidup perusahaan lo dan bahkan pekerjaan lo.

Keputusan desain dan arsitektur juga merupakan keputusan finansial. Pilihan-pilihan ini harus dibuat dengan memperhatikan biaya implementasi dan biaya hilirnya (downstream). Perpaduan sudut pandang teknis dan keuangan adalah salah satu tema penting disini.

### Use the Force

Keputusan awal lo akan membuat dampak terbesar pada akhirnya.

Keputusan awal ini bisa jadi adalah keputusan yang paling sulit untuk diubah nantinya.

Keputusan awal tentang batasan sistem dan dekomposisi kedalam subsistem ini akan dikristalisasi kedalam struktur tim, alokasi dana,struktur manajemen program dan juga kode yang ditulis.

Ironisnya, keputusan awal ini yang paling sedikit diinformasikan, ngeri.

Sepertinya desainer harus "use the force" untuk melihat masa depan dan memilih desain yang paling kuat atau terbaik. 

Karena alternatif berbeda sering kali memiliki biaya implementasi yang serupa tapi biaya siklus hidupnya berbeda, penting untuk mempertimbangkan setiap efek seperti availability, kapasistas dan fleksibilitas pada keputusan yang dibuat.

### Pragmatic Architecture 

Ada dua jenis arsitek, arsitek menara gading dan arsitek pragmatis.

Arsitek menara gading hanya memperdulikan hype tanpa kebutuhan. Tidak ada alasan yang jelas dalam menentukan keputusan, mereka juga tidak memperdulikan kebutuhan si pembuat kode.

Jadilah arsitek pragmatis yang semua keputusannya berdasarkan data, bekerja sama dengan pembuat code. dan cenderung membahas issue seperti kebutuhan CPU, kebutuhan bandwith dan keuntungan dan kerugian dari hyperthreading dan CPU binding.

Arsitek pragmatis terus menerus memikirkan dinamika perubahan, seperti:

- Bagaimana kita bisa mendeploy tanpa downtime?
- Metrik apa saja yang perlu dikumpulkan?
- Bagaimana menganalisanya?
- Bagian mana dari sistem yang membutuhkan perbaikan?

### Wrapping Up

Membangun sistem tidak bisa sebarangan, banyak faktor yang dipertaruhkan, banyak orang yang terlibat dalam keputusan yang diambil. Jadilah arsitek yang pragmatis yang mengerti bagaimana sistem seharusnya berkerja, kapan harus diganti, kapan harus ditambah.

## Part 1. Ciptakan Stabilitas

Pada part satu kita akan belajar dari kesalahan salah satu airline yang harus berhenti selama 3 jam karena bugs sepele yang membuat sistemnya down.
### 2. Case Study: The Exception That Grounded an Airline
Ini adalah kisah nyata, tapi nama, tempat dan tanggal telah diubah untuk melindungi kerahasiaan orang dan perusahaan yang terlibat.

Jadi ada satu maskapai yang sistemnya down hingga tiga jam. Karena ada satu kesalahan kecil. Yang menyebabkan aktifitas penerbangan terhenti dan kerugian hingga ribuan dolar. Kesalahan kecil ini terjadi hanya di satu sistem tapi menyebar kebanyak sistem, karena kegagalannya ada diCore Facilitiesnya. Setelah melakukan investigasi ditemukan bukti kalau semua sistem yang terintegrasi ke Core Facilities mengalami hang? Ternyata ada kegagalan koneksi ke database dari Core Facilities tapi Core Facilities tidak mengembalikan error, sehingga tidak ada inisiasi ulang atau koneksi ulang ke database.

Mereka membuat postmortem untuk mencatat investigasi dari kejadian ini agar dikemudian hari kita bisa belajar.

Oya, Core Facilities adalah software yang baru mereka rilis dengan teknologi terkini. Dan pada saat membuat, mereka sangat percaya diri dan membuat mereka jadi lalai dalam memprediksi hal lainnya.

Dari kisah ini kita belajar, untuk menjawab pertanyaan ini:

- Bagaimana hal seperti ini bisa dicegah?
- Apakah code review bisa menemukan bug seperti ini?
- Apakah dengan melakukan testing lebih bisa mencegah bug seperti ini?
- Bagaimana kita mencegah bug yang akan membuat kesalahan berantai?

### 3. Stabilkan sistem lo!

Software yang baru dirilis seperti anak fresh graduate. Penuh semangat dan optimis yang tiba-tiba harus menghadapi kenyataan pahit diluar sana karena Banyak hal-hal yang tidak pernah diajarkan di kampusnya.

Software yang baru rilis haruslah sinis, selalu berekspektasi jika hal-hal buruk terjadi dan tidak terkejut ketika itu terjadi.
Selalu tidak percaya sehingga selalu belajar dan melindungi dirinya dari kegagalan.

Seperti halnya yang bisa dipelari dari kasus sebelumnya, terlalu percaya diri dengan sistem baru dengan teknologi baru dan arsitektur yang canggih hingga akhirnya sistem mati selama hampir tiga jam hanya karena bugs yang kecil seperti itu.

Stabilitas yang baik tidak selalu membutuhkan biaya yang banyak. Ketika membangun arsitektur, desain dan bahkan low level implementasi banyak keputusan yang punya high leverage atas stabilitas utama dari sistem.

Dan menariknya adalah desain yang sangat stabil biasanya memiliki biaya yang sama seperti mengimplemen desain yang tidak stabil.

#### Apa itu stabilitas?

Kalo bicara tentang stabil, kita perlu tau dulu beberapa istilah.

Pertama adalah Transaction. 
Transaction adalah unit abstrak yang diproses oleh sistem.
Transaction disini bukan transaction database ya. 
Maksudnya adalah satu unit kerja pada sistem bisa saja ada beberapa transaction database.
Misalnya, transaksi yang umum pada e-commerce, pada saat pelanggan memesan, banyak sekali transaksi yang terjadi. Verifikasi Pembayaran, Perubahan stok pada gudang, dll.

Sistem yang kuat akan terus memperoses transaksi itu, bahkan ketika ada "kejutan" sementara (misalnya flash sale) dan tekanan terus menerus atau kegagalan komponen yang mengganggu pemrosesan. Inilah yang dimaksud "Stabilitas" oleh kebanyakan orang. Bukan hanya server atau aplikasi yang tetap aktif berjalan tapi pengguna masih bisa menyelesaikan transaksinya.

#### Extending Your Life Span

Bahaya utama dari umur panjang sistem adalah memory leak dan data growth.Mereka akan mematikan sistem lo di production.
Dan mereka jarang ketauan pada saat testing oleh QA.

Disini penulis bertanya, jadi bagaimana cara lo menemukan bugs yang kaya gini? Dia bilang satu satunya cara ya tangkap bugs itu sebelum ada diproduction dan jalankan longevity test. 
Longevity test yang dimaksud adalah load test, yang dijalankan secara terus menerus.

#### Failure Modes

Apapun yang terjadi, sistem lo harus punya beberapa variasi failure modes. Kalau lo mengabaikannya akan mengakibatkan lo gak punya kontrol dari sistem yang lo buat. Setelah lo menerima kegagalan yang mungkin akan terjadi. Lo akan punya kemampuan untuk merancang reaksi dari kegagalan itu.

Mengambil istilah dari James R. Chiles pada bukunya Inviting Disaster yaitu "crackstoppers". Seperti membangun zona yang akan menyerap benturan dan menjaga pengguna tetap aman.

Kalo lo gak mendesain failure mode. Lo gak bisa memperdiksi kejadian berbahaya dan memperbaikinya dengan cepat.
#### Stoping Crack Propagation

Belajar dari Core Facilities project yang tidak merencanakan mode kegagalan. Crack bermula dari kecerobohan menangani `SQLException`, padahal itu bisa saja dihentikan diawal atau dicarikan alternatifnya. Coba kita lihat.

Pencegahan bisa dimulai dari low-level detail sampai ke high-level architechture.

Low level misalnya membuat kode yang lebih proper agar bisa menangkap error.
High-Level misalnya membuat request/reply messages queue. Sehingga caller bisa tau kalo balasannya tidak pernah diterima. 

Intinya banyak cara atau pendekatan untuk mengentikan kegagalan si `SQLException` menyebar ke seluruh airlines. Sayangnya, para desainer tidak mempertimbangkan kemungkinan "crack" ketika membuat service yang digunakan bersama.

#### Chain of Failure

Disetiap sistem outage adalah kejadian berantai. Satu issue muncul akan berimpact ke yang lainnya dan menyebabkan issue ke yang lainnya lagi. Kegagalan gak bisa dihindari tapi kita bisa memprediksi kemungkinan kemungkinan.

Terminologi umum yang bisa kita gunakan:

- Fault / Kesalahan

    Kondisi dimana software lo tidak berjalan dengan benar. Bisa jadi karena ada bug atau ada kondisi yang tidak tercheck.

- Error
    
    Perlikau yang terlihat tidak benar. Misal pas lagi trading tiba tiba membeli 10 miliar dolar, ini adalah error.

- Failure / Kegagalan

    Sistem yang tidak merespon. Ketika sistem tidak merespon, bisa kita katakan ini gagal. Komputer mungkin berjalan tapi gak bisa meresponse apa apa.

Faults membuka crack, faults menjadi error dan error akan memprovokasi kegagalan. 

Salah satu cara untuk mempersiapkan setiap kemungkinan dari kegagalan adalah melihat setiap external call,setiao I/O, setiap penggunaan resources dan setiap ekspektasi outcome dan ask. Coba lo pikirin beberapa pertanyaan seperti ini:

- Bagaimana kalo gak bisa membuat inisiasi koneksi?
- Bagaimana jika membuat koneksi tapi membutuhkan waktu lama?
- Bagaimana jika sudah bisa membuat koneksi tapi abis itu langsung terputus?
- Bagaimana jika sudah bisa membuat koneksi tapi gak dapet response dari yang lain?
- Bagaimana jika query lambat merespon hingga 2 menit?
- Bagaimana jika ada 10 ribu request secara berasamaan?
- Bagaimana jika disk penuh ketika aplikasi lagi mau membuat log error message SQLException karena network down?

Itu hanya awalan dari semuanya yang bisa berjalan salah. 
Ada beberapa pendapat tentang bagaimana cara menangani faults. 

Pendapat pertama kira butuh membuat sistem fault-tolerant. Kita harus tangkap semua excepetions, cek error code, dan membuat faults menjadi error.

Pendapat kedua sebaliknya, percuma membuat fault-tolerance. Itu seperti mencoba membuat device anti bodoh, alam semesta akan selalu memberikan orang bodoh yang lebih baik. Gak peduli gimana cara lo buat menangkap keslahan dan memperbaikinya sesuatu yang tidak diekspektasikan akan selalu terjadi. Artinya ya biarin aja crash tinggal restart.

Tapi keduanya sepakat. Fault akan terjadi, mereka tidak pernah bisa secara sempurna mencegahnya. Dan kita harus terus menjaga faults agar tidak menjadi error. Lo harus memutuskan buat sistem lo, lebih baik gagal atau error. meskipun lo mencoa buat mencegah kegagalan dan error.


#### Wrapping Up

Kegagalan di production itu unik. Kejadiannya bisa berbeda beda. Yang bisa kita lakukan belajar dari kesalahan. Mengenal stability antipattern dan belajar dari patern dari kegagalan ini. 

Selain itu kita juga perlu pelajar stability pattern. belajar dengan desain dan arsitektur pattern untuk mengalahkan antipattern. Patern in gak bisa mencegajh crack pada sistem. gak ada yang bisa. dibeberapa kondisi akan selalu mentriger crack. tapi patern ini akan mencegah crack menyebar. 

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