+++
title = "Membuat upload dan download dengan golang graphql"
categories = [
    "belajar",
]
tags = [
    "golang",
]
date = "2020-06-05"
+++

Sama seperti tulisan [Membuat upload dan download dengan golang](https://sotoy.onrender/posts/golang/membuat-upload-dan-download.md) hanya kali ini kita membuatnya dengan graphql. Apakah bisa juga?
Contoh kasus dalam dunia nyata seperti Upload foto, Import dan Export CSV dan lain lain.

## Membuat Graphql Seerever Sederhana

Membuat graphql pada golang cukup mudah, kita bisa menggunakan library [gqlgen](https://gqlgen.com/). Instalasinya juga to the point bisa dibaca di [Getting Started](https://gqlgen.com/getting-started/).

### Intsall GQLGen

```bash
go get github.com/99designs/gqlgen
```

### Inisialisasi kerangka kerja gqlgen

```bash
go run github.com/99designs/gqlgen init
```

### Bikin Schema

```graphql
type File {
  id: ID!
  text: String!
}


type Query {
  file: [File!]!
}

input UploadInput {
  text: String!
}

type Mutation {
  singleUpload(input: UploadInput!): File!
}
```

### Generate ulang schemanya

Ini adalah proses penulisan ulang kode resolver caranya tambahkan **//go:generate go run github.com/99designs/gqlgen** pada file resolver.go, letakan paling atas.

```go
//go:generate go run github.com/99designs/gqlgen

package graph

// This file will not be regenerated automatically.
//
// It serves as dependency injection for your app, add any dependencies you require here.

type Resolver struct{}

```

lalu run

```bash
go generate ./...
```

untuk menjalakannya cukup dengan meruning file `server.go`

```bash
go run *.go
```

Sampai sini graphl server sudah run, kita sudah bisa test mutation query tapi karena belum diimplement kode apapun maka akan error.

```graphql
mutation UploadFile {
  singleUpload(input:{
    text: "adasd",
  }){
    id
    text
  }
}
```

## Menambahkan Fitur Upload

Tugas dari fungsi upload seperti ini:

1. Membaca file yang diupload dari request yang diterima
2. Membuat folder `temp`
3. Menulis file kedalam folder `temp`

Untuk membuat fitur upload, kita perlu mengubah schema. 

```graphql
scalar Upload

type File {
  id: ID!
  text: String!
}

type Query {
  files: [File!]!
}

type Mutation {
  singleUpload(file: Upload!): File!
}

```

Perubahan terdapat pada argumen muatation, `singleUpload(file: Upload!): File!`.
Kita menggunakan scalar Upload. Lalu generate ulang. Sehingga menghasilkan code seperti ini.

```go
func (r *mutationResolver) SingleUpload(ctx context.Context, file graphql.Upload) (*model.File, error) {
	folderPath := "./temp"
	if _, err := os.Stat(folderPath); os.IsNotExist(err) {
		_ = os.MkdirAll(folderPath, os.ModePerm)
	}

	// Tulis file kedalam folder temp
	name := filepath.Join("temp", file.Filename)
	temp, err := os.OpenFile(name, os.O_RDWR|os.O_CREATE|os.O_EXCL, 0600)
	if err != nil {
		return &model.File{}, err
	}
	defer temp.Close()

	filesByte, err := ioutil.ReadAll(file.File)
	if err != nil {
		return &model.File{}, err
	}

	_, _ = temp.Write(filesByte)

	return &model.File{}, nil
}
```

Scalar Upload akan menhasilkan `file graphql.Upload`. yang isinya adalah struct berisi `io.Reader` sehingga file bisa kita baca.

Kita test, asumsi kita menjalakan curl dibawah berada diroot project dan coba upload file server.go:

```bash
curl localhost:8080/query \
  -F operations='{ "query": "mutation ($file: Upload!) { singleUpload(file: $file) { id } }", "variables": { "file": null } }' \
  -F map='{ "0": ["variables.file"] }' \
  -F 0=@server.go
```

Selesai.

Referensi:

- [File Upload](https://github.com/99designs/gqlgen/blob/master/docs/content/reference/file-upload.md)
- [GraphQL multipart request specification](https://github.com/jaydenseric/graphql-multipart-request-spec)