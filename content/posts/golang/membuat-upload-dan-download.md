+++
title = "Membuat upload dan download dengan golang"
categories = [
    "belajar",
]
tags = [
    "golang",
]
date = "2020-06-04"
+++

Ditulisan kali ini kita coba membuat program untuk melakukan upload dan download menggunakan golang.
Contoh kasus dalam dunia nyata seperti Upload foto, Import dan Export CSV dan lain lain.

## Membuat WebServer Sederhana

Karena fokusnya hanya ke upload dan downloadnya. Webservernya kita buat sederhana aja ya.
Cukup dengan membuat 2 file `main.go` dan `index.html`.

### main.go

```go
package main

import (
    "fmt"
    "html/template"
    "log"
    "net/http"
    "path"
)

func upload(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Upload")
}

func download(w http.ResponseWriter, r *http.Request) {
    fmt.Fprintf(w, "Download")
}

func index(w http.ResponseWriter, r *http.Request) {
    fp := path.Join("./", "index.html")
    tmpl, err := template.ParseFiles(fp)
    if err != nil {
        http.Error(w, err.Error(), http.StatusInternalServerError)
        return
    }

    if err := tmpl.Execute(w, nil); err != nil {
        http.Error(w, err.Error(), http.StatusInternalServerError)
        return
    }
}

func main() {
    http.HandleFunc("/upload", upload)
    http.HandleFunc("/download", download)
    http.HandleFunc("/", index)
    log.Printf("server listen on http://localhost:8080")
    log.Fatal(http.ListenAndServe(":8080", nil))
}

```

### index.html

```html
<!DOCTYPE html>
<html>  
    <head>
        <meta charset="UTF-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1.0" />
        <meta http-equiv="X-UA-Compatible" content="ie=edge" />
        <title>Document</title>
      </head>
      <body>
        <form
          enctype="multipart/form-data"
          action="http://localhost:8080/upload"
          method="post"
        >
          <input type="file" name="myFile" />
          <input type="submit" value="upload" />
        </form>
        <a href="http://localhost:8080/download" >Download</a>
      </body>
</html>
```

Sepertinya tidak perlu dibahas dengan detail.
Cukup jalankan saja kode diatas dengan `go run *.go`. Sehingga kita bisa mengakses `http://localhost:8080`.
Yang jika diakses akan menampilkan halaman upload dan download.

## Menambahkan Fitur Upload

Setelah webserver berjalan dengan baik. Kini saatnya memasukan fitur upload.
Tugas dari fungsi upload seperti ini:

1. Membaca file yang diupload dari request yang diterima
2. Membuat folder `temp`
3. Menulis file kedalam folder `temp`

```go
func upload(w http.ResponseWriter, r *http.Request) {
	// Baca file dari request
	file, header, err := r.FormFile("myFile")
	if err != nil {
		http.Error(w, err.Error(), http.StatusInternalServerError)
		return
	}
	defer file.Close()

	// Bikin folder
	folderPath := "./temp"
	if _, err := os.Stat(folderPath); os.IsNotExist(err) {
		_ = os.MkdirAll(folderPath, os.ModePerm)
	}

	// Tulis file kedalam folder temp
	name := filepath.Join("temp", header.Filename)
	temp, err := os.OpenFile(name, os.O_RDWR|os.O_CREATE|os.O_EXCL, 0600)
	if err != nil {
		http.Error(w, err.Error(), http.StatusInternalServerError)
		return
	}
	defer temp.Close()

	filesByte, err := ioutil.ReadAll(file)
	if err != nil {
		http.Error(w, err.Error(), http.StatusInternalServerError)
		return
	}

	_, _ = temp.Write(filesByte)

	fmt.Fprintf(w, "Uploading File")
}
```

### Membaca file yang diupload dari request yang diterima

Untuk membaca request dapat menggunakan `r.FormFile("myFile")`. FormFile akan mengembalikan stuct `multipart.File`, `*multipart.FileHeader`, dan Error.
multipart.File adalah interface dimana method didalamnya ada os.Read yang bisa dibaca oleh `ioutil.ReadAll(file)`.
*multipart.FileHeader adalah struct yang berisi data attribute didalam file seperti FileName, Header, Size dan Content. 
Dari attribute attribute ini kita dapat mengetahui jenis dari file yang diterima.

