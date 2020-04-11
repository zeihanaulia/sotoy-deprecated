+++
title = "Membuat Aplikasi Pencatatan Keuangan dengan Golang dan GraphQL"
categories = [
    "tutorial",
]
tags = [
    "go",
    "golang",
    "graphql",
]
date = "2020-03-02"
+++

GraphQL di tahun 2020 sudah menjadi normal baru didunia pengembangan perangkat lunak.
Alat-alat atau pustaka untuk membangun GraphQL server sudah banyak sekali ada dibermacam-macam bahasa pemrogaman juga.
Di tutorial kali ini, saya akan coba untuk membangun aplikasi menggunakan GraphQL sebagai penyedia datanya.
Aplikasi yang akan dibangun adalah aplikasi pencatatan keuangan pribadi.

## Gqlgen

Gqlgen adalah salah satu pustaka yang dapat membantu kita membangun GraphQL Server.

## Memperpersiapkan Proyek


```bash
$ mkdir cawang
$ cd cawang
$ go mod init github.com/zeihanaulia/cawang
$ go get github.com/99designs/gqlgen
```

Buatlah folder dengan nama `cawang`. Cawang adalah nama dari aplikasinya singkatan dari `catat uang`.
Masuk kedalam folder `cawang`.
Lalu lakukan inisiasi untuk go modules karena aplikasi ini dibuat dengan bahasa go.
Dan terakhir kita unduh gqlgen dengan menggunakan `go get`

## Membangun GraphQL Server

```bash
$ go run github.com/99designs/gqlgen init
```

inisiasi gqlgen dan secara otomatis akan menghasilkan tata letak paket go.
file apa saja yang didapat?

- folder `graph` tempat dimana kode yang berhubungan dengan graphql diletakan
- gqlgen.yml file config yang dapat digunakan untuk pengaturan tata letak gqlgen
- server.go file yang digunakan untuk menjalankan server graphql

## Persiapan aplikasi

Untuk membangun aplikasi yang kita ingin kan, kita kosongkan isi dari folder graph
Untuk membuat pencatatan keuangan sederhana tipe diperlukan, Pengguna, Rekening dan Transaksi

**Pengguna** dapat membuat **Rekening** yang akan dicatat didalam **Transaksi**

Budi ingin mencatat keuangannya, yang dia ingin catat

- Uang masuk atau keluar? atau perpindahan rekening (misal dari saldo bank BCA ke GoPay)
- Jumlah nominal uang yang dikeluarkan
- Kategori Pengeluaran (Makanan, Transportasi, Belanja Bulanan, dll)
- Menggunakan sumber uang dari rekening mana? (Bank atau cash atau gopay)
- Tanggal dan jam transaksi terjadi
- Keterangan

Untuk tahap awal sepertinya cerita diatas cukup sederhana dan bisa dilakukan pengembangan.

Buat skema berdasarkan cerita diatas. Lalu jalankan kode, untuk mengasilkan file baru

```bash
$ go run github.com/99designs/gqlgen -v
```

Kita perlu mengubah sedikit pada aturan standar `gqlgen.yml`  dengan mendeklarasikan type mapping antara graphql dan go type system.
Tapi sebelum itu, kita pindahkan dulu model yang sudah dihasilkan ke file tersendiri. Kenapa?

Mekanisme dari menjalankan `go run github.com/99designs/gqlgen` adalah menghapus dan menghasilkan lagi `models_gen.go`. 
Sehingga ketika kita melakukan type mapping akan gagal karena modelnya tidak ada.

Sekarang sudah banyak nih resolvernya, ada `accountResolver`, `mutationResolver`, `queryResolver`, `transactionResolver` dan `userResolver`.

`mutationResolver` dan `queryResolver` adalah default dari GraphQL, bisa dibilang sebagai pintu masuknya. 
Jika ingin memutasi (create, update, delete) dapat menggunakan `mutationResolver`.
Jika ingin meminta darta (read) dapat menggunakan `queryResolver`.

Lalu `accountResolver`, `transactionResolver` dan `userResolver` ini apa? resolver ini yang akan mengeluarkan data dari type logika bisnis.
Seperti yang kita tau, aplikasi kita ada 3 domain yaitu `Account`, `Transaction` dan `User`. Karena pada skema mendefinikan relasi.
Maka kita dapat resolver tambahan yang mencerminkan logika bisnis.

```graphql
type Account {
    id: ID!
    type: TypeOfAccount!
    name: String!
    bank: String
    balance: Float!
    balanceMinimum: Float
    Description: String

    transactions: [Transaction]
    user: User!
}
```

Jika dilihat dari skema diatas, maka generator akan menghasilkan:

```go
func (r *accountResolver) Transactions(ctx context.Context, obj *model.Account) ([]*model.Transaction, error) {
	panic(fmt.Errorf("not implemented"))
}
func (r *accountResolver) User(ctx context.Context, obj *model.Account) (*model.User, error) {
	panic(fmt.Errorf("not implemented"))
}
func (r *Resolver) Account() generated.AccountResolver { return &accountResolver{r} }
type accountResolver struct{ *Resolver }
```