+++
title = "Mengenal Context pada Golang"
categories = [
    "belajar",
]
tags = [
    "golang",
]
date = "2020-05-20"
+++

## Apa itu Context
 
Golang [context](https://golang.org/pkg/context/) adalah standard libray muncul pertama kali pada versi [1.7](https://golang.org/doc/go1.7#context).
Tapi, sebenarnya package ini sudah ada dari dulu, ada di [golang.org/x/net/context](https://pkg.go.dev/golang.org/x/net/context?tab=doc). 
Kita dapat menggunakan context untuk melakukan `cancelation`, `deadlines` dan `passing value` ke standard library lainnya,
seperti [net](https://golang.org/doc/go1.7#net), [net/http](https://golang.org/doc/go1.7#net_http), and [os/exec](https://golang.org/doc/go1.7#os_exec).

### Apa sih yang dimaksud dengan **cancelation**?

>Analogi dari cancelation kurang lebih seperti ini. Misalnya, Andi meminta dibuatkan Bakso kepada Budi untuk acaranya. 
>Budi segera mencari daging dan bahan-bahan lain di pasar. Untuk memperecepat Budi meminta tolong Cici untuk membeli perlatan makan plastik. 
>Ditengah perjalanan Andi berubah pikiran dan membatalkan permintaannya. Budi langsung membatalkan semua belanjaannya.
>Begitu juga dengan Cici.

Cerita diatas adalah contoh dari `context cancelation` dan `cancelation propagation` karena permintaan Budi kepada Cici juga dibatalkan.

### Apa sih yang dimaksud dengan **deadlines**?

Deadline atau umumnya kita sebut "timeout". Cancelation berdasarkan waktu, Jika kita menambahkan dari analogi diatas, Menjadi:

>Analogi dari deadline kurang lebih seperti ini. Misalnya, Andi meminta dibuatkan Bakso kepada Budi untuk acaranya **dalam waktu 5 detik**. 
>Budi segera mencari daging dan bahan-bahan lain di pasar. Untuk memperecepat Budi meminta tolong Cici untuk membeli perlatan makan plastik. 
>Karena sudah lewat 5 detik secara otomatis Andi membatalkan permintaannya. Budi langsung membatalkan semua belanjaannya.
>Begitu juga dengan Cici.

Jika sebelumnya `cancelation` dapat dicancel melaui trigger Andi secara manual, dengan deadlines Andi bisa menentukan waktu dari tugas yang dikerjakan Budi dan Cici.

### Kalau yang dimaksud **passing value**?

Kita dapat menyisipkan value kedalam context dan function yang dilewati context bisa mendapat value tersebut. 
Detailnya nanti akan kita bahas di [Bagaimana cara membuat value dan dipassing ke function](#bagaimana-cara-membuat-value-dan-dipassing-ke-function)

Jadi, kode dari package context ini sebenernya tidak banyak kita bisa membaca semua codenya. Tapi intinya, context terdiri dari 2 tipe yaitu [CancelFunc](https://golang.org/pkg/context/#CancelFunc) dan [Context](https://golang.org/pkg/context/#Context). `CancleFunc` diexport hanya untuk tujuan dokumentasi, gak perlu kita bahas dulu lah si `CancelFunc` ini. 
Yang harus diperhatikan adalah tipe `Context`. Package context memiliki 3 method yang diperuntukan untuk `cancelation` dan `propagation`, dan 1 method yang diperuntukan untuk `passing value` atau `value propagation`.

| Cancelation    	| Value       	|
|----------------	|-------------	|
| WithCancel()   	| WithValue() 	|
| WithDeadline() 	|             	|
| WithTimeout()  	|             	|

Akar utama dari penggunaan context dengan mendefinisikan `context.Background()`. Background mengembalikan non-nil, empty context. Tidak pernah dicancel, tidak ada value, tidak ada deadline. Digunakan untuk main function, initialization dam sebagai top level context untuk request yang masuk.

Selain background ada juga `context.TODO()`, TODO, juga mengembalikan non-nil, empty context. TODO() biasanya digunakan ketika ada ketidakjelasan context mana yang akan digunakan atau belum tersedia.

Terus apa bedanya `context.Background()` dan `context.TODO()`?. Kalau dilihat dari source codenya keduanya sama aja [go/src/context/context.go](https://github.com/golang/go/blob/master/src/context/context.go#L199-L218)

Ya, Perbedaannya cuma ada dinamanya aja sih, TODO() dimaksudkan kedepannya akan dikasih context. Meskipun contextnya belum dikasih apapun. Jadi biar enak kalo kemarin lupa mana nih context yang kemarin mau dipake, tapi belum? tinggal search aja `context.TODO()` kalo search `context.Background()` pasti terlalu banyak.

## Bagaimana cara menggunakan context cancelation

```go
package main

import (
	"context"
	"fmt"
	"time"
)

func main() {
	ctx := context.Background()
	say(ctx, 5*time.Second, "hello")
}

func say(ctx context.Context, duration time.Duration, msg string) {
	time.Sleep(duration)
	fmt.Println(msg)
}
```

Kode ini akan menampilkan text `hello` setelah 5 detik. Karena ada `time.Sleep(duration)` untuk mensimulasikan program yang memproses lama. 

Sebelum masuk ke `deadlines` kita coba simulasikan kode diatas. yang apa bila setelah program berjalan kita cancel contextnya dengan cara, 
ditrigger dari membaca ketikan keyboard. Jadi kalo mengetik `enter` setelah program berjalan sebelum 5 detik, maka akan tercancel contextnya.

```go
package main

import (
	"bufio"
	"context"
	"fmt"
	"log"
	"os"
	"time"
)

func main() {
	ctx := context.Background()
	ctx, cancel := context.WithCancel(ctx)

	go func() {
		s := bufio.NewScanner(os.Stdin)
		s.Scan()
		cancel()
	}()

	say(ctx, 5*time.Second, "hello")
}

func say(ctx context.Context, duration time.Duration, msg string) {
	select {
	case <-time.After(duration):
		fmt.Println(msg)
	case <-ctx.Done():
		log.Print(ctx.Err())
	}
}
```

Kita jalankan `go run *.go`, lalu ketik enter, maka akan muncul tulisan:

```bash
➜  go-context git:(master) ✗ go run *.go

2020/05/26 13:44:29 context canceled
```

## Bagaimana cara membuat value dan dipassing ke function

<!-- TODO -->

## Bagaimana mendefine function yang menerima value

<!-- TODO -->

## Bagaiman context membuat HTTP request lebih efficient

<!-- TODO -->


## Referensi

- [Package context](https://golang.org/pkg/context/)
- [justforfunc #9: The Context Package](https://www.youtube.com/watch?v=LSzR0VEraWw)