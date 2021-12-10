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

## Menulis HTTP Server

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