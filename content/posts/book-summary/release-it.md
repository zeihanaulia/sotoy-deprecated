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

## **Introduction**

Buku ini menceritakan tentang contoh dari kasus-kasus nyata yang pernah dialami oleh penulis. Penulisnya adalah [Michael T. Nygard](https://twitter.com/mtnygard?lang=gl) adalah seorang professional programmer dan software architect yang sudah berpengalaman lebih dari 15 tahun.

## **Living in Production**

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

### **Aiming for the Right Target**

Betujuan untuk target yang tepat.

Desain software jaman sekarang sama seperti desain mobil diawal tahun 90an. Terputus dari dunia nyata. Dimana bentuknya yang cangih dan bagus tetapi berumur pendek.

Kebanyakan software juga begitu. 

Kita butuh software yang didesain secara individual dan seluruh ekosistemnya saling bergantung, untuk beroperasi dengan biaya rendah dan berkualitas tinggi.

### **The Scope of the challenge**

Ruang lingkup tantangan.

Tantangan software jaman dulu mungkin bisa diukur dalam puluhan atau ratusan pengguna secara bersamaan.

Jaman sekarang kita bahkan bisa melihat user lebih banyak dari total populasi manusia diseluruh dunia. Lihat saja facebook.

Kebutuhan uptime juga meningkat, ada istilah terkenal "five nines" uptime, 99.999 pecent. Dengan kata lain sistem harus selalu bisa digunakan 24/7.

Cakupan tantangan pun meningkat untuk membangun sofware yang berkualitas dengan cepat dan murah, bisa digunakan dan mudah dioperasikan oleh pengguna, dan menuntut untuk ditingkatkan dan didesign secara terus menerus.

### **A Million Dollars Here, a Million Dollars There**

Banyak yang dipertaruhkan disini, misalnya Kesuksesan proyek lo, Opsi Saham atau pembagian keuntungan lo, Kelangsungan hidup perusahaan lo dan bahkan pekerjaan lo.

Keputusan desain dan arsitektur juga merupakan keputusan finansial. Pilihan-pilihan ini harus dibuat dengan memperhatikan biaya implementasi dan biaya hilirnya (downstream). Perpaduan sudut pandang teknis dan keuangan adalah salah satu tema penting disini.

### **Use the Force**

Keputusan awal lo akan membuat dampak terbesar pada akhirnya.

Keputusan awal ini bisa jadi adalah keputusan yang paling sulit untuk diubah nantinya.

Keputusan awal tentang batasan sistem dan dekomposisi kedalam subsistem ini akan dikristalisasi kedalam struktur tim, alokasi dana,struktur manajemen program dan juga kode yang ditulis.

Ironisnya, keputusan awal ini yang paling sedikit diinformasikan, ngeri.

Sepertinya desainer harus "use the force" untuk melihat masa depan dan memilih desain yang paling kuat atau terbaik. 

Karena alternatif berbeda sering kali memiliki biaya implementasi yang serupa tapi biaya siklus hidupnya berbeda, penting untuk mempertimbangkan setiap efek seperti availability, kapasistas dan fleksibilitas pada keputusan yang dibuat.

### **Pragmatic Architecture**

Ada dua jenis arsitek, arsitek menara gading dan arsitek pragmatis.

Arsitek menara gading hanya memperdulikan hype tanpa kebutuhan. Tidak ada alasan yang jelas dalam menentukan keputusan, mereka juga tidak memperdulikan kebutuhan si pembuat kode.

Jadilah arsitek pragmatis yang semua keputusannya berdasarkan data, bekerja sama dengan pembuat code. dan cenderung membahas issue seperti kebutuhan CPU, kebutuhan bandwith dan keuntungan dan kerugian dari hyperthreading dan CPU binding.

Arsitek pragmatis terus menerus memikirkan dinamika perubahan, seperti:

- Bagaimana kita bisa mendeploy tanpa downtime?
- Metrik apa saja yang perlu dikumpulkan?
- Bagaimana menganalisanya?
- Bagian mana dari sistem yang membutuhkan perbaikan?

### **Wrapping Up**

Membangun sistem tidak bisa sebarangan, banyak faktor yang dipertaruhkan, banyak orang yang terlibat dalam keputusan yang diambil. Jadilah arsitek yang pragmatis yang mengerti bagaimana sistem seharusnya berkerja, kapan harus diganti, kapan harus ditambah.

## **Part 1. Ciptakan Stabilitas**

Pada part satu kita akan belajar dari kesalahan salah satu airline yang harus berhenti selama 3 jam karena bugs sepele yang membuat sistemnya down.
### **2. Case Study: The Exception That Grounded an Airline**
Ini adalah kisah nyata, tapi nama, tempat dan tanggal telah diubah untuk melindungi kerahasiaan orang dan perusahaan yang terlibat.

Jadi ada satu maskapai yang sistemnya down hingga tiga jam. Karena ada satu kesalahan kecil. Yang menyebabkan aktifitas penerbangan terhenti dan kerugian hingga ribuan dolar. Kesalahan kecil ini terjadi hanya di satu sistem tapi menyebar kebanyak sistem, karena kegagalannya ada diCore Facilitiesnya. Setelah melakukan investigasi ditemukan bukti kalau semua sistem yang terintegrasi ke Core Facilities mengalami hang? Ternyata ada kegagalan koneksi ke database dari Core Facilities tapi Core Facilities tidak mengembalikan error, sehingga tidak ada inisiasi ulang atau koneksi ulang ke database.

Mereka membuat postmortem untuk mencatat investigasi dari kejadian ini agar dikemudian hari kita bisa belajar.

Oya, Core Facilities adalah software yang baru mereka rilis dengan teknologi terkini. Dan pada saat membuat, mereka sangat percaya diri dan membuat mereka jadi lalai dalam memprediksi hal lainnya.

Dari kisah ini kita belajar, untuk menjawab pertanyaan ini:

- Bagaimana hal seperti ini bisa dicegah?
- Apakah code review bisa menemukan bug seperti ini?
- Apakah dengan melakukan testing lebih bisa mencegah bug seperti ini?
- Bagaimana kita mencegah bug yang akan membuat kesalahan berantai?

### **3. Stabilkan sistem lo!**

Software yang baru dirilis seperti anak fresh graduate. Penuh semangat dan optimis yang tiba-tiba harus menghadapi kenyataan pahit diluar sana karena Banyak hal-hal yang tidak pernah diajarkan di kampusnya.

Software yang baru rilis haruslah sinis, selalu berekspektasi jika hal-hal buruk akan terjadi dan tidak terkejut ketika itu benar-benar terjadi.
Selalu tidak percaya sehingga selalu belajar dan melindungi dirinya dari kegagalan.

Seperti yang bisa kita dipelari dari kasus sebelumnya, terlalu percaya diri dengan sistem baru dengan teknologi baru dan arsitektur yang canggih hingga akhirnya sistem mati selama hampir tiga jam hanya karena bugs yang kecil seperti itu.

Stabilitas yang baik tidak selalu membutuhkan biaya yang banyak. Ketika membangun arsitektur, desain dan bahkan implementasi low level, banyak keputusan yang punya high leverage atas stabilitas utama dari sistem.

Dan menariknya adalah desain yang sangat stabil biasanya memiliki biaya yang sama seperti mengimplemen desain yang tidak stabil.

#### **Apa itu stabilitas?**

Kalo bicara tentang stabil, kita perlu tau dulu beberapa istilah.

Pertama adalah Transaction. 
Transaction adalah unit abstrak yang diproses oleh sistem.
Transaction disini bukan transaction database ya. 
Maksudnya adalah satu unit kerja pada sistem bisa saja ada beberapa transaction database.
Misalnya, transaksi yang umum pada e-commerce, pada saat pelanggan memesan, banyak sekali transaksi yang terjadi. Verifikasi Pembayaran, Perubahan stok pada gudang, dll.

Sistem yang kuat akan terus memperoses transaksi itu, bahkan ketika ada "kejutan" sementara (misalnya flash sale) dan tekanan terus menerus atau kegagalan komponen yang mengganggu pemrosesan. Inilah yang dimaksud "Stabilitas" oleh kebanyakan orang. Bukan hanya server atau aplikasi yang tetap aktif berjalan tapi pengguna masih bisa menyelesaikan transaksinya.

#### **Extending Your Life Span**

Bahaya utama dari umur panjang sistem adalah memory leak dan data growth. Mereka akan mematikan sistem lo di production.
Dan mereka jarang ketauan pada saat testing oleh QA.

Disini penulis bertanya, jadi bagaimana cara lo menemukan bugs yang kaya gini? Dia bilang satu satunya cara ya tangkap bugs itu sebelum ada diproduction dan jalankan longevity test. 
Longevity test yang dimaksud adalah load test, yang dijalankan secara terus menerus.

#### **Failure Modes**

Apapun yang terjadi, sistem lo harus punya beberapa variasi failure modes. Kalau lo mengabaikannya akan mengakibatkan, lo gak punya kontrol dari sistem yang lo buat. 

Tapi kalau sudah mengekspektasikan failure modes sebelumnya, ketika lo menerima kegagalan yang mungkin akan terjadi. Lo akan punya kemampuan untuk merancang reaksi dari kegagalan itu.

Mengambil istilah dari James R. Chiles pada bukunya Inviting Disaster yaitu "crackstoppers". Seperti membangun zona yang akan menyerap benturan dan menjaga pengguna tetap aman.

Kalo lo gak mendesain failure mode. Lo gak bisa memperdiksi kejadian berbahaya dan memperbaikinya dengan cepat.
#### **Stoping Crack Propagation**

Belajar dari Core Facilities project yang tidak merencanakan mode kegagalan atau failure mode. Crack bermula dari kecerobohan menangani `SQLException`, padahal itu bisa saja dihentikan diawal atau dicarikan alternatifnya.

Pencegahan bisa dimulai dari low-level detail sampai ke high-level architechture.

Low level misalnya membuat kode yang lebih proper agar bisa menangkap error.

High-Level misalnya membuat request/reply messages queue. Sehingga caller bisa tau kalo balasannya tidak pernah diterima. 

Intinya banyak cara atau pendekatan untuk menghentikan kegagalan si `SQLException` menyebar ke seluruh sistem airlines. Sayangnya, para desainer tidak mempertimbangkan kemungkinan "crack" ketika membuat service yang digunakan bersama.

#### **Chain of Failure**

Disetiap sistem outage adalah kejadian berantai. Satu issue muncul akan berimpact ke yang lainnya dan menyebabkan issue ke yang lainnya lagi. Kegagalan gak bisa dihindari tapi kita bisa memprediksi kemungkinan kemungkinan yang akan terjadi.

Terminologi umum yang bisa kita gunakan:

- Fault / Kesalahan

    Kondisi dimana software lo tidak berjalan dengan benar. Bisa jadi karena ada bug atau ada kondisi yang tidak tercheck.

- Error
    
    Perilaku yang terlihat tidak benar. Misal pas lagi trading tiba-tiba membeli saham senilai 10 miliar dolar, ini adalah error.

- Failure / Kegagalan

    Sistem yang tidak merespon. Ketika sistem tidak merespon, bisa kita katakan ini gagal. Komputer mungkin berjalan tapi gak bisa meresponse apa apa.

Faults membuka crack, faults menjadi error dan error akan memprovokasi kegagalan. 

Salah satu cara untuk mempersiapkan setiap kemungkinan dari kegagalan adalah melihat setiap external call, setiap I/O, setiap penggunaan resources dan setiap ekspektasi outcome dan ask. Coba lo pikirin beberapa pertanyaan seperti ini:

- Bagaimana kalo gak bisa membuat inisiasi koneksi?
- Bagaimana jika membuat koneksi tapi membutuhkan waktu lama?
- Bagaimana jika sudah bisa membuat koneksi tapi abis itu langsung terputus?
- Bagaimana jika sudah bisa membuat koneksi tapi gak dapet response dari yang lain?
- Bagaimana jika query lambat merespon hingga 2 menit?
- Bagaimana jika ada 10 ribu request secara berasamaan?
- Bagaimana jika disk penuh ketika aplikasi lagi mau membuat log error message SQLException karena network down?

Itu hanya awalan dari semuanya yang bisa berjalan salah. 
Ada beberapa pendapat tentang bagaimana cara menangani faults. 

Pendapat pertama, kita butuh membuat sistem fault-tolerant. Kita harus tangkap semua excepetions, cek error code, dan membuat faults menjadi error.

Pendapat kedua sebaliknya, percuma membuat fault-tolerance. Itu seperti mencoba membuat device anti bodoh, alam semesta akan selalu memberikan orang bodoh yang lebih baik. Gak peduli gimana cara lo buat menangkap kesalahan dan memperbaikinya. Sesuatu yang tidak diekspektasikan akan selalu terjadi. Artinya ya biarin aja crash tinggal restart.

Tapi sebenarnya keduanya sepakat, Kalau Faults akan terjadi, mereka tidak pernah bisa secara sempurna mencegahnya. Dan kita harus terus menjaga faults agar tidak menjadi error. Lo harus memutuskan buat sistem lo, lebih baik gagal atau error. meskipun lo mencoba buat mencegah kegagalan dan error.

#### **Wrapping Up**

Kegagalan di production itu unik. Kejadiannya bisa berbeda-beda. Yang bisa kita lakukan belajar dari kesalahan. Mengenal stability antipattern dan belajar pola dari kegagalan ini. 

Selain itu kita juga perlu pelajar stability pattern. belajar dengan desain dan arsitektur pattern untuk mengalahkan antipattern. Patern ini gak bisa mencegah crack pada sistem. Dibeberapa kondisi akan selalu mentriger crack. tapi dengan belajar stability pattern akan mencegah crack menyebar. 

### **4. Stability Antipatterns**

Kenapa dunia software engineer masih terus dalam kondisi krisis? Karena memang krisis tidak akan pernah selesai. Software terus berevolusi, jika dulu hanya beberatus pengguna sekarang bisa jutaan bahkan miliaran pengguna.

Aplikasi sekarang juga terdiri dari raturan service yang semuanya berjalan secara continously dan diredeploy secara continously. Sistem besar melayani lebih banyak pengguna dengan menggunakan lebih banyak resource, tetapi dalam banyak mode kegagalan sistem besar gagal lebih cepat daripada sistem kecil.

Oleh karena itu kita perlu mengetahui dan menghindari stability antipattern. Tapi sebenarnya tidak cukup dengan menghindari aja. Pasti akan ketemu kesalahan yang tidak bisa dihindari juga. Tapi setidaknya kita sudah berasumsi kejadian terburuk. Kesalahan akan terjadi dan kita perlu memeriksa apa yang terjadi setelah kesalahan itu ada.

Selamat Belajar.

#### **Integration Points**

Ada dua pola pada pengembangan website, the butterfly dan the spider.

1. The butterfly

    Sistem terpusat biasa dikenal dengan sebutan monolith. Dimana ada satu sistem yang punya banyak tanggung jawab. 2N connection

    ![][the-butterfly]

2. The Spider

    Sistem dengan banyak service dan dependencies. Gambar pertama bentuk service yang memiliki tingkatan sedangkan yang kedua tidak ada tingkatan. 2^n connection. 
    
    Semua koneksi adalah integration point dan setiap connection bisa saja tidak berkerja

    ![][the-spider-1]
    ![][the-spider-2]

Integration points adalah pembunuh sistem nomor satu. Setiap point menghadirkan resiko stabilitas. Setiap socket, process, pipe atau rpc semuanya bisa hang. Bahkan database juga bisa hang.

**Http Protocols Problem**

Pertimbangkan beberapa titik integrasi ini yang dapat membahayakan caller:

- Provider mungkin menerima koneksi tapi mungkin gak pernah merespon balik.
- Provider mungkin menerima koneksi tapi tidak membaca request. 
- Privider mungkin mengembalikan response tapi caller tidak tau cara menghandlenya
- Provider mungkin mengembalikan response tapi content typenya berbeda dengan apa yang diespektasikan caller
- Provider merasa mengirimkan JSON padahal ngirim plain text.

Maka dari itu coba gunakan library yang bisa mengkontrol timeouts, connection timeout dan read timeout dan menangani response.

Penulis merekomendasikan hindari memproses response langsung pada domain object melankan response sebagai data yang sesuai dengan apa yang lo ekspektasikan.

**Countering Integration Point Problems**

Apa yang bisa lo lakukan untuk membuat integration point lebih aman?

Pattern paling efektif adalah Circuit Breaker dan Devoupling Middleware.

Testing juga membantu, software yang cynical  harus bisa menangani pelanggaran pada function. Untuk memastikan kalo software lo cynical, lo harus buat test harness atau simulator  dari perilaku pengguna yang dapat dikontrol pada setiap integration test.

**Remember This**

- Beware this necessary evil.

    Setiap integration point pada akhirnya aka gagal dalam banyak cara. Lo harus siap untuk kegagalan itu.

- Prepare for the many forms of failure.

    Kegagalan pada Integration point terjadi dalam banyak bentuk. mulai dari network error hingga semantic error. 

- Know when to open up abstractions.

    Debugging kegagalan pada integration point kadang bisa dicari ketika membaca abstraksi.

- Failures propagate quickly.

    Kegagalan pada remote system bisa jadi problem lo. biasanya sebagai cascading failure ketika kode lo tidak cukup defensive

- Apply patterns to avert integration point problems.

    Defensive programming lewat Circuit Breaker, Timeouts. Decoupling Middleware dan handshaking buat menghindari bahaya dari integration point.

#### **Chain Reactions**

Reaksi berantai dapat menyebabkan kegagalan ke seluruh sistem. 

**Remember This**

- Recognize that one server down jeopardizes the rest.

    Reaksi berantai terjadi karena salah satu server mati dan membuat server lain menjadi terbebani.

- Hunt for resource leaks.

    Kebanyakan reaksi berantai terjadi karena aplikasi ada memori leak.

- Hunt for obscure timing bugs.

    Race Condition yang tidak jelas juga disebabkan oleh trafik

- Use Autoscaling.

    Lo harus membuat health check untuk setiap grup autoscaling. Scaler akan mematikan instance kalo ada kegagal dihealth cheknya dan memulai instance baru. Kalo scaler lebih cepat dari reaksi berantai, service lo bakal tetap aman.

#### **Cascading Failures**

Keagalan cascading 

**Remember This**

- Stop cracks from jumping the gap.
- Scrutinize resource pools.
- Defend with Timeouts and Circuit Breaker.

#### **Users**

**Remember This**

- Users consume memory.
- Users do weird, random things.
- Malicious users are out there.
- Users will gang up on you.

#### **Blocked Threads**


**Remember This**

- Recall that the Blocked Threads antipattern is the proximate cause of most failures.
- Scrutinize resource pools.
- Use proven primitives.
- Defend with Timeouts.
- Beware the code you cannot see.

#### Self-Denial Attacks

**Remember This**

- Keep the lines of communication open.
- Protect shared resources.
- Expect rapid redistribution of any cool or valuable offer.

#### Scaling Effects

**Remember This**

- Examine production versus QA environments to spot Scaling Effects.
- Watch out for point-to-point communication.
- Watch out for shared resources.

#### Unbalanced Capacities

**Remember This**

- Examine server and thread counts.
- Observe near Scaling Effects and users.
- Virtualize QA and scale it up.
- Stress both sides of the interface.

#### Dogpile

**Remember This**

- Dogpiles force you to spend too much to handle peak demand.
- Use random clock slew to diffuse the demand.
- Use increasing backoff times to avoid pulsing.

#### Force Multiplier

**Remember This**

- Ask for help before causing havoc.
- Beware of lag time and momentum.
- Beware of illusions and superstitions.

#### Slow Responses

**Remember This**

- Slow Responses trigger Cascading Failures.
- For websites, Slow Responses cause more traffic.
- Consider Fail Fast.
- Hunt for memory leaks or resource contention.

#### Unbounded Result Sets

**Remember This**

- Use realistic data volumes.
- Paginate at the front end.
- Don’t rely on the data producers.
- Put limits into other application-level protocols.

#### Wrapping up



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


<!-- Image -->
[the-butterfly]: https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781680504552/files/images/stability/butterfly.png

[the-spider-1]: https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781680504552/files/images/stability/spiderweb_orderly.png

[the-spider-2]: https://learning.oreilly.com/api/v2/epubs/urn:orm:book:9781680504552/files/images/stability/spiderweb_disorderly.png