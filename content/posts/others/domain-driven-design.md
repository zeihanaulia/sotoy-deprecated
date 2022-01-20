+++
title = "Domain-Driven Design"
categories = [
    "concept",
]
tags = [
    "software architehture",
    "Domain-Driven Design",
    "ddd",
    "presentation-script"
]
date = "2022-01-12"
draft = false
+++

## Introduce

<!-- Slide buku domain driven design  -->

Domain-Driven Design atau DDD adalah proses arsitektural yang cukup penting, baik itu untuk microservice dan agile.
Ini adalah cara yang bagus untuk menghasilkan arsitektur yang sangat modular dan dapat tumbuh secara bertahap seiring dengan perkembangan sistem.

<!-- Slide buku event stoming dan foto praktek  -->
Pada kesempatan ini, kita akan mendefinisikan DDD dan mengenali event storming, yang merupakan salah satu cara paling efektif untuk mengembangkan Domain-Driven Design.

<!-- Slide gambar domain store dan warehouse  -->
Kita akan membahas konsep-konsep utama seperti Bounded-Context, entity, dan agregat.
Kita akan melihat bagaimana event-based sistem bekerja, dan yang terpenting, 
nanti mungkin gw akan mencoba mendemonstrasikan proses event storming kepada kalian 
sehingga kalian dapat melihat bagaimana sebuah desain benar-benar menyatu.
Kita gak bisa agile kalau arsitektur sistem yang kita buat, tidak tahan terhadap tekanan perubahan yang konstan.
DDD, topik kali ini, adalah solusi yang bagus untuk masalah itu.

## Apa itu DDD?

<!-- Slide judul -->
Jadi, Apa itu DDD? kita coba dalami detailnya.

<!-- Slide foto eric evans dan bukunya -->
Domain-Driven Design didevelop oleh eric evans pada tahun 2003 dimana saat itu istilah agile masih asing karena agile manifesto baru ada di tahun 2001 
dan istilah microservice  belum ada karena istilah ini baru ada ditahun 2011 atau 2012.

Microservice membawa konsep Domain-Driven Design ini dikenal kembali, karena ternyata ini adalah cara yang hampir ideal untuk mendesain microservice.
Meskipun aslinya, konsep ini ditujukan untuk monolitik bukan microservice.
Artinya teknik DDD ini bisa berkerja dengan baik dimonolitik dan juga microservice.

Kita akan membahas sedikit tentang microservice nanti.

<!-- Slide buku eric evans dan buku vaugn vernon -->
Buku dari eric evans adalah buku yang bagus, 
tapi sekarang, sudah ada penulis lain yang menulis implementasinya 
dan intisarinya agar lebih mudah kita pahami dan lebih ringkas.
Kalian bisa baca buku dari Vaughn Vernon yang judulnya Domain-Driven Design Distiled.  
Silahkan baca untuk lebih mendalami, karena kita gak bisa mengcover semua materi dibuku itu saat ini.

Nah, sekarang apa itu Domain-Driven Design?

DDD memiliki beberapa karakteristik.

<!-- Slide Kutipan agile manifesto -->

Yang pertama adalah proses kolaborasi.

"Orang bisnis dan developer berkerja bersama". dikutip dari agile manifesto poin ke 4

Jadi, dari karakteristiknya saja sudah sangat - sangat agile dan Filosofi yang mendasari dari DDD dan agile ada kemiripan.

<!-- Slide permodelan domain driven design -->

Yang kedua adalah Domain-Driven Desain dibangun diatas permodelan.
Ide dasarnya adalah bahwa struktur kode kalian harus dimodelkan atau dipetakan ke struktur domain yang sedang dipecahkan.

Domain itu contohnya seperti module Accounting atau Order Prosesing.
Atau apapun yang dibutuhkan oleh orang bisnis.
Idenya adalah bahwa orang bisnis harus bisa melihat keseluruhan struktur besar arsitektur sistem kita dan memahaminya.

Itu artinya ketika membuat perubahan disuatu tempat, 
mudah juga untuk membuat perubahan ditempat lain.

Dan itulah salah satu keuntungan dari pendekatan DDD. 
Dengan kata lain, perubahan terjadi di domain bukan di kode. 
Domain berubah, kode berubah. 

Jadi ketika itu terjadi, kode yang kita buat bisa menyetarakan perubahan dengan mudah dan secepat mungkin.

Kalau ada mapping antara kode dan domain, ketika terjadi perubahan didomain, kita bisa tau persis dimana harus mencari perubahan itu.

<!-- Slide A domain-driven design architechture is designed to grow incrementally over time -->

Karakteristik ketiga adalah ia bersifat inkrimental.

Ide dari pendekatan inkrimental ini adalah lo gak perlu membuat arsitektur besar didepan.
Melainkan, lo datang dengan arsitektur yang cukup untuk memecahkan masalah yang ada didepan lo saat ini.
Kemudian di implementasikan, lalu belajar banyak tentang masalahnya.
Setelah itu lo harus kembali dan mengubah arsitektur untuk mengakomodasi hal baru apapun yang lo pelajari.

Dan itu adalah cara kerja agile. iterasi, feedback dan learning.
DDD dirancang dengan pemikiran itu.

