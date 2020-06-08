+++
title = "Error download library"
categories = [
    "belajar",
]
tags = [
    "rust",
]
date = "2020-05-06"
+++

Ada masalah ketika mencoba menggunakan library. Jadi ketika run syntax `cargo run` dapat hasil seperti ini

```bash
Updating crates.io index
error: failed to load source for a dependency on `actix`

Caused by:
  Unable to update registry `https://github.com/rust-lang/crates.io-index`

Caused by:
  failed to fetch `https://github.com/rust-lang/crates.io-index`

Caused by:
  failed to authenticate when downloading repository
attempted ssh-agent authentication, but none of the usernames `git` succeeded

Caused by:
  no authentication available
```

Coba googling dapet issue [#2078](https://github.com/rust-lang/cargo/issues/2078) dari repository rustnya.
Dari sana dapet solusi untuk menambah config cargo `~/.cargo/config`.

```bash
[net]
git-fetch-with-cli = true
```

Selesai.