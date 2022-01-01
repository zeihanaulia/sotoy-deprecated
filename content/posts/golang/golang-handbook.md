+++
title = "Golang Handbook"
categories = [
    "golang",
]
tags = [
    "handbook",
    "snipets"
]
date = "2021-12-07"
draft = false
+++

Kumpulan snippet yang biasa digunakan dalam project menggunakan golang,

## Idiomatic

### Gunakan gofmt

[gofmt](https://pkg.go.dev/cmd/gofmt) adalah program untuk formating digolang.
Pastikan selalu ada disetiap code editor yang digunakan.

#### VSCode

Link: 
- [https://code.visualstudio.com/docs/languages/go(https://code.visualstudio.com/docs/languages/go)

#### Vim

Link:
- [https://github.com/fatih/vim-go/](https://github.com/fatih/vim-go/)

### Buat nama receiver tetap pendek

```go
type User struct {
	Name string
	IsActive bool
}

// gunakan
func (u User) Activated() {}

// jangan gunakan
func (user User) Activated() {}
func (self User) Activated() {}
func (this User) Activated() {}
```

Aktifkan [golangci-lint](https://github.com/golangci/golangci-lint) dengan [revive](https://revive.run/docs) untuk dapat pengecekan [receiver-naming](https://revive.run/r#receiver-naming).

### Memberikan nama package yang sesuai

Penamaan package sebaiknya sesuai dengan nama directory atau fungsi dari package tersebut.

- Fungsi: Implementasi elasticsearch ,Dir: /elasticsearch

```go
package elasticsearch
```

- Fungsi: Membuat order pesanan, Dir; /order

```go
package order
```

### Kelompokan import sesuai asalnya

Ada tiga bagian pada setiap import dan urutkan sesuai dengan asalnya, jangan urutkan sesuai abjad. Contoh

```go
import (
	// 1. Untuk standard library
	"context"

	// 2. Untuk external dependency yang digunakan
	"go.opentelemetry.io/otel"

	// 3. Untuk internal code yang kita atau tim kita tulis sendiri
	"github.com/zeihanaulia/my-project/internal"
)
```

### Gunakan nama pendek untuk scope yang terbatas

Nama pendek seperti 1 karakter huruf hanya boleh digunakan pada scope terbatas.
Misalnya seperti pada function for.

```go
for i:=0; i<5; i++ {}
```

sisanya berikan nama yang jelas sesuai maknanya.

### Context harus menjadi parameter pertama

Ada aturan pada penggunaan context:

1. Gunakan context sebagai parameter pertama pada function

	```go
	// Boleh
	func Create(ctx context.Context, args entity.Order) {}

	// Tidak boleh
	func Create(args entity.Order, ctx context.Context) {}
	```

2. Jangan simpan context sebagai field dalam struct 
   
	```go
	// Tidak boleh
	type User struct {
		ctx context.Context
		Name string
	}
	```

### Return diawal

Dalam setiap function yang mengembalikan nilai seharusnya return diawal.

```go
// Tidak boleh dilakukan
func ParseValue(value Value) (string, error) {
	if !value.MustBeTrue {
		return "", fmt.Errorf("must be true")
	} else {
		if value.Message == "" {
			return "", fmt.Errorf("value must be set")
		} else {
			if value.Priority == 0 {
				return "", fmt.Errorf("priority must be set")
			}

			return fmt.Sprintf("%d %s", value.Priority, value.Message), nil
		}
	}
}
```

Meskipun code diatas berjalan dengan benar, tapi secara bentuk tidak boleh dilakukan. 
Sebaiknya seperti ini.

```go
// Tidak boleh dilakukan
func ParseValue(value Value) (string, error) {
	if !value.MustBeTrue {
		return "", fmt.Errorf("must be true")
	} 

	if value.Message == "" {
		return "", fmt.Errorf("value must be set")
	} 

	if value.Priority == 0 {
		return "", fmt.Errorf("priority must be set")
	}

	return fmt.Sprintf("%d %s", value.Priority, value.Message), nil
}
```

#### Comments untuk mesin dan manusia
#### Hindari fungsi yang tidak perlu
#### Hindari fungsi init()
## Membuat HTTP Server

Ada banyak cara dalam membuat HTTP Server pada golang.
Kita bisa menggunakan standard libary golang `net/http` atau selain itu. seperti:

- chi
- mux
- gin
- dll

### Standard Library

```go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
	"time"
)

var (
	port            = flag.Int("port", 8080, "http port to listen on")
	shutdownTimeout = flag.Duration("shutdown-timeout", 30*time.Second,
		"shutdown timeout (5s,5m,5h) before connections are cancelled")
)

func main() {
	flag.Parse()

	http.HandleFunc("/info", func(w http.ResponseWriter, r *http.Request) {
		log.Printf("===================================================")
		log.Printf("Query-string: %s", r.URL.RawQuery)
		log.Printf("Path: %s", r.URL.Path)
		log.Printf("Method: %s", r.Method) // type method yang diterima 
		log.Printf("Host: %s", r.Host)

		for k, v := range r.Header { // baca header
			log.Printf("Header %s=%s", k, v)
		}

		if r.Body != nil { // baca body / payload data
			body, _ := ioutil.ReadAll(r.Body)
			log.Printf("Body: %s", string(body))
		}
		log.Printf("===================================================")

		w.WriteHeader(http.StatusAccepted)
	})

	port := 8080
	s := &http.Server{
		Addr:           fmt.Sprintf(":%d", port),
		ReadTimeout:    10 * time.Second,
		WriteTimeout:   10 * time.Second,
		MaxHeaderBytes: 1 << 20,
	}
	
	c := make(chan os.Signal, 1)
	signal.Notify(c, syscall.SIGINT, syscall.SIGTERM)

	go func() {
		log.Printf("Listening on port :%d\n", port)
		if err := s.ListenAndServe(); err != nil {
			if err != http.ErrServerClosed {
				log.Fatal(err)
			}
		}
	}()

	sig := <-c
	log.Printf("shutting down: %+v", sig)

	ctx, cancel := context.WithTimeout(context.Background(), *shutdownTimeout)
	defer cancel()

	if err := s.Shutdown(ctx); err != nil {
		log.Fatal(err)
	}
}
```

## Build golang menggunakan Docker

```Dockerfile
FROM golang:1.17 as build
WORKDIR /app
ADD . .
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -ldflags '-extldflags "-static"' -o main .
CMD ["go", "run", "main.go"]

FROM gcr.io/distroless/base-debian10 as production
COPY --from=build /app/main /
EXPOSE 8080

CMD ["/main"]
```

- `FROM golang:1.17 as build`
	
	build akan dialankan pada golang versi1.17, versi bisa disesuaikan dengan versi golang saat ini.

- `WORKDIR /app`

	Buat folder /app sebagai working directory 

- `ADD . .`

	copy source code kita dengan `ADD . .`

- `RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -ldflags '-extldflags "-static"' -o main .`

	build source code hingga menjadi binary dan filenya nanti akan bernama main

- `FROM gcr.io/distroless/base-debian10 as production`

	selanjutnya ambil image minimal menggunakan distroless base image.
	info selanjutnya bisa dibaca disini [https://github.com/GoogleContainerTools/distroless](https://github.com/GoogleContainerTools/distroless).

- `COPY --from=build /app/main /`

	lalu copy binary apliasi yang sudah dibuat sebelumnya.

- `COPY --from=build /app/main /`

	copy binary dari build stage ke root
  
- `EXPOSE 8080`

	expose 8080

- `CMD ["/main"]`

	runing command `/main`


## Setup golangci-lint

Linter sangat membantu untuk melakukan standarisasi code.
Mereka juga bisa mengecek jika ada kode yang tidak efektif atau bugs kecil.

Cara setupnya cukup mudah

- Windows & Linux

	untuk windows bisa gunakan git bash agar bisa menggunakan `curl`.

	```bash
		# binary will be $(go env GOPATH)/bin/golangci-lint
		curl -sSfL https://raw.githubusercontent.com/golangci/golangci-lint/master/install.sh | sh -s -- -b $(go env GOPATH)/bin v1.43.0

		golangci-lint --version
	```

- Mac

	```bash
		brew install golangci-lint
		brew upgrade golangci-lint
	```

Informasi lebih lanjut bisa cek di [https://golangci-lint.run/usage/install/](https://golangci-lint.run/usage/install/).

<!-- ###  Observability menggunakan [OpenTelemetry](https://opentelemetry.io/) -->

