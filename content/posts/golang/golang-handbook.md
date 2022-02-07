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
- [https://code.visualstudio.com/docs/languages/go](https://code.visualstudio.com/docs/languages/go)

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

## Golang Test

### Testing

Untuk testing biasa cukup dengan ini.

```bash
go test ./...
```

### Coverage

Untuk mendapatkan test coverage bisa dengan syntax ini.

```bash
 go test -covermode=count  -coverprofile=coverage.out ./...
 go tool cover -html=coverage.out -o coverage.html
```

## [Go migrate](https://github.com/golang-migrate/migrate)

Salah satu tools yang sangat membantu terutama pada saat proses development adalah go migrate.

Go migrate membantu kita untuk melakukan migrasi database cukup dengan mengetikan beberapa syntax.

### Install

```bash
go get -v -u  github.com/golang-migrate/migrate
migrate -v
```

### Generate Migration

```bash
migrate create -ext sql -dir db/migrations -seq create_users_table
```

Nanti akan menghasilkan file

```bash
1481574547_create_users_table.up.sql
1481574547_create_users_table.down.sql
```

### Migrate skema yang sudah ada

#### Buat database dan koneksikan ke gomigrate

```bash
export POSTGRESQL_URL='postgres://postgres:password@localhost:5432/example?sslmode=disable'
```


#### Lakukan Migrate

```bash
migrate -database POSTGRESQL_URL -path PATH_TO_YOUR_MIGRATIONS up
```

## [SQLC](https://sqlc.dev/)

Generator untuk membuat SQL Command, Ini sangat membantu untuk kita yang terbiasa menggunakan SQL dibanding orm.
sqlc akan mengenerate code berdasarkan kode sql yang kita buat

### Install

- GO VERSION >= 1.17

```bash
go install github.com/kyleconroy/sqlc/cmd/sqlc@latest
```

- DO VERSION < 1.17

```bash
go get github.com/kyleconroy/sqlc/cmd/sqlc
```
### Cara menggunakan

1. Buat sqlc config file `sqlc.yaml`
	
	```yaml
		version: 1
		packages:
		- name: "db"
			path: "db"
			queries: "./query/"
			schema: "../../../db/migrations/"
			engine: "postgresql"
			sql_package: "pgx/v4"
			emit_prepared_queries: true
			emit_interface: false
			emit_exact_table_names: false
			emit_empty_slices: false
			emit_json_tags: true
	```

2. Buat folder `query` dan isi query yang ingin kita generate dengan nama `author.sql`

	```bash
		-- name: GetAuthor :one
		SELECT * FROM authors
		WHERE id = $1 LIMIT 1;

		-- name: ListAuthors :many
		SELECT * FROM authors
		ORDER BY name;

		-- name: CreateAuthor :one
		INSERT INTO authors (
				name, bio
		) VALUES (
		$1, $2
		)
		RETURNING *;

		-- name: DeleteAuthor :exec
		DELETE FROM authors
		WHERE id = $1;
	```


3. Lalu generate

	```bash
		sqlc generate
	```

4. Kita akan mendapatkan beberapa file pada folder db, yaitu:
	- db.go
	- models.go
	- table_name

## [GORM Generator](https://github.com/go-gorm/gen)

Ketika kita menggunakan gorm, akan ada beberapa task yang membosankan misalnya ketika membuat models.
Dengan menggunakan gorm generator kita bisa mengenerate table menjadi models dengan cepat.

### Cara install

## Database on docker

### Run PostgreSQL

```bash
docker run --rm --name pg-docker -e POSTGRES_PASSWORD=postgres -d -p 5432 postgres
```

#### Buat Database

```bash
docker exec -it pg-docker psql -U postgres -c "CREATE DATABASE myservicedb ENCODING 'utf8' TEMPLATE template0 LC_COLLATE 'C' LC_CTYPE 'C';"
docker exec -it pg-docker psql -U postgres -c "GRANT ALL PRIVILEGES ON DATABASE postgres TO postgres;"
```

## Troubleshooting

### [pgtype](https://github.com/jackc/pgtype)

#### cannot encode status undefined

Ada issue dapet error karena status undefined, ini terjadi karena belum melakukan `Set` kepada variable tipe itu.

Contoh

- Negative Case

```go
	// postgre type numeric akan menggunakan type pgtype.Numeric
	var price pgtype.Numeric

	// ketika disave, akan return err `cannot encode status undefined`
	db.Save(&Order{
		ID: input.ID,
		Price: price
	})

```

- Fix

Ini terjadi karena kita belum set value untuk `price`

```go
	// postgre type numeric akan menggunakan type pgtype.Numeric
	var price pgtype.Numeric

	if err := price.Set(input.Price); err != nil {
		return err
	}

	// ketika disave, akan return err `cannot encode status undefined`
	db.Save(&Order{
		ID: input.ID,
		Price: price
	})
```