Ide dasarnya adalah bahwa arsitektur Domain-Driven Design akan memungkinkan kode lo tumbuh secara bertahap, 
karena arsitektur itu sendiri tumbuh secara bertahap seiring waktu.

Jadi pendekatan DDD ini cocok untuk pengembangan inkrimental.

<!-- Slide domain mempengaruhi system -->

Domain akan mempengaruhi sistem.
Dan kita semua sangat menyadari itu.
Kita membuat sistem karena permintaan dari user.
Tapi ini bukan proses yang linier.

Benar, domain akan mempengaruhi sistem, tapi sistem juga bisa mempengaruhi domain.

Dengan kata lain, DDD dirancang untuk dunia dimana lo dapat merilis software secara bertahap.
Jadi artinya setiap kali kita merilis domain, user akan menggunakan kode yang baru saja kita rilis dan itu akan mengubah domain sampai batas tertentu.

<!-- Slide siklus DDD application life cycle -->

Jadi siklusnya seperti ini.
Kita menulis code atau mendevelop sesuatu.
Lalu kita deliver ke user.
User menggunakan kode itu.
Dan itu mengubah cara mereka berkerja, yang akan menyebabkan perubahan lain didalam sistem itu sendiri.

Jadi sekali lagi DDD sangat tepat untuk pekerjaan siklus semacam itu karena apa yang kita modelkan adalah domain yang sebenarnya ada di depan kita saat ini.

Dan perubahan mudah dilakukan karena ada hubungan dan pemahaman yang sama antara domain dan sistem yang mendasarinya.

## Bagaimana DDD bisa cocok dengan Agile?

<!-- Slide Judul -->

Jadi, bagaimana sih DDD bisa cocok dengan Agile?

DDD tentu bisa berkerja sangat baik dengan agile.

Alasannya karena, lingkungan kerja yang kolaboratif.

<!-- Slide Kutipan agile manifesto Business people and developers work together daily "throughout the project" -->

Jika lo melihat Agile manifesto dan prinsip-prinsipnya

"Orang-orang bisnis dan developer bekerja bersama setiap hari "sepanjang proyek." 

Dengan kata lain, Lo tidak dapat melakukan desain ala DDD jika lo tidak memiliki orang bisnis di dalam ruangan.

Itu merupakan proses kolaboratif, dan Lo akan menemukan, sebagai seorang arsitek, 
bahwa orang-orang bisnis akan berkontribusi lebih banyak pada desain kode Lo daripada orang-orang teknis.

Karena ingat, jika Lo menganalisis domain, Lo menganalisis kodenya.

Struktur domain adalah struktur kode.

Jadi, gagasan pelaku bisnis tentang cara kerja domain, memberi tahu Lo dengan tepat bagaimana kode harus bekerja.

Agile didasarkan pada iterasi yang disebut iterasi inspect dan adapt.

<!-- Slide inspect and adapt loop -->

Ide dasarnya adalah kita membuat perubahan kecil, lalu kita rilis.

kita evaluasi apa yang telah kita lakukan, biasanya dengan berbicara dengan end-user, 

dan kemudian kita mengimprove berdasarkan feedback yang kita dapatkan.

Proses siklus seperti ini benar-benar Agile, karena semua hal lain yang dibicarakan orang ketika mereka berbicara tentang Agile, 
seperti Scrum dan sebagainya, sama sekali tidak penting.

Karena yang terpenting adalah untuk menjadi agile, lo harus mendapatkan sedikit peningkatan ke user lo secepat mungkin, 
lalu menapatkan feedback, lalu menggunakan feedback untuk meningkatkan apa saja yang baru lo rilis sebelumnya.

Selama prosesnya sepert itu, itu adalah agile. gak peduli tim lu ada atau gak ada scrum master atau PO atau seremoni seremoni lainnya.

Yang penting adalah Lo merilis kode dengan cepat 
dan Lo mengubah apa yang Lo lakukan berdasarkan Feedback yang Lo dapatkan dari kode yang Lo rilis.

<!-- Slide Stories in Agile -->

Sekarang untuk menjaga sistem itu tetap koheren.
dan Agar dapat memiliki sesuatu yang berharga untuk dirilis kepada pengguna Lo,
Lo harus menggunakan apa yang disebut dalam pengembangan Agile, sebuah cerita atau stories.

Oke, sekarang kita ngomongin tentang stories sebentar.

Pertama, stories itu adalah sebuah narasi.

Stories itu bukan kata kata khusus yang rumit. Ya hanya cerita dengan makna sebenarnya.

Stories harus berupa narasi dan agar lebih mudah stories-nya harus pendek.
Satu atau dua kalimat cukup lah.

Tidak ada patokan, tapi bayangin aja kalo misal storynya gak bisa ditulis di post it, nah itu tandanya udah kepanjangan.

Jadi pendek itu penting.

Apa yang ditulis pada stories itu, adalah penggambaran user kita dan bukan pekerjaan kita, 
sekali lagi, harus menggambarkan pekerjaan user kita.
Dan mereka melakukan itu untuk mendapatkan hasil yang mereka anggap berharga.

Jadi, sebuah stories adalah hal yang teknis dalam arti tertentu.
karena itu adalah cerita yang menggambarkan User lo bekerja atau menggunakan sistem lo 
untuk mendapatkan hasil yang baik bagi mereka.

Atau dengan kata lain, cerita atau story menggambarkan karya user kita, dan bukan milik kita.

