+++
title = "Mengenal Kotlin"
categories = [
    "belajar",
    "kotlin",
]
tags = [
    "mobile development",
]
date = "2020-08-03"
draft = false
+++

Kotlin adalah bahasa pemrograman yang powerfull yang dibuat oleh [JetBrains](https://www.jetbrains.com/).
[Statically type](https://android.jlelse.eu/magic-lies-here-statically-typed-vs-dynamically-typed-languages-d151c7f95e2b), penggunaan secara umum dan open source. Mengkombinasikan fitur OOP dan Functional Programming.

## Menginstal Kotlin

Mengintal kotlin sangat mudah,

Kalau di Mac OS bisa menggunakan homebrew:

```bash
brew install kotlin
```

Jika sudah selesai kita coba cek versi dan mainkan REPL-nya dengan menggunakan [compile kotlin](https://kotlinlang.org/docs/reference/compiler-reference.html#kotlinjvm-compiler-options).


```bash
kotlinc -version
> info: kotlinc-jvm 1.3.72 (JRE 11.0.2+9-LTS)
```

## Mencoba kotlin

Masuk kedalam REPL shell, REPL adalah prototyping tools, jadi kalo misal sudah ditengah-tengah ngoding terus mau cobain fungsi kecilnya, dari pada test semua code mending cobain di REPL ini aja.

```bash
kotlinc

> Welcome to Kotlin version 1.3.72 (JRE 11.0.2+9-LTS)
> Type :help for help, :quit for quit
>>>
```

Kita coba melakukan simple progamming

```bash
>>> 1+2
> res0: kotlin.Int = 3

>>> val list = listOf(1,2,3)
>>> list.map{it*2}
```

Bagaimana cara esekusi kode didalam file? ya kita bikin file aja misal `Hello.kt` lalu isi dengan kode simple hello world.

```kotlin
fun main() = println("Hello World!")
```

Di REPL bisa dipanggil menggunakan fungsi load (asumsi REPL diesekusi difolder yang sama dengan filenya).

```bash
>>> :load Hello.kt
>>> main()
> Hello World!
```

Kita buktikan apakah benar kotlin bisa menggunakan kungsi java? Kita bikin file lagi namanya `testjava.kts`

```kotlin
java.io.File(".")
    .walk()
    .filter{file -> file.extension == "kts"}
    .forEach{println(it)}
```

kode diatas mengoprasikan program mencari file dengan extensi `kts`.

```bash
>>> :load testjava.kts
./testjava.kts
```

Menjalankan progam diluar REPL.

1. Kompile code kedalam `.jar` file
2. Execute `.jar`

```bash
kotlinc Hello.kt -d Hello.jar
java -jar Hello.jar
```

Atau bisa juga kalo kita mau jalankan langsung tanpa compile dan running terpisah.

```bash
kotlinc -script testjava.kts
```

`-script` bisa dijalankan hanya untuk file extensi `.kts` tidak bisa untuk `.kt`

## Kompile ke target lain

Kotlin bisa dikompile ke beberapa target seperti

- [Device Android](https://kotlinlang.org/docs/reference/android-overview.html)
- [Javascipt](https://kotlinlang.org/docs/reference/js-overview.html)
- Native dengan [kotlin/native](https://kotlinlang.org/docs/reference/native-overview.html)
- [Server Side](https://kotlinlang.org/docs/reference/server-overview.html)
- [WebAsembly](https://superkotlin.com/kotlin-and-webassembly/)