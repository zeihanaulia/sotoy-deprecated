+++
title = "Belajar Algorithms"
categories = [
    "belajar",
]
tags = [
    "algoritmns",
]
date = "2020-09-21"
+++

## Kenapa harus belajar Algoritma dan data struktur? Apa untungnya buat saya?

Oke, Pertama-tama kita harus mengerti dulu apa yang dimaksud dengan Algoritma dan data struktur.
Algoritma adalah metore untuk menyelesaikan masalah,
Sedangkan Data Struktur adalah tempat dimana kita menyimpan informasi yang terkait dengan masalah.

Jika ditanya Kenapa saya harus belajar algortima. Ya jelas, karena dampaknya itu sangat luas.
Terlebih diera internet sekarang, semua serba menggunakan software. Pesan makan saja lewat software.
Untuk yang ingin atau sudah berkarir didunia programming. 
Belajar algoritma sangat menguntungkan, karena bisa digunakan untuk menyelesaikan masalah yang ingin diselesaikan dengan programming.
Belajar algoritma sangat menguntungkan, karena membantu melewati proses interview dibanyak perusahaan.
Belajar algoritma sangat menguntungkan, karena membantu menstimulasi intelektual kita.
Belajar algoritma sangat menguntungkan, karena menjadikan kita ahli programmer.
Belajar algoritma sangat menguntungkan, karena karena kita bisa tau asal usul dari kode yang ditulis.

>> I will, in fact, claim that the difference between a bad programmer
 and a good one is whether he considers his code or his data structures
 more important. Bad programmers worry about the code. Good
 programmers worry about data structures and their relationships. -- Linus Torvalds (creator of Linux)

## Kalo gitu apa yang perlu dipelajari?

Tulisan ini adalah hasil belajar dicoursera, semua akan dicatat disini. Nama kursusnya [https://www.coursera.org/learn/algorithms-part1/home/welcome](Algorithms Part 1). 

| topic      	| data structures and algorithms                     	|
|------------	|----------------------------------------------------	|
| data types 	| stack, queue, bag, union-find, priority queue      	|
| sorting    	| quicksort, mergesort, heapsort                     	|
| searching  	| BST, red-black BST, hash table                     	|
| graphs     	| BFS, DFS, Prim, Kruskal, Dijkstra                  	|
| strings    	| radix sorts, tries, KMP, regexps, data compression 	|
| advanced   	| B-tree, suffix array, maxflow                      	|

- [ ] Minggu pertama
  - [ ] Union Find
  - [ ] Analisis Algoritma
  - [ ] Programming Asignment
- [ ] TODO

Kode implementasi dikursus ditulis dengan menggunakan bahasa pemrograman java, dan disini akan ditulis ulang menggunakan bahasa go.

### Minggu Pertama

Persoalan yang akan dibahas diminggu pertama adalah tentang *Dynamic Connectivity Problem*.
Dalam membagun algoritma yang berguna ada beberapa langkah, yaitu:

- Model the problem
  - Coba untuk mengerti, masalah apa saja yang ingin diselesaikan.
- Temukan algoritma yang untuk menyelesaikannya
  - Apakah cukup cepat? Apakah cukup memorynya?
  - Jika tidak, Cari tau kenapa?
  - Temukan jalan untuk menyelesaikan masalah.
- Ulangi step diatas sampai puas

Oke, pertama-tama kan kita ingin menyelesaikan *Dynamic Connectivity Problem* (DCP). DCP ini adalah model dari problem untuk *Union Find*.
Idenya seperti ini,

Berikan sekumpulan dari N object:

    - Union Command: hubungkan 2 object `union(2,1)`
    - Find/Connected Query: apakah 2 object ini terkoneksi? `connected(0,7)`

```bash

(0)

(1) -> (2)

(5) -> (6)

(7)

(8) -> (3) -> (4) -> (9)
```

- Terjemahan hasil diatas, seperti ini.
- Hubungkan object 2 dengan 1 `union(2, 1)`!
- Hubungkan object 6 dengan 5 `union(6, 5)`!
- Hubungkan object 4 dengan 3 `union(4, 3)`
- Hubungkan object 3 dengan 8 `union(3, 8)`
- Hubungkan object 9 dengan 4 `union(9, 4)`

- Jika kita tanya Apakah 0 terkoneksi dengan 7 `connected(0,7)` ? Tidak.
- Jika kita tanya Apakah 8 terkoneksi dengan 9 `connected(8,9)` ? Ya.

- Lalu, bagaimana cara membuat 0 terkoneksi dengan 7?

- Hubungkan object 5 dengan 0 `union(5, 0)`!
- Hubungkan object 7 dengan 2 `union(7, 2)`!
- Hubungkan object 6 dengan 1 `union(6, 1)`!
- Hubungkan object 1 dengan 0 `union(1, 0)`!
- Jika kita tanya Apakah 0 terkoneksi dengan 7 `connected(0,7)` ? Ya.

```bash

(0) -> (1) -> (2)
 |      |      |
(5) -> (6)    (7)

(8) -> (3) -> (4) -> (9)
```

Implementasi Codenya [https://play.golang.org/p/qcz2EqJC5B1](https://play.golang.org/p/qcz2EqJC5B1)

1. Buat struct QuickFindUF yang berisi `id` dalam bentung slice integer `[]int`
2. Buat method pada struct QuickFindUF `Connected( p, q int)` dan  `Union(p, q int)`
3. Isi dari `Connected( p, q int)` hanya membandingkan apakah value dari index id[p] dan id[q] itu sama
4. Isi dari Union
   1. Ambil isi dari index id[p] sebagai pid dan id[q] sebagai qid
   2. iterasi sejumlah slice id
   3. jika value id[i] sama dengan pid
   4. assign id[i] sama dengan qid

Cara selanjutnya bisa menggunakan QuickUnion, Implementasi codenya [https://play.golang.org/p/WcIVSmD48Et](https://play.golang.org/p/WcIVSmD48Et)

1. Buat struct QuickFindUF yang berisi `id` dalam bentung slice integer `[]int`
2. Buat method pada struct QuickUnionUF `Connected( p, q int)` dan  `Union(p, q int)` ditambah dengan `Root(i int)`
3. Fungsi `Root(i int)` bertugas mencari root dari i
4. Fungsi `Connected( p, q int)` membandingkan apakah root dari `p` dan `q` sama?
5. Fungsi dari `Union(p, q int)` berubah menjadi mencari root dari p dan assign nilai root dari q

| algorithm   	| initialize 	| union 	| find 	|
|-------------	|------------	|-------	|------	|
| quick-find  	| N          	| N     	| 1    	|
| quick-union 	| N          	| N †   	| N    	|

Kecacatan pada QuickFind

- Proses Union terlalu mahal mengakses sejumlah N array
- Tree datar, tapi terlalu exprensive untuk membuat menjadi datar

Kecacatan pada QuickUnion

- Tree menjadi tinggi
- Proses Find terlalu mahal mengakses sejumlah N array

<!-- Apa yang dimaksud dengan tree flat dan tree menjadi tinggi? -->

### Improvement

#### Weighting

## Referensi

- [Algorithms Part 1](https://www.coursera.org/learn/algorithms-part1/home/welcome)
