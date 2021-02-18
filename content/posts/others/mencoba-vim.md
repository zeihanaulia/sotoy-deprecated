+++
title = "Mencoba terbiasa dengan VIM"
categories = [
    "belajar",
]
tags = [
    "vim",
]
date = "2020-08-11"
draft = true
+++

## Install dan Setting Vim

## Bermain dengan vim

- Untuk megerakan cursor bisa menggunakan:
   h(kiri)	j(bawah) k(atas) l(kanan)
- Untuk membuka editor vim bisa dengan menggunakan `vi` [enter]
- Untuk keluar dari vim bisa menggunakan
	- <ESC> :q! keluar dengan menghapus semua perubahan
    - <ESC> :wq keluar dengan menyimpan perubahan
- Dalam kondisi normal mode kita dapat menghapus karakter dengan mengetik `x` pada karakter yang mau dihapus
- Untuk menulis text bisa menggunakan
    - `i` untuk insert sebelum cursor
    - `a` untuk append setelah cursor
- Menghapus karakter
	- `dw` menghapus karakter setelah cursor
	- `d$` menghapus semua karakter setelah cursor
	- `dd` menghapus seluruh baris
- Memindahkan cursor dengan cepat
	- `2w` memindahkan cursor ke dua kata setelahnya ([number] [motion])
		- misal ada kata `belajar vim seru` jika ketik 2w dengan cursor dihuruf `b` pada kata belajar maka kursor akan pindah ke kata `seru`
		- angka 2 bisa diganti dengan angka apapun, misal 11
- Mengkombinasikan operator number dan motion.
	- `d2w` hapus 2 kata mulai dari curso
		- `coba hapus kata ini` kalu kita ketik `d2w` mulai dari huruf `c` pada kata `coba`, maka akan menghapus 2 kata `coba hapus`
	- `d2$` dengan sintak ini, kita bisa menghapus 2 baris mulai dari curs
- Undo bisa dengan mengetik `u` pada normal mode
- Undo sampai akhir bisa menggunakan capital u `U`
- Redo bisa dengan mengetik `CRTL - r`, ditahan ya CTRL nya
- Menghapus beberapa baris 
	- `dd` menhapus 1 baris
    - `7dd` menhapus 2 baris
- Copy paste perbaris bisa menggunakan `dd` dan paste menggunakan `p`i
- Gunakan `r` untuk mengubah karakter pada kursor
	- `tipo` pindahkan kursor ke huruh `i` lalu pencet `r` 
- Menghapus 1 kata untuk mengganti dengan kata lain
	- `cibi pihitikin ini silih` kita bisa ke huruf `c` pada kata `cibi` lalu ketik ce untuk menghapus kata `cibi` lalu kita ketikan `coba`
	- `ce` menghapus kata dan masuk kedalam insert mode
- Untuk pindah ke baris paling bawah dengan menggunakan `gg`
- Untuk pindah ke baris paling atas dengan menggunakan `G`
- Untuk mencari kata mulai dari atas gunakan `/`
- Untuk mencari kata mulai dari bawah gunakan `?`
- Untuk mencari kata yang sama berulang ulang gunakan `n`
- Untuk mencari kata yang sama berulang sebaliknya gunakan `N`
- Untuk kembali ke kata dimana pencarian pertama kali ditemukan gunakan `CRTL + o`
- Untuk pindah dari `[]`, `{}`, `()` bisa gunakan `%`
	- Lihat kode dibawah kalau kita mau pindah ke penutup kurung kurawal bisa menggunakan `%`
```kotlin
fun hello(){
	println("hello")
}

```
- Memengganti suatu kata
	- `:s/old/new` mengganti satu kata pada 1 baris
	- `:s/old/new/g` mengganti semua kata `old` dengan `new` pada 1 baris
	- `:s/old/new/gc` mengganti semua kata `old` dengan `new` pada 1 baris dengan konfirmasi
	- `%s/old/new/g` mengganti semua kata `old` dengan `new` pada 1 file

Next Lesson 5.1