Sekarang intinya adalah bahwa story harus memberikan nilai atau value.

Dengan kata lain, jika cerita atau story mendefinisikan sesuatu yang dilakukan user lo untuk mencapai tempat yang berharga, 
maka ketika kita mengimplementasikan cerita itu, 
Kita akan menulis secuil kode yang melakukan sesuatu yang berguna bagi end user kita, 
yang membuat mereka mengerti dan mencapai ke tempat yang berharga itu.

Jadi kesimpulannya, 
Agile dan DDD berjalan sangat baik bersama-sama.

Jadi selanjutnya adalah kita perlu berbicara tentang bagaimana kita akan mengimplementasikan DDD, 
dan itu akan membawa kita ke topik microservice.

## Microservice

<!-- Slide Judul Microservice -->

Sekarang kita beralih ke implemntasi arsitektur

Saat ini, microservice merupakan cara paling umum untuk mengimplementasikan DDD.

Tapi gak harus ya, bisa juga untuk mengimplementasikan arsitektur DDD di dalam monolit besar, misalnya.

Tapi nanti monolith yang lo buat, akan diatur seperti microservice, modular perdomain domain gitu.

<!-- Slide big monolith to microservice https://s3.amazonaws.com/rails-camp-tutorials/blog/monolith-vs-microservice-rails-applications/microservice-1.jpg -->

Jadi mari kita ulas sedikit dan lihat apa sebenarnya microservice itu.

Microservice memecahkan masalah tertentu dan masalah itu adalah monolit, program raksasa yang memiliki segalanya di dalamnya.

Biasanya ada banyak baris kode mungkin bisa 500.000 atau satu juta baris kode.

Mereka sangat rumit dan sulit untuk dihadapi.

Dan kerumitan itu adalah masalah terbesar dari monolit.

<!-- Slide big ball of mud -->

Ada banyak koneksi internal dan banyak dependency didalam monolit yang membuatnya hampir mustahil untuk dikerjakan ketika sudah membesar.

Ide dari microservice adalah memecah monolit dan membuatnya dapat diatur, untuk menyelesaikan beberapa masalah yang terkait dengannya.

Jadi masalahnya bukan hanya kode yang besar, sulit untuk dikerjakan dan tidak diatur dengan benar.
Ada masalah lain yaitu ia cenderung fokus pada data.

<!-- Slide Break all system -->

Dan karena itu, ketika model data berubah, monolit dan kodenya juga harus berubah, dan itu sangat sulit.

Akibatnya, monolit sangat, sangat sulit untuk diperbarui karena semua ada ketergantungan internal.

Masalah selanjutnya,

<!-- Slide Deployment -->

Terkadang butuh waktu berhari-hari untuk mendeploy monolit.

Dan jika kita berada di dunia Agile dimana kita membuat perubahan kecil dan merilisnya ke user secara bertahap, 
Lo bisa menghabiskan waktu berhari-hari hanya untuk melakukan deployment.

Akibatnya, monolith sulit untuk dipertahankan.

Jadi agility benar-benar tidak mungkin di dunia seperti ini karena semuanya memakan waktu terlalu lama.

Jika kita menempatkan ini dalam hal bisnis, masalah terbesar dari monolit adalah membakar uang.

Karena banyak waktu yang terbuang untuk mendevelop suatu fitur.

## Advandate Microservice
<!-- 
Slide Keuntungan Miroservice 
    1. Kecil
    2. Bisa di Deploy secara independen
    3. Menyembunyikan implementasi detail
    4. Dimodelkan berdasarkan konsep bisnis
    5. Desentralisasi
 -->

Microservice memecah monolit menjadi serangkaian service kecil yang berbicara satu sama lain di seluruh network.

Gw gak bilang ini akan mudah, kita tidak bisa hanya mengambil monolit dan memecahnya menjadi beberapa bagian dan menyebutnya microservice.

Microservice harus dirancang dengan cara yang sangat spesifik.

DDD hampir merupakan cara yang ideal untuk merancang microservice.

Jadi, karakteristik apa yang dimiliki Microservice?

### Small

Pertama-tama, Microservice sangat kecil.

Seberapa kecil?

Aturan umumnya adalah bahwa Microservice harus sebesar kepala Lo.

Yang dimaksud adalah Lo harus tau segala sesuatu tentang service itu tanpa lo harus mencarinya.

Dalam praktiknya, jika Lo menyukai OOP, Lo dapat menganggap microservice seukuran class.
Dimana masuk kedalam satu file yang mungkin hanya berisi ratusan baris code.

### Bisa di Deploy secara independen

Kedua, dapat dideploy atau digunakan secara independen.

Jadi, artinya Lo harus bisa mendeploy suatu service, tanpa perlu merestart seluruhnya.
Dan ini adalah suatu keuntungan dibandingkan monolitik.

Intinya, kalau lo gak perlu independen deployment, ya gak perlu ,microservice sih.

Menerapkan Microservice bisa sangat rumit.

Jadi, independen deployment memungkinkan kita untuk memperbarui sistem kita saat sedang berjalan, saat sedang live.

Kalau lo gak butuh, Lo tetap bisa mempertimbangkan pendekatan Domain-Driven Design, 
untuk diimplementasikan pada monolith besar.

### Menyembunyikan implementasi detail

