+++
title = "OpenTelemetry"
categories = [
    "microservice",
]
tags = [
    "observability",
    "opentelemetry"
]
date = "2021-12-23"
draft = true
+++

## OpenTelemetry

OpenTelemetry adalah kumpulan dari APIs, SDKs, tooling dan integrasi yang didesain untuk membuat dan mengelola telemetry data seperti **traces**, **metrics** dan **logs**.

Project ini bersifat vendor-agnostic kita bisa menggunakan bahasa pemrograman apapun dan visual apapun.

### Bahasa Pemrograman

#### [Go Support](https://opentelemetry.io/docs/instrumentation/go/)

Sampai tulisan ini dibuat, support pada bahasa go statusnya masih seperti ini:

- Tracing - Stable
- Metrics - Alpha
- Logging - Not Yet Implemented

Check link ini [https://opentelemetry.io/registry/?language=go](https://opentelemetry.io/registry/?language=go) untuk melihat package yang sudah dibuat.

## Specification Examples

Karena OpenTelemertry ditujukan agar vendor agnostic, maka dibuatlah spesifikasi.
Dimana setiap vendor harus mengikuti spesifikasi ini.

- [Overview]([https://opentelemetry.io/registry/?language=go](https://opentelemetry.io/registry/?language=go))
- [Database](https://github.com/open-telemetry/opentelemetry-specification/blob/main/specification/trace/semantic_conventions/database.md)

## Vendors

Ada beberapa vendors atau service yang bisa digunakan untuk memvisualisasikan data dari OpenTelemetry.

- [LightStep](https://lightstep.com/developers/opentelemetry/)
- [Datadog](https://docs.datadoghq.com/tracing/setup_overview/open_standards/)
- [Jaeger](https://www.jaegertracing.io/docs/1.21/opentelemetry/)[Self-Hosted]
- [New Relic](https://docs.newrelic.com/docs/more-integrations/open-source-telemetry-integrations/opentelemetry/introduction-opentelemetry-new-relic/)