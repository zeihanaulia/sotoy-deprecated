+++
title = "Mencoba Rust System Programming Language"
categories = [
    "belajar",
]
tags = [
    "rust",
]
date = "2020-04-05"
+++

Sudah dari tahun 2018 kami sudah mendengar rust, terutama ketika AWS me-"Open Source" `FireCraker - Engine Lambda`, 
rust menjadi salah satu bahasa yang digadang-gadang saingan go.
Ya meski tidak ada saingan diantara bahasa pemrograman, karena itu cuma alat bantu untuk menyelesaikan masalah.
Baru ditahun ini ingin mencoba Rust, Karena ada kejenuhan dan ingin mencoba sesuatu yang baru.

Sekilas baca buku `Programming Rust, 2nd Edition`, bagian `Why Rust?`. Yang kami tangkap disana itu Rust adalah

> a safe, concurrent language with the performance of C and C++.

Rust adalah bahasa yang dikembangkan oleh Mozilla dan komunitasnya. Ia adalah system programming baru.
System programming itu untuk, Sistem Operasi, FileSystem, Database, Bisa jalan di device ya`ng murah, dll.
Intinya sistem programming adalah`resource-constrained programming` artinya pemrograman yang benar-benar
memperhatikan setiap byte dan CPU.

## Kenalan dulu

### Instalasi Rust

Instalasinya cukup mudah, hanya perlu googling sekali dan langsung ditujukan ke [https://doc.rust-lang.org/book/ch01-01-installation.html](https://doc.rust-lang.org/book/ch01-01-installation.html).

### Mencoba Rust

Setelah sukses melakukan instalasi, Kita bisa mencoba, Bukan hanya rust, instalasi diatas juga memberikan `Cargo` sebagai `package manager`.
Apa saja yang cargo lakukan.

```bash Cek versi dari cargo
cargo --version

# cargo 1.42.0 (86334295e 2020-01-31)
```

Versi Cargo diatas terpisah dengan versi rust.

```bash
cargo new --bin myproject
```

Dengan syntax diatas, Cargo akan membuatkan folder yang berisi layout dari project.

```bash
- myproject
-- Cargo.toml
-- src
```

`Cargo.toml` berisi metadata dari project kita

```toml Cargo.toml
[package]
name = "myproject"
version = "0.1.0"
authors = ["Zeihan Aulia <zeihan@warungpintar.co>"]
edition = "2018"

# See more keys and their definitions at https://doc.rust-lang.org/cargo/reference/manifest.html

[dependencies]

```

Disana ada 2 bagian yaitu `[package]` dan `[dependencies]`. Package adalah metadata dari project.
Dependencies adalah list library dependency yang kita gunakan.

Didalam folder src, terdapat `main.rs`. didalamnya tertulis kode hello world.
Selanjutnya kita build project tersebut dengan menggunakan syntax

```bash Cargo Build
cargo build

# Compiling myproject v0.1.0 (/Users/zeihanaulia/Programming/research/rust-learning/myproject)
# Finished dev [unoptimized + debuginfo] target(s) in 1.62s
```

Jika build berhasil, maka kita akan mendapatkan folder baru yaitu `target` yang didalamnya berisi folder `debug`.
Yang didalamnya terdapat binary dari hasil build kita.

```bash
./target/debug/myproject
# Hello, world!
```

Kalau kita ingin melakukan analisis dari `runtime performance` dari kode kita. Pastikan kita melakukan build dengan

```bash
cargo build --release
# Compiling myproject v0.1.0 (/Users/zeihanaulia/Programming/research/rust-learning/myproject)
# Finished release [optimized] target(s) in 0.92s
```

dan mejalankannya

```bash
./target/release/myproject
# Hello, world!
```

Pada phase development dari pada menggunakan build, kita bisa menggunakan 

```bash
cargo run
```

### Variable

Variable di rust sama seperti variable pada umumnya dibahasa pemrograman lain yang mana variable dapat meletakan isi disebuah wadah.

#### Gimana sih cara declare variable?

```rust
let x = 5;
```

Variable dimulai dengan kata kunci `let` dan dilanjutkan dengan `x` sebagai nama dari variable.
Selanjutya isi vairable `x` dengan menggunakan `=` lalu nilainya `5`, jangan lupa ditutup dengan semicolon `;`.

```rust
fn main() {
    let x = 5;
    let y = 6;
    let z = x + y ;
    println!("z is {}", z)
}
```

Coba [disini](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=2ea5f1704cb696d7c4942b391df853f2).

#### Rust itu defaultnya mutable loh.

Perbedaan variable rust dibanding yang bahasa lain adalah mutable by default.

```rust
fn main() {
    let x = 5;
    x += 1;
    println!("x is now {}", x);
}
```

hasilnya

```bash
Compiling playground v0.0.1 (/playground)
error[E0384]: cannot assign twice to immutable variable `x`
 --> src/main.rs:3:5
  |
