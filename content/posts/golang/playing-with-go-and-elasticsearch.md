+++
title = "Playing with go and elasticsearch"
categories = [
    "belajar",
    "elasticsearch",
    "golang",
]
tags = [
    "golang",
    "full-text search",
]
date = "2020-06-16"
draft = true
+++

I think all engineers know about elasticsearch as a datastore with fast searching. 
They have a full-text search feature and many rich features for searching.
And we can easily access the data using a simple REST API.
So, with this article, I want to make some walkthrough playing with elasticsearch and Golang.

First think frist, we need the elasticsearch it self, we can using [docker](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html) or you can install [manually](https://www.elastic.co/downloads/elasticsearch). But, I prefer using docker instead manually.

First, we need the elasticsearch itself, we can use [docker](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html) or install it [manually](https://www.elastic.co/downloads/elasticsearch). 
But, I prefer using docker instead manually.

```bash
docker run --rm -d -p 9200:9200 -p 9300:9300 --name es-server  -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.12.1
```

When the download is complete, you can check by accessing it. `http://localhost:9200`.
If it's work then we can play with the [REST API](https://www.elastic.co/guide/en/elasticsearch/reference/current/docs.html). But, we dont want to review all api right now, we focusing how to play with go and elasticsearch. 
So, Now I want to create a new project.

```bash
mkdir playing-go-elasticsearch
cd playing-go-elasticsearch
go mod init github.com/zeihanaulia/playing-go-elasticsearch
touch main.go
```

In main.go file, we need add code for connecting to elasticsearch.
There are 2 library for elasticsearch in go. 
From [elastic](https://github.com/elastic/go-elasticsearch) and [olivier](https://github.com/olivere/elastic).
You can see the difference in [here](https://github.com/olivere/elastic/issues/1240).
But I think, for now I choose library from elastic. Maybe I can compare in the next article.

```go
package main

import (
	"log"

	"github.com/elastic/go-elasticsearch/v8"
)

func main() {
	es, err := elasticsearch.NewDefaultClient()
	if err != nil {
		log.Fatalf("Error creating the client: %s", err)
	}
	log.Println(elasticsearch.Version)

	res, err := es.Info()
	if err != nil {
		log.Fatalf("Error getting response: %s", err)
	}
	defer res.Body.Close()
	log.Println(res)
}
```

Ok, Now if we run `go run main.go`, We can see response same as when we access `http://localhost:9200`.
For the simplicity of the application, we can make simple cli app.
I think we can have process for load the data, and get all data and then search by value.

I using cobra CLI, for generating the CLI app. It makes it simple for me. You can choose whatever you want.

```bash
cobra init --pkg-name github.com/zeihanaulia/playing-go-elasticsearch . -a "Playing with go and elasticsearch"
cobra add load
cobra add get
```

After creating a simple CLI app, then we start creating the process.
The First Process is to load the data product.
I make a sample of product data in the `data` directory.
The data is a JSON file. So, Lets make loaded process.

First, we add flags for path location of data and then we read the data. 
Open `./cmd/load.go` file. add some code like this.

```bash
/*
package cmd

import (
	"fmt"
	"io/ioutil"

	"github.com/spf13/cobra"
)

// loadCmd represents the load command
var loadCmd = &cobra.Command{
	Use:   "load",
	Short: "A Loader Porcess",
	Run: func(cmd *cobra.Command, args []string) {
		dat, err := ioutil.ReadFile(dataFile)
		check(err)
		fmt.Print(string(dat))

		fmt.Println("load called", dataFile)
	},
}

func init() {
	rootCmd.AddCommand(loadCmd)

	// Add flags for directory of producs file
	loadCmd.PersistentFlags().StringVar(&dataFile, "data", "./data/products.json", "data file")
}

func check(e error) {
	if e != nil {
		panic(e)
	}
}

```

Make sure the fail can be loaded, using `go run main.go load --data ./data/products.json`.
After that we transform products data to struct. using `json.Unmarshal()`. 
Before that we define the models first.

```go

```