Ketiga, menyembunyikan detail.

Karena nantinya agar tetap dapat digunakan secara independen, detail implementasi harus disembunyikan dari bagian sistem lainnya.

Dengan kata lain, 

kalau Lo punya service A, tahu cara kerja service B, jika service B berubah, Lo gak mau kan si service A ikut berubah?.

Jika service A atau B tidak menyembunyikan detail implementasinya, maka Kita tidak akan memiliki kemampuan decoupling itu.

Jadi dalam Microservice mereka harus saling menyembunyikan implementasi.

Dan itu akan terjadi dengan segala jenis sistem Domain-Driven Design, baik menggunakan Microservice atau tidak.

### Dimodelkan berdasarkan konsep bisnis

Keempat,

Semua Microservice dimodelkan di sekitar konsep bisnis dan itulah yang dimaksud dengan Domain-Driven Design.

Jadi ada pemetaan bisnis langsung disana.

### Desetralisasi

Kelima,

Mereka terdesentralisasi. tetapi desentralisasi adalah masalah besar juga. Kita gak akan bahas masalah ini sekarang.

Faktanya, microservice akan tersebar dimana mana, kalau kita tidak bisa mengaturnya akan jadi masalah besar.

### Observability

Keenam,

Microservice harus dapat diamati, kita harus tau jika ada yang tidak beres.

### Autonomus

Dan terakhir,

Kita dapat melakukan perubahan pada satu service tanpa harus melakukan perubahan pada service lainnya.

Jadi, Microservice sangat cocok dengan dunia Domain-Driven Design 
dan alasan mengapa Microservice dan Domain-Driven Design sering berjalan bersama adalah 
karena Microservice hampir merupakan implementasi yang ideal untuk Domain-Driven Design. 

## Apa itu Context

<!-- Slide Apa itu Context -->

Sekarang kita masuk ke detail. 
Sebenarnya seperti apa Domain-Driven Design itu dan bagaimana strukturnya.

Saat lo berpikir dalam istilah DDD, 
Lo harus berpikir tentang mengatur kode disepanjang batas-batas tertentu yang terdefinisi dengan baik.

Hal pertama yang akan dibahas adalah bounded context

<!-- Slide Bounded Context -->

Bisa dilihat pada gambar dislide, yang dilingkari itu adalah bounded kontext.
Dan didalamnya terdiri aktifitas yang hanya terjadi pada konteks itu saja.

Jadi misalnya, 

<!-- Slide store dan warehouse -->

jika Lo membuat bookstore, store itu sendiri adalah konteksnya.

Tujuan dari konteks store adalah untuk menjual buku kepada orang-orang, 
tetapi ada konteks lain yang membuat bookstore berfungsi yaitu warehouse, tempat buku-buku disimpan dan akan dikirimkan.

Jadi sekarang kita punya dua konteks yang berbeda.

Salah satunya adalah store yang mana bertanggung jawab atas penjualan.

Dan warehouse yang bertanggung jawab dalam penyimpanan dan pengiriman.

Ini penting, untuk menjaga konteks tetap terpisah.
Pikirkan tanggung jawab apa yang dikerjakan oleh orang yang berkerja dimasing masing konteks.

Jika seseorang pergi bekerja ke warehouse, mereka tidak akan berpikir untuk menjual buku.
Dan sebaliknya, disisi store tidak perlu berpikir dirak mana bukunya berada dan gimana melakukan pengiriman.

Sekarang, tentu saja. untuk melakukan itu semua mereka harus berkomunikasi satu sama lain.

Dan komunikasinya sebagian besar berbicara dalam konteks masing masing.
Dan ini mengapa Domain-Driven Design ada dan menjadi prinsip desain yang baik.

Karena yang terjadi didalam konteks, tetap berada didalam konteks.

Akan salah jika kita memlilki object yang berada dikedua sisi berbicara satu sama lain tanpa memperhatikan batasn konteks.
Ini adalah hal yang harus kita hindari.

<!-- Slide orang yang berkomunkasi satu sama lain -->

Jadi, DDD memecahkan masalah ini dengan mengalokasikan tanggung jawab untuk berkomunikasi ke satu object kecil.
Seperti pada gambar di slide, harus ada satu orang disisi store yang tugasnya berbicara dengan satu orang sisi warehouse.

Dan ini juga cara yang biasanya terjadi di dunia nyata.

Gak mungkin kan kalo lo butuh sesuatu dari tim lain, misal tim finance, lo main comot aja orangnya dan lo suruh dia mengerjakan ditempat lo.
Pasti ada regulasinya ada proses yang harus lo lakukan ada channel khusus untuk memproses itu semua.

Dan inilah yang akan kita lakukan di dunia DDD, yaitu memodelkan gagasan tadi dalam kode.

Sekarang kita akan membahas sedikit terminologi disini.

<!-- Slide Entity
    - Seperti class didalam OOP
    - Punya satu perkerjaan dan mengerjakan satu hal
    - Dan dilakukan dalam spesifik konteks
 -->

Di dunia DDD ada yang namanya entitas atau entity
Entity mirip seperti sebuah kelas di dunia object oriented programming.
ia hanya memiliki satu tanggung jawab dan hanya melakukan satu hal saja.
Dan hal itu dilakukan dalam konteks yang spesifik.

