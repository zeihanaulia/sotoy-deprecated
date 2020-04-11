+++
title = "Memahami GraphQL Pagination"
categories = [
    "belajar",
]
tags = [
    "graphql",
]
date = "2020-03-02"
+++

Implementasi pagination atau bahasa indonesianya pemberian halaman pada data cukup berbeda.
Biasanya cuma bikin `?page=1&limit=10` dimana pada query databasenya menjadi `OFFSET 0 LIMIT 1`.
Pada GraphQL ada beberapa cara untuk melakukan pagination.


Sumber Bacaan:

- [BEST PRACTICES - Pagination](https://graphql.org/learn/pagination/)-
- [Paginating results with GraphQL](https://shopify.dev/concepts/graphql/pagination)