2 |     let x = 5;
  |         -
  |         |
  |         first assignment to `x`
  |         help: make this binding mutable: `mut x`
3 |     x += 1;
  |     ^^^^^^ cannot assign twice to immutable variable

error: aborting due to previous error

For more information about this error, try `rustc --explain E0384`.
error: could not compile `playground`.

To learn more, run the command again with --verbose.
```

Coba [disini](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=888e3987d64934cd8c92805969742a2c).

Kita tidak diizinkan untuk mengubah value dari dari `x`. 
Jadi untuk mengubah nilai dari suatu variable harus menggunakan kata kunci tambahan `mut`.

```rust
fn main() {
    let mut x = 5;
    x += 1;
    println!("x is now {}", x);
}
```

Coba [disini](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=5954b4dd29405f9d8f6c4c5cdd0a89fe).

#### Semua variable bisa memiliki type

```rust
fn main() {
    let mut x: i32 = 5;
}
```

dengan menambahkan colon atau titik dua kita bisa memberikan tipe pada suatu variable.

### Data Types

Data types di rust dibagi menjadi 2 bagian. `Simple` dan `Compound`.

#### Single

- Bool
- Integer
- Floating point
- Charecter

##### Boolean

Boolean disebut `bool`. Isinya hanya `true` atau `false`. 
Boolean biasanya digunakan pada kondisi kontrol flow seperti `if` dan while.

```rust
fn main() {
    let a = true;
    let b = false;

    if a {
        println!("a is true!")
    }

    if b {
        println!("b is false!")
    }
}
```

##### Integer

Number tanpa decimal disebut dengan interger. Ada dua type Signed dan Unsigned. 
Sebagai tambahan kita dapat memutuskan berapa banyak space integer yang akan digunakan.

| Signed 	| Unsigned 	|
|--------	|----------	|
| i8     	| u8       	|
| i16    	| u16      	|
| i32    	| u32      	|
| i64    	| u64      	|

##### Floating

Number dengan decimal point.

##### Character

#### Compund

##### Tuple

Tuple menungkinkan kita mengelompokan beberapa nilai secara bersamaan dalam `()`.
Nilai didalamnya tidak harus dengan nilai yang sama.

```rust
// structuring
let tup = (1, 'c', true);

// descructuring

ley (x, y, z) = tup;
```

##### Array

Array adalah koleksi dimana semua elemen memiliki tipe yang sama.

```rust
fn main() {
    let a = [0.0, 3.14, -8.8722]
    let second = a[1];
    println!("{}", second)
}
```

bisa juga kita ganti nilai didalam array, dengan menambah kata kunci `mut`

```rust
fn main() {
    let mut a = [0.0, 3.14, -8.8722]
    let a[0] = 1.1;
    println!("{}", second)
}
```

Tapi kita tidak bisa menambah atau mengurangi jumlah dari array

```rust
fn main() {
    let mut a = [0.0, 3.14, -8.8722]
    let a += 1.1;
    println!("{}", second)
}
```

##### Slices

Slice memberikan kita referesnsi sekumpulan data yang bedekatan dalam structur data lain.

```rust
    let a = [100,200, 300]
    let b = &a[0..1]
```

### Function

Sama seperti function di bahasa lain. 

```rust 
func name(param1: type1, ...) -> return_type {
    ...body...
}

name(param1: type, ...)
```

name dapat diisi dengan nama function dan bisa diisi dengan parameter. lalu diakhiri dengan result.

contoh function :

```rust
fn next_birthday(name: &str, current_age: u8) {
    let next_age = current_age + 1;
    println!("Hi {}, on your next birthdat, yuu'll be {}!", name, next_age)
}

fn main() {
    next_birthday("Jake", 33);
    next_birthday("Vivian", 0);
}
```

Coba [disini](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=9045f12057d41257915c026dc644c21e).

Function dengan return value

```rust
fn square(num: i32) -> i32 {
    num * num
}

fn main() {
    println!("The answer is {}", square(3));
}
```

Coba [disini](https://play.rust-lang.org/?version=stable&mode=debug&edition=2018&gist=3c3eda06e8b25ef802514ee4bc33e108)

Uniknya return value bisa saja tanpa menggunakan kata kunci `return` dan tanpa diakhiri semicolon `;`.
Tapi kalau mau pake return juga bisa, dan harus diakhiri dengan semicolon.


## Sumber Bacaan:

- [Programming Rust, 2nd Edition](https://learning.oreilly.com/library/view/programming-rust-2nd/9781492052586/)
- [Rust in Motion](https://learning.oreilly.com/videos/rust-in-motion/10000MNLV201742)
- [The Rust Programming Language](https://doc.rust-lang.org/nightly/book/title-page.html)