Selain entitas ada juga agregate.

<!-- Slide Agregate -->

Agregat adalah kumpulan entitas yang dapat berkomunikasi dalam satu portal.
Dalam contoh kasus bookstore ini aggreagatenya adalah order.

Oke, jadi disini ada sedikit hal yang agak blur.
Agregate nanti akan terlihat sepeti entitas, terlihat seperti satu object yang melakukan satu pekerjaan.

Tapi nanti ketika sudah fokus ke code, baru keliatan bagaimana aggregate menyelesaikan tugasnya.

<!-- Slide Gambar Aggregate bahas tentang gambar -->
Kurang lebih aggregate seperti gambar dislide, dimana Mobil adalah aggregasi dari semua printilan yang embuat mobil berkerja

## Ubiquitous Language

<!-- Slide Ubiquitous Language -->

Sekarang, konsep berikutnya yang menarik adalah gagasan tentang Ubiquitous Language

Gagasan tentang Ubiquitous Language adalah bahwa dalam konteks tertentu,
orang-orang yang bekerja dalam konteks itu menggunakan bahasa mereka sendiri 
dan bahasa satu konteks berbeda dari bahasa dalam konteks lain. 

Jadi, misalnya, dalam konteks store tadi,
Apa hal-hal yang relevan dengan tanggung jawab orang-orang yang berkerja dalam konteks itu?

<!-- Slide Store Responsible
    - Sales
    - Display product
  -->

Jadi jika berpikir tentang buku, bahasa bahasa yang dibicarakan seperti
Penulisnya siapa, Review Pembaca, Berapa lama waktu membaca dan hal hal lain
yang membuat pembeli akan tertarik pada bukunya.

Nah kalau disisi gudang, bahasa yang dibicarakan akan berbeda.
Mereka tidak peduli membuat pembeli akan tertarik.
Mereka fokus pada pengiriman. 
Jadi bahasa yang mereka bicarakan seperti jumlah buku, berat buku, ukuran, dirak mana buku itu ada.
Dan hal hal lain yang menarik bagi orang orang yang berkerja dibagian warehouse.

Padahal barangnya sama, yaitu buku. tapi yang dibicarakan berbeda.
Tergantung pada konteks dimana lo berada.
Di sisi penjualan, lo mendisplay dan menjual buku.
Di sisi warehouse, lo mengambil buku dari rak, mengemas, dan mengirimnya.

Bahasanya berbeda.

Jadi, meformalkan Ubiquitous Language adalah bagian penting dari DDD.
Ide dasarnya adalah bahasa itu akan dipakai dalam kode.

Kata kata yang digunakan untuk menggambarkan buku akan berbeda dalam konteks warehouse maupun store.

Sebagai programmer atau arsiek, ini adalah bahasa yang akan lo gunakan saat lo coba berbicara pada hal yang sedang lo pecahkan.

Bahasa bahasa ini akan ada dalam kode itu sendiri.
Kita akan memberikan penamaan dalam kode kita berdasarkan Bahasa bahasa tadi.

Dan bahasa ini akan digunakan pada tingkat domain itu sendiri, untuk menggambarkan pekerjaan yang sedang dilakukan.
Jadi bahasa itu digunakan dimana mana, tapi dimana mana dalam konteks tertentu.

## Same name different entity

<!-- Slide nama sama tapi beda entitas -->

Sekarang kita lihat slide dan pikirkan dalam implementasi atau model data.

Jika berpikir tentang data, khususnya jika berpikir tentang cara tradisional membangun sistem, 
di mana Kita memikirkan model data terlebih dahulu,
Lo mungkin berpikir dalam hal normalisasi dan database relasional.
yang akan berakhir dengan gambar yang terlihat seperti ini.

Lo akan memiliki konteks yang berisi hal-hal yang jelas berada dalam konteks itu, 
tetapi di luar konteks,
Lo akan memiliki entitas yang independen,
yang entah bagaimana harus bekerja dengan berbagai konteks pada saat yang bersamaan.

Dalam dunia Domain-Driven Design, 
setiap entitas yang dikaitkan dengan beberapa konteks atau lainnya
adalah hal yang buruk.

Sekarang kembali ke masalah Ubiquitous Language, 
Lo akan ingat bahwa hal-hal seperti order, dan customer, 
dan sebagainya akan diperlakukan secara berbeda tergantung pada konteks tempat Anda berada.

Bahasa seputar order akan berbeda di sisi store dan disisi warehouse.

Jadi saat kita bergerak menuju struktur Domain-Driven Design, 
kita akan berakhir dengan sesuatu yang terlihat lebih seperti ini.

<!-- Slide domain driven design -->

Perhatikan bahwa konteks store dan konteks warehouse masing-masing memiliki ordernya sendiri dan order tersebut akan berbeda.

Sama dengan produk, konteks store dan warehouse memiliki produk yang berbeda.

Di sisi store, entitas produk akan memperhatikan hal-hal seperti image dan harga dan tentu saja nomor SKU.

Di sisi warehouse, juga akan ada SKU di sini.

Pasti akan ada sesuatu untuk mengikat mereka bersama-sama, 
tetapi pihak warehouse akan memperhatikan rak dan ukuran serta berat produk.

Dan di sisi warehouse, Lo gak terlalu peduli dengan gambar dan harganya.


