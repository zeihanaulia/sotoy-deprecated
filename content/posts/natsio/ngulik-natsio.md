+++
title = "Ngulik NATS io"
categories = [
    "belajar",
]
tags = [
    "natsio",
]
date = "2020-04-30"
+++

Di dunia software engineering jargon microservice terdengar keren. Dilingkaran software engineer gw, mereka berlomba lomba untuk mengimplementasi microservice dengan harapan service yang dibuatnya menjadi lebih "scalable" kaya engineer engineer di silicon valley gitu gitu lah.
Salah satu topik yang seru untuk dibahas adalah messaging system dan merek yang tengah bersinar itu salah satunya [nats.io](https://nats.io/).

Kalau mau tau ceritanya tentang nats itu apa, bisa baca di dokumentasinya [https://docs.nats.io/](https://docs.nats.io/). Dibagian introduction sudah dijelaskan secara jelas. Masalah apa saja yang dapat diselesaikan oleh nats.

> Btw, Emang kenapa sih harus pake messaging system? Emang kalo REST API biasa kenapa? gak boleh kah?

Mengutip dari web [solace](https://solace.com/blog/experience-awesomeness-event-driven-microservices/). Salah satu pertimbangannya tidak menggunakan REST API adalah Syncronous, komunikasi antar service secara Syncronous bisa saja, tapi jika balikan dari API terlalu lama, akan menyebabkan kegagalan.

Untuk menyelesaikan masalah itu maka ditemukanlah messaging system yang keuntungannya.

1. Loose Coupeling, dimana messaging bertugas hanya untuk melakukan publish dan subscribe message, tidak ada bisnis logic disana, dan bisa dipublish kebeberapa domain bisnis secara terpisah.
2. Nonblocking, Messaging system juga bisa melakukan tugas asycronous yang mana system bisa melakukan proses dari banyak request tanpa menunggu response.
3. Sederhana untuk discale. 
4. Lebih tahan dan error handling yang lebih baik.

## Menginstal Nats

Cara dibawah ini menggunakan docker dan seharusnya docker sudah terinstal dengan benar. 

Jalankan:

```bash
docker run --rm -p 4222:4222 -p 8222:8222 -p 6222:6222 --name nats-server -ti nats:latest
```

Lalu coba check ke  [http://localhost:8222/](http://localhost:8222/). Harusnya bisa liat halaman monitoring nats.

## Contoh Kasus

Jika nats sudah terinstall, kita akan coba bermain main dengan nats.

### Subject-Based Messaging

Link: [https://docs.nats.io/nats-concepts/subjects](https://docs.nats.io/nats-concepts/subjects)

Pada dasarnya NATS itu tentang penerbitan dan menerima pesan. Penerbitan dan menerima pesan bergantung pada subjek yang dimasukan kedalam aliran atau topik.

#### Membuat Subscriber

Kode pertama yang kita tulis adalah subscriber, kode ini bertugas mendengarkan dari server nats apakah ada message masuk atau tidak.

```go
package main

import (
	"log"
	"runtime"

	"github.com/nats-io/nats.go"
)

func main() {
    subject := "contoh.subject"

	opts := []nats.Option{nats.Name("Contoh NATS Subsriber")}
	nc, err := nats.Connect(nats.DefaultURL, opts...)
	if err != nil {
		log.Fatal(err)
	}

	_, _ = nc.Subscribe(subject, func(msg *nats.Msg) {
		log.Printf(" Pesan diterima dari [%s]: '%s'", msg.Subject, string(msg.Data))
	})

	log.Printf("Mendengarkan pada subject [%s]", subject)
	runtime.Goexit()
}

```

Membuat koneksi ke NATS cukup mudah, dibawah ini cukup menggunakan 2 blok kode.
Kode dimulai dari membuat koneksi ke server NATS.

```go
opts := []nats.Option{nats.Name("Contoh NATS Subsriber")}
nc, err := nats.Connect(nats.DefaultURL, opts...)
if err != nil {
    log.Fatal(err)
}
```

fungsi `Connect()` membutuhkan 2 parameter, url dan options. Url diisikan nats.DefaultUrl, karena kita tidak mengubah settingan apapun pada server nats. Bentuk urlnya seperti ini `nats://127.0.0.1:4222` ini bisa diisi username dan password juga `nats://derek:pass@localhost:4222`. Untuk setting username dan password akan dibahas dicatatan berikutnya. Jangan lupa dicek errornya juga ya.

Lalu kode dilanjutkan dengan memanggil fungsi `Subscribe` yang isinya nama subject dan fungsi callback.

```go
_, _ = nc.Subscribe("my.topic", func(msg *nats.Msg) {
	log.Printf(" Pesan diterima dari [%s]: '%s'", msg.Subject, string(msg.Data))
})
```

Nama subject bisa menggunakan `wildcard` loh dengan mengganti dengan tanda (*). Sedangkan callback berfungsi untuk menangkap message dari server NATS.

Setelah program `subscriber` sudah dibuat, selanjutnya kita buat kode untuk melakukan publishernya. Oya jangan lupa dijalan kan dulu kodenya.

#### Membuat Publisher

Kurang lebih sama seperti subscriber cara mainnya.

```go
package main

import (
	"log"

	"github.com/nats-io/nats.go"
)

func main() {
	subject := "contoh.subject"
	msg := "hello abc"

	opts := []nats.Option{nats.Name("Contoh NATS Publisher")}
	nc, err := nats.Connect(nats.DefaultURL, opts...)
	if err != nil {
		log.Fatal(err)
	}
	defer nc.Close()

	err = nc.Publish(subject, []byte(msg))
	if err != nil {
		log.Fatal(err)
	}
	nc.Flush()

	if err := nc.LastError(); err != nil {
		log.Fatal(err)
	} else {
		log.Printf("Diterbitkan [%s] : '%s'\n", subject, msg)
	}
}

```

Blok kodenya pun tidak jauh berbeda. Tetap ada memmanggil fungsi `Connect()`, hanya saja bukan `Subscribe()` melaikan memanggil fungsi `Publish()`.
Fungsi `Publish()` terdiri dari 2 argumen, argument pertama untuk subject dan yang kedua untuk pesannya. Pesan dikirimkan dalam bentuk `[]byte`.

```go
err = nc.Publish(subject, []byte(msg))
if err != nil {
    log.Fatal(err)
}
```

Coba kita jalankan. Pertama jalankan program subsriber lalu program publisher. Seharusnya apa yang dikirimkan oleh publiser akan terbaca oleh subsriber.

```bash
2020/04/30 23:59:32  Pesan diterima dari [contoh.subject]: 'hello abc'
```

### Masalah masalah yang dihadapi

Raealitanya akan ada kemungkinan NATS server mati. Jika publisher yang mengakses tidak akan menjadi masalah, karena pasti akan dapat error. Akan tetapi apa yang akan terjadi pada subsriber?. Coba saja matikan servernya, lalu nyalakan kembali dan jalankan program publisher. Apa yang terjadi, Bahaya sekali bukan? Message yang dipublish tidak ada yang bisa diambil oleh subscriber. Kita akan banyak kehilangan data. Tapi tenang, NATS sudah ada solusinya.

#### Menambahkan options reconnectiing

```go
package main

import (
	"log"
	"runtime"
	"time"

	"github.com/nats-io/nats.go"
)

func main() {
	subject := "contoh.subject"
	totalWait := 10 * time.Minute
	reconnectDelay := time.Second

	opts := []nats.Option{nats.Name("Contoh NATS Subsriber")}
	opts = append(opts, nats.ReconnectWait(reconnectDelay))
	opts = append(opts, nats.MaxReconnects(int(totalWait/reconnectDelay)))
	opts = append(opts, nats.DisconnectErrHandler(func(nc *nats.Conn, err error) {
		log.Printf("Disconnected due to:%s, will attempt reconnects for %.0fm", err, totalWait.Minutes())
	}))
	opts = append(opts, nats.ReconnectHandler(func(nc *nats.Conn) {
		log.Printf("Reconnected [%s]", nc.ConnectedUrl())
	}))
	opts = append(opts, nats.ClosedHandler(func(nc *nats.Conn) {
		log.Fatalf("Exiting: %v", nc.LastError())
	}))

	nc, err := nats.Connect(nats.DefaultURL, opts...)
	if err != nil {
		log.Fatal(err)
	}

	_, _ = nc.Subscribe(subject, func(msg *nats.Msg) {
		log.Printf(" Pesan diterima dari [%s]: '%s'", msg.Subject, string(msg.Data))
	})
	nc.Flush()

	log.Printf("Mendengarkan pada subject [%s]", subject)
	runtime.Goexit()
}

```

Kode subscriber diatas ditambahkan beberapa options untuk melakukan reconnecting.

```go
opts = append(opts, nats.ReconnectWait(reconnectDelay))
```

`ReconnectWait(t time.Duration)` adalah fungsi yang mengeset waktu antara reconnect

```go
opts = append(opts, nats.MaxReconnects(int(totalWait/reconnectDelay)))
```

`MaxReconnects(max int)` adalah fungsi yang mengeset berapa kali maksimal usaha reconnecting berejalan

```go
opts = append(opts, nats.DisconnectErrHandler(func(nc *nats.Conn, err error) {
    log.Printf("Disconnected due to:%s, will attempt reconnects for %.0fm", err, totalWait.Minutes())
}))
```

`DisconnectErrHandler(cb ConnErrHandler)` ketika terjadi disconnect. kita bisa membuat log atau kode didalam handler.

```go
opts = append(opts, nats.ReconnectHandler(func(nc *nats.Conn) {
    log.Printf("Reconnected [%s]", nc.ConnectedUrl())
}))
```

`ReconnectHandler(cb ConnHandler)` handler ketika terjadi reconnect.

```go
opts = append(opts, nats.ClosedHandler(func(nc *nats.Conn) {
    log.Fatalf("Exiting: %v", nc.LastError())
}))
```

`ClosedHandler(cb ConnHandler)` ketika sudah mencapai maksimum usaha reconnecting sebaiknya diclose bisa si asumsikan server mati selamanya. diperlukan manusia untuk menyalakan.

Sampai disini, kita sudah bisa bikin pub/sub menggunakan nats, dan ketika server mati sementara NATS akan mengulang reconnecting beberapa kali.

Referensi:

- [REST vs Messaging for Microservices – Which One is Best?](https://solace.com/blog/experience-awesomeness-event-driven-microservices/)