+++
title = "Write Less Code, Generate MoreI"
categories = [
    "golang",
]
tags = [
    "golang",
    "codegen",
	"youtube"
]
date = "2021-10-15"
draft = false
+++

# [Write Less Code, Generate More](https://www.youtube.com/watch?v=iLk_LnGrst4)

## Examine the principle of [[code generation]]

Code Generation adalah salah satu teknik yang powerfull dimana kita bisa sedikit menulis kode dan menbuat secara otomatis.

## Discover the main parts of a code generator

### Who or what is a code generator?

Kalau ditanya siapa? manusia atau programmer adalah code generator. Bagus dalam problem solving dan kreative. Tapi manusia juga biasanya lama kelamaan jenuh dengan melakukan tugas yang terus berulang.

Tapi dengan adanya komputer, dia bisa membuat komputer program dengan menulis komputer program. Dia juga bagus dengan task yang berulang. Tapi komputer saat ini belum bisa kreative dan menyelesaikan masalah, mungkin nanti era AI. 

Lalu apa yang bisa digenerate?

Apapun bisa digenerate. Program bisa generate apapun.

### But what about the input(s)?

Bisa aja cuma zero input, kalo lo pernah cobain [hugo](https://gohugo.io/) pas lo generate kita bisa tanpa perlu input apapun. Tapi biasanya ada input seperti ini:

- Flags/arguments
- Code
- Configuration
- Metadata
- Environment
- No Input
- ...

## Look at some popular code generators

### Today: we will focus on Go-based code generation

Go Code -> Code Generator -> Go Code

Kita bisa membuat code generator dengan Go Code yang akan menghasilkan Go Code.

#### Example of code generators

- [go test](https://pkg.go.dev/cmd/go/internal/test)

```bash
Input: *_test.go Go source files
Output: package main test program source file
```

- [https://github.com/gopherjs/gopherjs](https://github.com/gopherjs/gopherjs)

```bash
Input : Go source code
Output: Javascript source code
```

- [Protocol Buffers Compilers](https://developers.google.com/protocol-buffers)

```bash
Input : .proto declarations
Output: Go source code, Java source code, ...
```

-[ https://pkg.go.dev/golang.org/x/tools/cmd/stringer](https://pkg.go.dev/golang.org/x/tools/cmd/stringer)

```bash
Input : type declarations in Go Code
Output: String() method for those type
```

## See how code generation fits into developer workflow

### Motivating example: stringer

Stringer adalah adalah tools yang akan mengenerate code dari integer menjadi string. Lihat contoh kode dibawah ini.

```go
package main

func main() {
 var p Pill = Paracetamol
 fmt.Printf("You need to take 2 %v per day\n", p)
}

type Pill int

const (
	Placebo Pill = iota
	Aspirin
	Ibuprofen
	Paracetamol
	Acetaminophen = Paracetamol
)

// Output: You need to take 2 3 per day
```

Ketika diprint kode tersebut sangat kurang bagus, karena  yang diprintnya itu berbentuk integer, tidak jelas maknanya. Agar lebih jelas mungkin bisa saja kita bikin function tambahan seperti ini:

```go
func (p Pill) String() string {
	switch p {
	case Aspirin:
		return "Aspirin"
	case Ibuprofen:
		return "Ibuprofen"
	case Paracetamol:
		return "Paracetamol"
	case Acetaminophen:
		return "Paracetamol"
	case Placebo:
		return "Placebo"
	}
}
```

Tapi kan kalo seperti itu jadiada usaha lebih ya dan mungking bisa aja typo dan menyebabkan bugs. Ngerjainnya juga ngebosenin banget.

Dengan tools stringer kita tidak perlu lagi repot repot menulis kode seperti itu. cukup dengan install lalu ketik

```bash
string -type=Pill
```

maka tools tersebut akan langsung membuatkan file dengan isi kode seperti ini:

```go
// Code generated by "stringer -type=Pill"; DO NOT EDIT.
package main

import "strconv"

func _() {
	 // An "invalid array index" compiler error signifies that the constant values have changed.
	 // Re-run the stringer command to generate them again.
	 var x [1]struct{}
	 _ = x[Placebo-0]
	 _ = x[Aspirin-1]
	 _ = x[Ibuprofen-2]
	 _ = x[Paracetamol-3]
}

const _Pill_name = "PlaceboAspirinIbuprofenParacetamol"

var _Pill_index = [...]uint8{0, 7, 14, 23, 34}

func (i Pill) String() string {
 if i < 0 || i >= Pill(len(_Pill_index)-1) {
 	return "Pill(" + strconv.FormatInt(int64(i), 10) + ")"

 }
 return _Pill_name[_Pill_index[i]:_Pill_index[i+1]]
}
```

Kode tersebut sudah efisien dan tidak akan terjadi typo karena semua dibuat secara otomatis.

### stringer: meet go generate

Kalau sebelumnya kita menulis secara manual `stringer -type=Pill`, bagaimana jika ada banyak yang harus digenerate? bagaimana jika lupa mengenerate ulang, padahal ada perubahan pada typenya. Untuk mengatasi semua itu kita bisa menggunakan tool `go generate`.

go generate mengotomatikan code genenator. Dia akan membaca spesial comment pada source code lo.

komen yang perlu ditambahkan seperti ini

```bash
//go:generate stringer -type=Pill
```

lalu kita jalankan go generatenya

```bash
go generate ./...
```

Jadi semakin mudah bukan? kita tidak perlu mengenerate semuanya

Tapi perlu diingat, `go generate` tidak masuk didalam build proses, jadi tetep harus dijalankan duluan atau dijalankan secara terpisah.

### Jadi intinya apa
- Dengan tool generator semacam ini akan mempermudah hidup lo sebagai programmer golang. Lo gak perlu lagi nulis kode yang berulang setiap kali membuat hal yang sama.

## Write a simple code generator

Kita akan coba membuat generator sendiri, meniru dari stringer. dengan nama simplestringer.

-  simplestringer: what it will do
	- ambil nama dari interger type T sebagai argument
	- temukan semua konstant dari type T didalam source package
	- definisikan metode *String()* pada T in go ffile baru, sama kaya `stringer`

![[write-less-code-layer-of-abstraction.png]]

1. Buat input sederhana yang menerima argumen nama type
	```go
	typeName := os.Args[1]
	var names []string
	```
2. membuat package
	
	membuat configurasi dari package `packages`
	
	```go
	cfg := &packages.Config{
		Mode: packages.NeedTypes | packages.NeedTypesInfo |
			packages.NeedSyntax | packages.NeedName,
	}
	pkgs, err := packages.Load(cfg)
	if err != nil {
		panic(err)
	}
	if len(pkgs) != 1 {
		panic(fmt.Errorf("got unexpected number of packages %v", len(pkgs)))
	}
	pkg := pkgs[0]
	```
3. Ambil nama dari types
    Extract value dari types lalu, karena type yang kita buat dalam 1 scoup const, maka cari nama type yang targetTyepnya ==  const. Jika sama lalu ambil dan append pada names slice yang sudah kita define diawal
	```go
    targeType := pkg.Types.Scope().Lookup(typeName)
	if targeType == nil {
		panic(fmt.Errorf("failed to find type declaration for %v", typeName))
	}

	for _, file := range pkg.Syntax {
		for _, decl := range file.Decls {
			gd, ok := decl.(*ast.GenDecl) // type, const, var
			if !ok {
				continue
			}
			if gd.Tok != token.CONST {
				continue
			}
			for _, spec := range gd.Specs {
				spec := spec.(*ast.ValueSpec)
				for _, name := range spec.Names {
					if pkg.TypesInfo.Defs[name].Type() == targeType.Type() {
						names = append(names, name.Name)
					}
				}
			}
		}
	}
	sort.Strings(names)
	```
4. Buat tempate code
	Karena kita ingin bikin method baru dari suatu type maka kita harus membuat template untuk digenerate

	```go
	const outTmpl = `package {{.PkgName}}

	import "fmt"

	func (v {{.TypeName}}) String() string {
		switch v {
			{{range .ConstVals -}}
		case {{.}}:
			return "{{.}}"
		{{end -}}
		default:
			panic(fmt.Errorf("unknown {{.TypeName}} value %d", v))
		}
	}`
	```

5. Buat file file baru untuk diisi code dari tempate
	
	```go
	outFileName := fmt.Sprintf("gen_%v_simplestringer.go", strings.ToLower(typeName))
	outFile, err := os.Create(outFileName)
	if err != nil {
		panic(err)
	}
	```
6. Buat struct untuk assign value ke template
	```go
	var tmplVals = struct {
		PkgName   string
		TypeName  string
		ConstVals []string
	}{
		PkgName:   pkg.Name,
		TypeName:  typeName,
		ConstVals: names,
	}
	```
7. Inject value ke template
	```go
	tmpl := template.Must(template.New("out").Parse(outTmpl))
	if err := tmpl.Execute(outFile, tmplVals); err != nil {
		panic(err)
	}

	if err := outFile.Close(); err != nil {
		panic(err)
	}
	```
8. Execute command
	
	```go
	cmd := exec.Command("gofmt", "-w", outFileName)
	if errOut, err := cmd.CombinedOutput(); err != nil {
		panic(fmt.Errorf("failed to run %v: %v\n%s", strings.Join(cmd.Args, " "), err, errOut))
	}
	```

## Inspire everyone to join the golang-tools community of tool builders!

- [Go Tooling WIKI](https://github.com/golang/go/wiki/golang-tools)
- [Join Gophers Slack](https://invite.slack.golangbridge.org/)