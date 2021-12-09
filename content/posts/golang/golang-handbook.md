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

Contoh menggunakan standard library

```go
package main

import (
	"fmt"
	"io/ioutil"
	"log"
	"net/http"
	"time"
)

func main() {
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
	log.Printf("Listening on port :%d\n", port)
	log.Fatal(s.ListenAndServe())
}
```