Jadi sebagai konsekuensinya, ketika kita masuk ke pendekatan Domain-Driven Design,

kita akan beralih dari reational database yang berpikir untuk mencoba membuat komponen raksasa,
yang bisa digunakan di mana-mana.

<!-- Slide domain driven design harus -->

Di dunia Domain-Driven Design, 
memiliki satu objek yang dapat digunakan dalam berbagai konteks
adalah hal yang buruk.

Karena komponen produk itu akan menjadi terlalu besar.

Akan sulit untuk mempertahankannya.

Ini akan berisi banyak dependensi internal dan sebagainya yang tidak Anda inginkan.

Jadi, di dunia DDD akan ada dua entitas sama dengan konteks yang berbeda.

yang satu dalam konteks store, satu lagi dalam konteks warehouse

Dan jangan coba coba untuk menempatkan mereka bersama sama.

<!-- Slide everything stays context specific -->

kalau kita menerapkan desain DDD di microservice,
kita akan memiliki dua service produk yang berbeda.

Satu service produk akan berfokus pada warehouse,
produk lainnya akan berfokus pada store.

Kedua service tersebut akan memiliki database yang sama sekali berbeda.

Lo tidak akan memiliki satu tabel yang berisi semua yang harus dimiliki suatu produk di dalamnya.

Jadi semuanya tetap pada konteks masing masing.

## Orchestarated Declarative System

<!-- Slide Orchestarated Declarative System -->

Sekarang mari kita menelusuri lebih jauh dan melihat bagaimana entitas akan berkomunikasi satu sama lain saat mereka menyelesaikan pekerjaan.

Ada dua cara untuk melakukan komunikasi.

Satu mudah dan yang lainnya lebih baik.

Jadi mari kita mulai dengan mudah.

Cara mudah untuk melakukan sesuatu adalah  orkestrasi atau deklaratif sistem.

Ide dasarnya adalah bahwa satu entitas memberitahu entitas lain apa yang harus dilakukan.

<!-- Slide satu entitu memberitau -->

Jadi melihat contoh bookstore tadi, Lo dapat memiliki service yang mewakili
entitas individu, cart service, billing service,
warehouse service, email service dan lain lain.

Layanan tersebut di dunia deklaratif akan saling memberi tahu apa yang harus dilakukan.

<!-- Slide Sync Service -->

Cart serice memberitahu billing service untuk mengeluarkan invoice.

Lalu memberitahu warehouse service untuk mempersiapkan item untuk pengiriman.

Jadi, dalam rangkaian event yang normal, yang Anda harapkan adalah aliran komunikasi yang natural antar object.

Cara deklaratif seperti ini berkerja dengan baik pada monolitik.

Tapi mereka tidak dapat bekerja dengan baik di dalam microservice 
dan alasannya adalah garis biru itu, 
jalur komunikasi itu adalah jalur komunikasi di dunia microservice.

Dalam monolit, tentu saja, itu hanya panggilan fungsi.

Di microservice, Network connection dapat memiliki segala macam hal yang bikin jariang itu tidak berkerja dengan baik.

Jadi misalnya, apa yang terjadi jika Network connection antara cart service  dan warehouse service terputus?

Nah, cart service bisa direquest oleh pembeli, 
tapi kemudian cart service gagal mengakses ke  warehouse service.

Lalu apa yang dilakukannya pada saat itu, 
ya terserah Lo sebagai seorang programmer, tetapi yang pasti bakal rumit.

Saat Lo mempelajari tentang microservice,
Lo akan menghabiskan banyak waktu untuk mempelajari berbagai design pattern yang dapat Lo gunakan untuk memecahkan masalah khusus ini,
tapi masalahnya adalah ini masalah yang sulit.

Jadi jika masalah tadi terjadi,
nanti akan ada masalah dengan billing service.

Kan pada saat yang bersamaan Cart service mengatakan kepada billing service,
untuk mengeluarkan invoice,
Lalu billing mengeluarkan invoice.

jadi kita sekarang dalam situasi di mana invoice telah diterbitkan, 
tetapi kita tidak mengirimkan produk ke pembeli
dan itu jelas tidak dapat diterima.

Masalah lain yang terkait dengan jenis  deklaratif sistem ini adalah 
ada tight coupling antara berbagai service di sini.

Dengan kata lain, cart service perlu tahu sedikit tentang warehouse service untuk menggunakannya.

Intinya, jika Lo melakukan perubahan pada warehouse service, Lo juga harus mengubah cart service.

Dan hal yang sama berlaku untuk setiap downstream service ini.

Jika Lo membuat perubahan pada downstream service apa pun, 
kemungkinan besar upstream service akan terpengaruh.

Jadi, solusi untuk itu adalah topik kita selanjutnya.

## Choreography - Reeactive system

Cara lain untuk mengatur service kita adalah dengan menggunakan koreografi atau sistem reaktif.

Dalam arti tertentu koreografi adalah solusi untuk masalah yang baru saja kita bicarakan ketika kita melihat sistem deklaratif tadi.

Sekarang, di sinilah kita memulai, dengan sync service kami yang tahu tentang hal-hal yang ada di downstream.

Apa yang terjadi jika kita mengganti API deklaratif?

