+++
title = "Github Action"
categories = [
    "DevOps",
]
tags = [
    "CI/CD",
]
date = "2021-12-06"
draft = true
+++

Automation adalah salah satu inti dari praktek budaya DevOps.
Banyak sekali tools yang sudah dibuat dan bisa kita manfaatkan untuk melakukan CI/CD (continous integration / continous delivery) yang arti singkatnya adalah proses otomatisasi seperti test, build dan deployment.

Salah satu ide utama dari CI adalah otomatisasi proses seperti test, code quality dan build yang bisa dilakukan beberapa kali dalam sehari.
Ini penting banget karena biasanya bugs itu terjadi atau muncul ketika melakukan integrasi code, yang tidak sengaja terbuat.
CI akan mengelola proses ini, sehingga software engineer lebih fokus ke codenya sendiri.
Lalu CD akan mengikuti untuk mengumpulkan semua perubahan dan mengirimkan ke pengguna atau productions.
Ketika keduanya dikombinasikan mereka akan membantu kita untuk menjaga kualitas code, ringan dan mudah diadaptasi.

Salah satu tools yang sekarang sangat popular adalah Github Action, tools ini memudahkan kita untuk melakukan otomatisasi build, testing dan deployment pada platform apapun tanpa meninggalkan repository tempat dimana code lo berada. Karena github action terjadi didalam repository yang ada di github. 
Maka dari itu lo harus paham dulu cara menggunakan git dan punya akun GitHub. 
Gw asumsikan lo sudah paham dan sudah punya akun GitHub, jadi kita fokus ke Github Actionnya saja.

## Menggali lebih dalam Github Actions

Github Action memiliki lima komponen dan konsep inti. Yaitu:

1. Events
2. Jobs
3. Steps
4. Actions
5. Runners

### Events

Github action adalah event-driven. Maksudnya lo bisa mendefinisikan apa yang terjadi setelah event terjadi. Ada tiga grup workflow dari event.

1. Scheduled events
2. Manual events
3. Webhook events

#### Scheduled events

Scheduled events adalah event yang ditrigger berdasarkan waktu. 
Mereka menggunakan POSIX cron syntax.
Ini contoh event yang ditulis di YAML untuk mendefinisikan dimana workflow akan ditrigger setiap 5 menit.

```yaml
on:
    schedule:
        - cron: '*/5 * * * *'
```

