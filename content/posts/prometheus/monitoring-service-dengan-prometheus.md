+++
title = "Monitoring Service Dengan Prometheus"
categories = [
    "belajar",
    "monitoring",
]
tags = [
    "monitoring",
    "microservice",
    "golang",
]
date = "2020-05-26"
+++

Prometheus adalah proyek open source metrics-based monitoring system.
Intinya sih cuma alat untuk mengumpulkan data model dan bisa diquery (PromQL). 
Karena bisa diquery, jadi kita bisa menganalisa performa dari aplikasi dan infrastruktur yang kita bangun.
[Data model](https://prometheus.io/docs/concepts/data_model/) itu sebenernya hanya kumpulan teks yang berformat.
Data yang dikumpulkan disimpan sebagai [time series](https://en.wikipedia.org/wiki/Time_series).
Bentuk dari notasinya seperti ini:

```bash
<metric name>{<label name>=<label value>, ...}
```

Contoh, Kalau kita mau membuat metric http request total:

```bash
api_http_requests_total{method="POST", handler="/messages"}
```

Jadi kalo diartikan sama dengan:

- Nama metrics : api_http_requests_total
- label 1 : method="POST"
- label 2 : handler="/messages"

Metrics bukan hanya berisi nama tetapi juga ada `label` yang berisi key-value format. 
PromQL query bisa melakukan aggregasi lintas label, 
jadi kita bisa menganalisa bukan hanya perproses melainkan juga bisa per datacenter atau per service.
Semuanya bisa digambarkan di dashboard system seperti [Grafana](https://grafana.com/).

Alert system juga bisa dilakukan sama PromQL, Pokoknya kalo bisa digambarkan, pasti kita bisa bikin alertnya.
Label membuat pemeliharaan alert menjadi lebih mudah, Kita bisa bikin satu alert yang mencangkup semua nilai nilai label.

## Apa yang dimaksud dengan monitoring dengan prometheus

Kita sudah tau lah, monitoring biasanya dilakukan untuk memperhatikan sesuatu, istilah indonesiannya memantau dan mengukur lah.
Misalnya monitoring kemacetan, berapa km perjam rata rata kendaraan berjalan. 
Berapa derajat suhu panas yang dibutuhkan untuk membuat secangkir kopi manual brew.
Tapi bukan itu yang akan kita bahas, konteksnya adalah bagaimana kita memonitor sistem atau aplikasi.
Monitoring dibagi menjadi 4 hal, yaitu:

- Alerting    = Mengirimkan pemberitahuan hal yang berjalan dengan salah
- Debugging   = Invstigasi untuk menentukan root cause dan menyelesaikan apapun masalahnya.
- Trending    = Melihat trend dari service, semisal ada lonjakan traffic, kita bisa siap siap untuk merencanakan dan memutuskan sistem yang baru
- Plumbing    = Kerjaan iseng aja, mau monitor lain lain.

## Install Prometheus

Kita coba menginstall prometheus menggunakan docker. Pastikan docker sudah terinstall dikomputer masing-masing.

```bash
docker run --rm --name promtheus-server -p 9090:9090 -v $HOME/Programming/research/go-prometheus/config:/etc/prometheus prom/prometheus
```

Prometheus image menggunakan volume untuk mengambil config file. Config filenya diberinama `prometheus.yml` dan isinya seperti ini

```yaml
global:
  scrape_interval:     15s # By default, scrape targets every 15 seconds.

  # Attach these labels to any time series or alerts when communicating with
  # external systems (federation, remote storage, Alertmanager).
  external_labels:
    monitor: 'codelab-monitor'

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s

    static_configs:
      - targets: ['localhost:9090']
```

Konfigurasi diatas adalah config standart bisa dilihat [disini](https://prometheus.io/docs/prometheus/latest/getting_started/).
Inti dari config diatas adalah bikin job yang bernama `prometheus` yang jarak pengambilannya 5 detik dan akses di endpoint `http://localhost:9090` ini endpoint prometheus yang sebentar lagi kita jalankan.

Kita coba jalankan syntax ini diconsole masing masing, tapi sebelumnya bikin dulu folder config dan masukan file `prometheus.yml`nya kedalam folder config. Biar rapih aja.
Jika berhasil dijalankan kurang lebih tampilannya sepertin ini.

```bash
➜ docker run --rm --name promtheus-server -p 9090:9090 -v $HOME/Programming/research/go-prometheus/config:/etc/prometheus prom/prometheus
level=info ts=2020-05-26T19:51:05.388Z caller=main.go:302 msg="No time or size retention was set so using the default time retention" duration=15d
level=info ts=2020-05-26T19:51:05.388Z caller=main.go:337 msg="Starting Prometheus" version="(version=2.18.1, branch=HEAD, revision=ecee9c8abfd118f139014cb1b174b08db3f342cf)"
level=info ts=2020-05-26T19:51:05.388Z caller=main.go:338 build_context="(go=go1.14.2, user=root@2117a9e64a7e, date=20200507-16:51:47)"
level=info ts=2020-05-26T19:51:05.388Z caller=main.go:339 host_details="(Linux 4.19.76-linuxkit #1 SMP Fri Apr 3 15:53:26 UTC 2020 x86_64 8dd5b71bfa58 (none))"
level=info ts=2020-05-26T19:51:05.388Z caller=main.go:340 fd_limits="(soft=1048576, hard=1048576)"
level=info ts=2020-05-26T19:51:05.389Z caller=main.go:341 vm_limits="(soft=unlimited, hard=unlimited)"
level=info ts=2020-05-26T19:51:05.393Z caller=main.go:678 msg="Starting TSDB ..."
level=info ts=2020-05-26T19:51:05.402Z caller=web.go:523 component=web msg="Start listening for connections" address=0.0.0.0:9090
level=info ts=2020-05-26T19:51:05.405Z caller=head.go:575 component=tsdb msg="Replaying WAL, this may take awhile"
level=info ts=2020-05-26T19:51:05.406Z caller=head.go:624 component=tsdb msg="WAL segment loaded" segment=0 maxSegment=0
level=info ts=2020-05-26T19:51:05.406Z caller=head.go:627 component=tsdb msg="WAL replay completed" duration=655.995µs
level=info ts=2020-05-26T19:51:05.411Z caller=main.go:694 fs_type=EXT4_SUPER_MAGIC
level=info ts=2020-05-26T19:51:05.411Z caller=main.go:695 msg="TSDB started"
level=info ts=2020-05-26T19:51:05.411Z caller=main.go:799 msg="Loading configuration file" filename=/etc/prometheus/prometheus.yml
level=info ts=2020-05-26T19:51:05.425Z caller=main.go:827 msg="Completed loading of configuration file" filename=/etc/prometheus/prometheus.yml
level=info ts=2020-05-26T19:51:05.425Z caller=main.go:646 msg="Server is ready to receive web requests."
```

Jika kita akses [http://localhost:9090/graph](http://localhost:9090/graph) seharusnya muncul halaman prometheus. Disana sudah ada metrics dari prometheusnya itu sendiri.

### Darimana datangnya data-data metrics itu?

ya dari endpoint [http://localhost:9090/metrics](http://localhost:9090/metrics) yang sudah kita setting di file config tadi.

Kita coba implementasikan pada Web Server sederhana untuk menampilkan metricsnya.

## Monitoring HTTP (Go Language)

```go
package main

import (
  "fmt"
  "log"
  "net/http"
)

func handler(w http.ResponseWriter, r *http.Request) {
  _, _ = w.Write([]byte("Hello Prometheus!"))
}

func main() {
  http.HandleFunc("/", handler)
  fmt.Println("Listening on http://localhost:8000")
  log.Fatal(http.ListenAndServe(":8000", nil))
}

```

Diatas adalah kode web server sedehana hanya ada 1 endpoint dengan method GET yang hanya menampilkan text `Hello Prometheus!`.
Untuk menambahkan metrics sebenernya cukup mudah, Kita hanya perlu menambahkan endpoint  `/metrics` sama seperti endpoint metricsnya prometheus server itu sendiri.

```go
package main

import (
  "fmt"
  "log"
  "net/http"

  "github.com/prometheus/client_golang/prometheus/promhttp"
)

func handler(w http.ResponseWriter, r *http.Request) {
  _, _ = w.Write([]byte("Hello Prometheus!"))
}

func main() {
  http.HandleFunc("/", handler)
  http.Handle("/metrics", promhttp.Handler())
  fmt.Println("Listening on http://localhost:8000")
  log.Fatal(http.ListenAndServe(":8000", nil))
}
```

Dengan menambah syntax `http.Handle("/metrics", promhttp.Handler())` kita sudah mengexpose metrics yang ada di webserver yang kita buat.
Sekarang tinggal kita bikin job pada config prometheus dan kita restart. 

```yaml
  - job_name: 'simplewebserver'

    # Override the global default and scrape targets from this job every 5 seconds.
    scrape_interval: 5s

    static_configs:
      - targets: ['host.docker.internal:8000']
```

> Catatan:
> **host.docker.internal** hanya untuk docker for mac.

Lalu pada [dashboard prometheus](http://localhost:9090/graph), 
Coba cek pada bagian Status -> Target. Apakah sudah muncul job yang kita buat?

## Referensi

- [GOTO 2019 • An Introduction to Systems & Service Monitoring with Prometheus • Julius Volz](https://www.youtube.com/watch?v=5O1djJ13gRU)
- [Intro + Deep Dive: Prometheus - Julius Volz, Prometheus & Richard Hartmann, SpaceNet](https://www.youtube.com/watch?v=9GMWvFcQjYI)
- [INSTALLATION - Prometheus](https://prometheus.io/docs/prometheus/latest/installation/)