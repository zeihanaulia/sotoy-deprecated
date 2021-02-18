+++
title = "Membuat event driven microservice dengan cqrs dan event sourcing"
categories = [
    "belajar",
    "microservice",
    "golang",
]
tags = [
    "golang",
]
date = "2020-06-13"
draft = true
+++

Tulisan ini dibuat untuk mengimplementasian tulisan dari [@shijucv](https://twitter.com/shijucv), [Building Event-Driven Microservices with Event Sourcing and CQRS](https://medium.com/@shijuvar/building-microservices-with-event-sourcing-cqrs-in-go-using-grpc-nats-streaming-and-cockroachdb-983f650452aa)

Sepertinya menarik untuk mengulik tulisan tersebut dan mencatat kembali pengalaman ketika membuatnya. Dengan contoh kasus yang kurang lebih sama.

## Membuat Service Dasar

Service pertama yang akan dibuat adalah **service order**. Setiap order yang dibuat akan dilanjukan ke **shop service**. Service order juga akan terhubung ke **service voucher** untuk menggitung voucher yang digunakan. Service order juga akan terhubung ke **payment service** untuk memberitahukan jika order telah diperoses.

Karena sekarang membuat service dasar, semua terhubung lewar API.

Referensi: 
- [Building Event-Driven Microservices with Event Sourcing and CQRS](https://medium.com/@shijuvar/building-microservices-with-event-sourcing-cqrs-in-go-using-grpc-nats-streaming-and-cockroachdb-983f650452aa)