Kalau tidak familiar bisa coba ini [crontab.guru](https://crontab.guru/).

#### Manual events

Meskipun menggunakan GitHub Action biasanya untuk otomatisasi, tapi github action juga menyediakan manual events.

Ada dua tipe dari manual events, yaitu **workflow_dispatch** dan **repository_dispatch**.

**workflow_dispatch** event bisa digunakan untuk mentrigger spesifik workflow tertentu dalam repository di GitHub secara manual.

```yaml
on:
  workflow_dispatch:
    inputs:
      username:
        description: 'Your GitHub username'
        required: true
      reason:
        description: 'Why are you running this workflow manually?'
        required: true
        default: 'I am running tests before implementing an automated workflow'
```

<!--TODO: demo workflow dispatch -->

**repository_dispatch** adalah event yang juga mengizinkan lo mentrigger manual workflow.
Perbedaannya workflow ini terjadi di repository berbeda atau dilingkungan luar GitHub.
Untuk trigger event ini lo harus menggunakan GitHub API dan mengirimkan post request yang menyediakan **event_type** yang menjelaskan jenis aktivitas apa yang mau ditrigger.

```bash
curl -X POST -H "Accept: application/vnd.github.v3+json" https://api.github.com/repos/octocat/hello-world/dispatches -d '{"event_type":"event_type"}'
```

lalu tambahkan setting ini di workflow filenya.

```yaml
on:
  repository_dispatch:
```

#### Webhook events

Event ini ditrigger oleh GitHub webhook events seperti issue dan pull request.
Misalnya buat trigger dari issue kita bisa nambahkan disini:

```yaml
on:
  issues:
    types: [opened]
```

### Jobs

Job adalah kumpulan dari langkah-langkah yang ada di runner yang sama.
Secara default multiple job akan dijalankan secara parallel tapi bisa juga dijalankan secara beurutan.

```yaml
jobs:
  tests_manual_workflow:
    runs-on: ubuntu-latest
```

### Steps

Steps adalah individual task yang bisa menjalankan command seperti shell command.
Dan perlu diketahui kalau step itu berjalan pada environtmentnya sendiri sehinga data didalamnya tidak bisa dipertahankan atau dibagi ke step lain.

```yaml
steps:
    - run: >
          echo "User ${{ github.event.inputs.username }} ran a workflow manually."
          echo "Because ${{ github.event.inputs.reason }}."
```

### Actions

Actions adalah command yang dapat berdiri sendiri dan bisa portable.
Mereka digabungkan menjadi steps untuk menciptakan jobs.
Lo bisa bikin actions sendiri dan membagikannya dengan komunitas atau lo dapat menggunakan action yang telah dibuat oleh komunitas. Lo bisa liat disini buat lihat kumpulan actions [github.com/actions/starter-workflows](https://github.com/actions/starter-workflows)

```yaml
jobs:
  stale:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/stale@v3
```

### Runners

Runner adalah server application sering kali diinstal di virtual machine atau docker container, yang menjalankan job dari github action.

Ada dua tipe runner:

- GitHub-hosted runner
- Self-hosted runner

```yaml
jobs:
  build:
    runs-on: macos-latest
```

## Mengerti workflow dasar

Workflow adalah proses otomatis dan dapat dikonfigurasi yang bisa lo tambahkan ke dalam repository github lo. Workflow terdiri dari satu atau beberapa job yang ditrigger oleh event tertentu. Konfigurasi alur kerja ditentukan dalam file dan workflow yang ditulis menggunakan YAML.


### Workflow Syntax

Ada beberapa `key` yang ada dikonfigurasi GitHub Action.

- **name:**

key **name:** merepresentasikan nama workflow untuk menambahkan penamaan yang berarti.
Kalau lo gak nambahin name maka akan diset menjadi nama file path di root repository lo.

- **on:**

Ini adalah key yang wajib ada untuk menentukan event atau event apa yang akan mentrigger workflow.

- **jobs**

Workflow bisa menjalakan satu atau lebih jobs. Secara default jobs berjalan secara parallel, kalo lo butuh job untuk menjalakan secara beurutan, lo butuh mendefinisikan type **needs:** dependencies kedalam **job_id**.

- **job_id**

Setiap job harus ada **job_id**. job ID berisi strings dan berisi karakter alphanumeric.
Setiap job id harus dimulai underscorde atau hurug dan harus unik.

- **needs**

Ini adalah key yang optional yang biasanya digunakan untuk melakukan urutan.

```yaml
jobs:
  jobA:
  jobB:
    needs: jobA
  jobC:
    needs: [jobA, jobB]
```

- **runs_on**

Ini adalah key yang dibutuhkan untuk mnentukan mesih runner dimana job akan berjalan.

```yaml
runs-on:
  self-hosted
```

- **steps**

Steps adalah tugas tugas yang ada didalam job. 
Steps juga berjalan didalam proses mereka sendiri.
Jadi perubahan envirntment TIDAK dipertahankan diantara steps.


```yaml
steps:
    - run: >
          echo "User ${{ github.event.inputs.username }} ran a workflow manually."
          echo "Because ${{ github.event.inputs.reason }}."
```

- **uses**

Kalau lo memutuskan untuk menggunakan existing action, yang merupakan unit code yang bisa digunakan kembali didalam workflow file lo. maka penggunaan **uses** akan sangat menbantu.
Sangat direkomendasikan lo menggunakan version. 


```yaml
jobs:
  stale:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/stale@v3
```

- **run**

**run** key digunakan untuk menjalankan command program seperti yang biasa digunakan pada console atau terminal pada setiap sistem operasi.

```yaml
- run: >
    echo "User ${{ github.event.inputs.username }} ran a workflow manually."
    echo "Because ${{ github.event.inputs.reason }}."
```

Lo bisa belajar tentang keys yang bisa lo masukin kedalam workflow file lo dengan menbaca dokumentasi ini [https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions.](https://docs.github.com/en/free-pro-team@latest/actions/reference/workflow-syntax-for-github-actions.).

### Membuat workflow file dari awal

Langsung saja kita coba GitHub Action. Disini gw udah ada sample code menggunakan golang.
Goalsnya adalah membuat CI/CD dengan GitHub Action yang stagenya dibagi menjadi seperti berikut ini:

- test
  - running unit testing and check coverage
- build
  - build images
  - push to docker hub
- deploy

Terima Kasih.