Dengan kata lain, 
sekarang alih-alih cart service memberi tahu billing service, dan warehouse service, dan email service apa yang harus dilakukan, 
cart service hanya mengumumkan kepada mereka bahwa order baru telah dibuat (OrderPlaced) 

<!-- Slide Async -->

billing service akan duduk manis menunggu pesan itu dibuat.

Dan ketika mendapatkan pesam order.placed

Dia akan mengatakan, wah ada pesanan masuk nih, oke langsung ane buatin invoicenya.

Begitu juga dengan warehouse service dan email service.

Mereka akan segera melakukan tugasnya masing masing.

Jadi pada titik ini kita telah memecahkan banyak masalah yang terkait dengan sistem deklaratif kami.

Secara khusus, kita telah menghilangkan tight coupling pada service downstream dan upstream.

Dengan kata lain, 

Lo dapat membuat perubahan sebanyak yang lo inginkan pada billing service, 
dan cart service tidak peduli.

Bahkan, cart service tidak mengetahui adanya billing service.

Yang dia tahu hanyalah mengirimkan event order.placed dan siapa saya yang mau silahkan proses.


Kita dapat mengubah cara kerja downstream service tanpa memengaruhi upstream service.

Dan pada saat yang sama kita dapat menambahkan downstream service baru dan upstream service tidak mempedulikannya.

Jadi ini adalah tempat yang jauh lebih baik.

Secara umum sebagian besar komunikasi yang terjadi di antara entitas harus dilakukan dengan menggunakan model koreografi atau reaktif.

Nah dari segi implementasi, yang ini model publish-subscribe.
Ide dasarnya adalah cart menerbitkan event, dan downstream service subscribe event itu.

Jadi subscriber tidak diketahui oleh publisher, dalam model pub-sub.

Dengan kata lain, cart hanya mengirimkan event ke service yang tertarik, dan tidak tahu siapa yang menerima.
 
cara terbaik untuk melakukannya dengan menggunakan messaging system.

<!-- Slid merk messaging system -->

messaging system seperti Nats, Kafka, ZeroMQ, dan RabbitMQ,

Kalau kita mau menerapkan sistem DDD semacam ini, gunakan model reaktif.

## Event-Storming

Jadi setelah melihat DDD secara strutural, Pertanyaan selanjutnya adalah.
Bagaimana kita membuatnya?
Bagaimana cara mendesainnya?

Teknik terbaik yang diketahui saat ini adalah EventStorming yang sedang didevelop oleh Alberto Brandolini.

<!-- Slide Eventstorming -->

Ini adalah gambar bukunya, bisa dibeli dileanpub.com

Ini adalah buku yang pasti layak dibeli dan dibaca.

Sekarang, event storming adalah teknik yang benar-benar digunakan untuk dua hal, 
tetapi seperti yang baru saja kita lihat dalam Domain-Driven Design, 
kedua hal itu adalah hal yang sama.

Kita dapat menggunakannya untuk menganalisis domain, menganalisis bisnis, 
dan juga untuk mengembangkan kode yang akan menjadi model bisnis karena dengan memodelkan bisnis, 
juga mendapatkan desain untuk kode yang kita buat nantinya.

Jadi, event storming kemudian adalah teknik kolaboratif yang Kita lakukan sebagai developer, 
bersama dengan orang bisnis untuk menghasilkan desain sistem yang memodelkan struktur 
dan aliran aktivitas dalam bisnis itu sendiri.

Stories juga penting disini.

Salah satu hal yang membuat orang salah jalan saat melakukan eventstorming adalah 
mereka mencoba dan melakukan terlalu banyak sekaligus.

Mereka mencoba dan memodelkan seluruh bookstore dalam satu sesi,
yang harusnya dilakukan adalah fokus pada stories tertentu 
dan kemudian memodelkan bagian sistem yang perlu kita modelkan untuk mengimplementasikan story itu.

Kemudiam, kalo kita berkerja dengan cara agile.
Kita akan langsung mengimplementasi.
Tapi setelah selesai.
Kita akan kembali dan memodifikasi arsitektur sebelumnya dan memasukan cerita tambahan kedalamnya

Jadi, kita sebagai developer akan diksusi bersama tim bisnis, 
debgan papan tulis dan banyak post it yang ditempel dan mulai memodelkan.

Sekarang, ada beberapa istilah yang harus kita diskusikan.

Yang pertama adalah Event

Jadi, event adalah sesuatu yang terjadi di tingkat bisnis yang dipedulikan oleh customer, 
atau enduser, atau domain expert kita.

Ini masalah tingkat bisnis.

Ingat apa yang kita lakukan dengan tingkat domain,

Domain-Driven Design, mengimplementasikan domain.

Jadi, perintah yang telah diajukan adalah suatu event,

<!-- Slide contoh event -->

Misalnya

Order telah dibuat, atau eventnya order submited,

Pembayaran telah diterima atau eventnya Payment Received,

Dan setiap event akan dinamakan dalam bentuk lampau atau past tense.

Event adalah sesuatu yang sudah terjadi, yang akan memicut hal lain yang terjadi selanjutnya.

Jadi membuat event dalam bentuk past tense adalah cara yang baik.

## Eventstorming Gameplay

<!-- Slide cara main event stormnig -->

Oke, sekarang bagaimana gameplay dari Eventstorming.
Anggep aja kaya lagi main boardgame ya.