### Membuat folder `temp` (temporary/sementara)

Sebenarnya code pembuatan folder ini tidak wajib dilakukan, bisa saja kita arahkan file ke root aplikasi atau bikin manual foldernya dan arahkan.
Tapi biarlah ditulis, biar satu siklus sampai pembuatan folder. 
Pembuatan folde hanya perlu mengecek apakah folder ada atau engga dengan menggunakan fungsi `os.Stat(folderPath)` dan `os.IsNotExist(err)`.
Lalu jika tidak exist / tidak ada maka bikin folder dengan fungsi `os.MkdirAll(folderPath, os.ModePerm)`.
os.ModePerm adalah permision unix yang nilainya **0777**.

### Menulis file kedalam folder `temp`

Menulis file kedalam dimulai dari mendefinisikan path dan nama file berserta extensionnya.
Lalu coba dibuka izin untuk menulis `os.OpenFile(name, os.O_RDWR|os.O_CREATE|os.O_EXCL, 0600)`.
Misal,
Buka izin folder `temp` dengan nama `products.csv`, izin kan untuk `open the file read-write.` dan `create a new file if none exists.` dan `used with O_CREATE, file must not exist.` lalu kasih permission `[0600](http://www.filepermissions.com/directory-permission/0600)`.

Setelah itu baca fle dengan `ioutil.ReadAll(file)` yang fungsinya mentransfer file dalam bentuk byte.
Lalu ditulis dengan fungsi `temp.Write(filesByte)`.

## Membuat export Csv

Sebelum kita masuk ke proses download, mungkin kita coba, bagaimana sih membuat file dengan golang.
Bayangkan link download yang ada dihalam index kita adalah fungsing untuk mendownload laporan bertipe `csv`.
Tugas dari fungsi `exportCsv` seperti ini:

1. Menyiapkan dataset
2. Bikin folder
3. Bikin File
4. Tulis datanya

```go
func exportCsv() error {
	// Data set
	var data = [][]string{{"ID", "Name"}, {"1", "Zei"}, {"2", "Han"}}

	// Bikin folder
	folderPath := "./temp"
	if _, err := os.Stat(folderPath); os.IsNotExist(err) {
		_ = os.MkdirAll(folderPath, os.ModePerm)
	}

	// Bikin file
	file, err := os.Create("./temp/result.csv")
	if err != nil {
		return err
	}
	defer file.Close()

	// Tulis datanya
	writer := csv.NewWriter(file)
	defer writer.Flush()

	for _, value := range data {
		if err := writer.Write(value); err != nil {
			return err
		}
	}

	return nil
}
```

## Menambahkan Fitur Download

Dengan fungsi `exportCsv` diharapkan kita sudah dapat membuat file.
File akan berenama `result.csv` yang dimasukan kedalam folder `temp`.
Tugas dari fungsi `download` seperti ini:

1. Panggil fungsi `exportCsv`
2. Membuka file dimana hasil export csv berada
3. Membuat attachment dengan header
4. Copy file dari file ke `http.ResponseWriter`

```go
func download(w http.ResponseWriter, r *http.Request) {
	// Bikin file untuk didownload
	if err := exportCsv(); err != nil {
		http.Error(w, err.Error(), http.StatusInternalServerError)
		return
	}

	// Open file
	f, err := os.Open("./temp/result.csv")
	if err != nil {
		http.Error(w, err.Error(), http.StatusInternalServerError)
		return
	}
	defer f.Close()

	// Bikin file terdownload
	cd := fmt.Sprintf("attachment; filename=%s", f.Name())
	w.Header().Set("Content-Disposition", cd)
	if _, err := io.Copy(w, f); err != nil {
		http.Error(w, err.Error(), http.StatusInternalServerError)
		return
	}
}
```

Selesai.

Referensi:

- [Uploading Files in Go - Tutorial](https://tutorialedge.net/golang/go-file-upload-tutorial/)