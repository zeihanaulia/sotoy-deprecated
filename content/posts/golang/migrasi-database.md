+++
title = "Migrasi database dengan golang"
categories = [
    "belajar",
]
tags = [
    "golang",
    "postgre",
]
date = "2020-03-30"
+++

Akhir akhir ini terasa capek ketika membuat schema database langsung menggunakan GUI. Dan schema yang berubah bisa kadang kehilangan sejarahnya, kapan perubahannya tidak terawasi. Bagaimana jika skema database disatukan dengan source code aplikasi kita. Sehingga perkembangan skema database dapat tercatat sama seperti code.

Dengan tools bernama [migrate](https://github.com/golang-migrate/migrate) menjadi solusi untuk permasalahan diatas. alih alih kita langsung membuat tabel diGUI database, kita bikin dalam script yang nantinya dimasukan kedalam database juga.

## Install Golang Migrate

Untuk pengguna mac os:

```bash
brew install golang-migrate
```

## Membuat file migrate

```bash
    migrate create -ext sql -dir postgres/migrations create_categories
    migrate create -ext sql -dir postgres/migrations create_products
```

Kode diatas akan membuat file seperti ini 

```bash
20200208175630_create_categories.down.sql
20200208175630_create_categories.up.sql
20200208175630_create_products.down.sql
20200208175630_create_products.up.sql
```

Bagian `.up` akan diisi dengan syntak `CREATE table` misal, 

```bash categories
CREATE SEQUENCE categories_id_seq;
CREATE TABLE categories (
   id INTEGER NOT NULL DEFAULT nextval('categories_id_seq'),
   parent_id INTEGER,
   name VARCHAR(255) NOT NULL,
   created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW() NOT NULL,
   created_by  INTEGER                                NOT NULL,
   updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW() NOT NULL,
   updated_by  INTEGER                                ,
   deleted_at TIMESTAMP WITH TIME ZONE DEFAULT NULL,
   deleted_by  INTEGER, 
   PRIMARY KEY (id)
);
ALTER SEQUENCE categories_id_seq
OWNED BY categories.id;
```

```bash products
CREATE SEQUENCE products_id_seq;
CREATE TABLE products (
   id INTEGER NOT NULL,
   category_id  INTEGER NOT NULL,
   sku VARCHAR(255) NOT NULL,
   name VARCHAR(255) NOT NULL,
   price NUMERIC NOT NULL,
   margin NUMERIC NOT NULL,
   created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW() NOT NULL,
   created_by  INTEGER                                NOT NULL,
   updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW() NOT NULL,
   updated_by  INTEGER                                ,
   deleted_at TIMESTAMP WITH TIME ZONE DEFAULT NULL,
   deleted_by  INTEGER,   
   PRIMARY KEY (id),
   FOREIGN KEY (category_id) REFERENCES categories (id)
);
ALTER SEQUENCE products_id_seq
OWNED BY products.id;
```

sedangkan bagian `.down` akan diisi dengan `DROP TABLE`

```bash categories
DROP TABLE categories;
```

```bash products
DROP TABLE products;
```

Setelah itu tinggal jalankan command,

```bash
migrate -path "repositories/postgres/migrations" -database "$DATA_SOURCE_NAME" up / down
```

Selesai.