Untuk alat, pastinya kita butuh papan yang luas dan juga sticky notes.
Atau kita bisa pakai miro untuk online.

Mainnya cukup mudah kita hanya perlu menempel event satu persatu 
dan kemudian diatur dari kiri kekanan.

Bisa lihat pada gambar ini.

<!-- Slide arrage event storming -->

Setiap warna ada artinya. Event yang perlu kita letakan diawal adalah yang berwarna orange.

<!-- Slide warna orange biru merah -->

Warna orange mengindikasikan event yang berada pada tingkat domain, atau bisnis event.
Dan penamaannya harus dalam past tense. 
Dan bagian ini juga mungkin akan menarik untuk dibuat codenya sama developer.

Selanjutnya adalah warna biru, warna biru mewakili Action atau activities atau command.

Maksudnya, ketika event yang berwarna orange telah terjadi akan menyebabkan suatu action.
Nah, action ini akan ditandai dengan warna biru.

Ketika kita belum tau apa yang akan teradi, kita akan membuat pertanyaan
Yang ditandai dengan warna merah.
Yang menarik nantinya, kita sudah mulai biasanya akan banyak pertanyaan pertanyaan yang belum bisa terjawab.
Dan mungkin akan kita diskusikan ulang, dan mungkin akan ada proses tambahan nantinya.
Selama ada warna merah dipapan, artinya kita belum siap untuk implementasi dalam kode.

Warna selanjutnya adalah warna ungu. Ini mewakili policy, atau kebijakan dari orang bisnis.
Sesuatu yang akan mengendalikan action.
Jadi ketika suatu event diterima, 
Kita dapat menerapkan kebijakan dan memutuskan apa yang harus dilakukan selanjutnya berdasarkan kebijakan itu.

Selanjutnya, warna kuning yang mewakili aktifitas dari manusia.
Dan itu artinya, ada kemungkinan event akan dihasilkan bukan dari proses event sebelumnya
melainkan dari aktivitas manusia.

Dan terakhir, warna pink yang mewakili external system.
mengindikasikan hal hal yang terjadi diluar dari sistem kita. Third party.

Nah itu tadi tahap pertama dalam bermain eventstorming.
Tahap kedua, kita merangkai event dan memutuskan aktifitas apa saja yang harus terjadi.
Tahap ketiga, kita memutuskan agregate atau konteks atau ada entitas apa saja yang bertanggung jawab untuk menerima event.

Tahap ketiga ini adalah tempat dimana ketidak jelasan antara aggregate dan event.

Perlu diingat kembali, aggregate merupakan kumpulan entitas yang bertindak sebagai satu kesatuan. tapi memiliki satu porta yang bisa digunakan untuk saling berkomunikasi.

Tahap terakhir, kita akan mengumpulkan semua event yang terkait dangan konteks tertentu
kedalam satu tempat sehingga akan lebih mudah untuk memprogram desain ini.

<!-- Slide hasil event storming -->

Jadi event storming memperlihatkan kita
Seluruh flow event yang ada pada sistem
Siapa yang akan menghandle event itu
Dan context apa yang mereka dapat.


## Demo Event

Selanjutnya kita coba demo lewat slide ini bagamana proses event storming.

Yang pertama kita perlu memutuskan event apa saja yang terjadi disini.

Orang bisnis lebih tau tentang semua ini, jadi biarkan mereka memutuskan apa saya event event yang akan terjadi.

<!-- Menjelaskan tiap tiap event -->

## Demo Activities flo

<!-- Slide demo activities flow 
menjelaskan
-->

## Demo activities flow 2

<!-- Slide demo activities flow 2
menjelaskan
-->

## Demo activities flow 2

<!-- Slide demo activities flow 3
menjelaskan
-->

## Demo entities

<!-- Slide demo entities
menjelaskan
-->

## Demo context map

<!-- Slide demo context map
menjelaskan
-->

## Terima kasih

- Software Architecture Superstream: Domain-Driven Design and Event-Driven Architecture
  - https://learning.oreilly.com/live-events/software-architecture-superstream-domain-driven-design-and-event-driven-architecture/0636920064974/0636920064973/
- Event-Driven Architectures Using EventStorming for Rapid Innovation
  - https://learning.oreilly.com/live-events/event-driven-architectures-using-eventstorming-for-rapid-innovation/0636920482888/0636920068621/
 - Modeling Cheatsheet
   - https://on24static.akamaized.net/event/33/30/22/5/rt/1/documents/resourceList1631812013318/slides.pdf
 - Software Architecture Superstream Series: Domain-Driven Design and Event-Driven Architecture
   - https://learning.oreilly.com/videos/software-architecture-superstream/0636920509301/
  
## Demo entity maps

<!-- Slide Entity Maps -->

Ada satu hal lain yang mungkin berguna, yaat kita berkerja dari entity map ke code.

Ada teknik yang disebut CRC Card.

CRC Card ini sudah lama dikembangkan oleh kent beck dan ward cunningham ketika mereka mengajar OOP.

Rebeca wirf brock melakukan banyak pekerjaan dengan CRC cart.
Dia sudah menulis buku tentang ini.

Ide dasar dari CRC card adalah untuk mencoba dan mengidentifikasi class class yang ada dalam sistem.
Tanggung jawab uang mereka miliki
dan kolaborator dengan siapa mereka berbicara.

Sekarang,