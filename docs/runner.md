---
layout: docs
---

## Setup dengan Script Runner

Runner adalah fitur inti di DOM Cloud. Ini memungkinkan Anda untuk melakukan konfigurasi otomatis semua dengan kenyamanan satu skrip.

Untuk penerapan skrip umum, Anda tidak perlu menulisnya sendiri. Anda dapat memulai dari template yang ada di [the homepage](https://domcloud.io/#templates) atau [template repository](https://github.com/domcloud/dom-templates).

Kami membuatnya sesederhana mungkin untuk dipahami. Dan lebih baik lagi,runner script kami adalah open source. Penjelasan lebih mendalam tersedia di halaman repositori.

Script runner dalam format YAML. Misalnya untuk membuat instance WordPress baru :

```yaml
source:
  url: https://wordpress.org/latest.zip
  directory: wordpress
features: [mysql, ssl]
nginx:
  ssl: enforce
  fastcgi: on
  locations:
    - match: /
      try_files: $uri $uri/ /index.php$is_args$args
commands:
  - cp wp-config-sample.php wp-config.php
  - sed -i "s/database_name_here/${DATABASE}/g" wp-config.php
  - sed -i "s/username_here/${USERNAME}/g" wp-config.php
  - sed -i "s/password_here/${PASSWORD}/g" wp-config.php
  - sed -i "s/utf8/utf8mb4/g" wp-config.php
```

Dengan melihat sekilas, Anda dapat mengetahui bahwa itu akan diunduh `https://wordpress.org/latest.zip` mengekstrak dan memindahkannya dari `wordpress` directory, kemudian membuat database mysql dan menandatangani sertifikat SSL, juga mengonfigurasi catatan NginX yang benar untuk WordPress dan melakukan dengan cepat `sed` operasi untuk inject database credentials langsung ke file konfigurasi.

Inilah yang dapat Anda konfigurasikan:

---

### `source`

tipe : `string` atau `object`. Jika itu string, itu akan menjadi `url`.

Jika nilai ini disetel, itu akan mengunduh konten di dalam host. Konten dapat berupa file ZIP atau repositori Git (untuk melakukan kloning).

> PERHATIAN: Tindakan ini akan **menimpa** semua konten di dalam host.

#### `url`

Zip (atau kloning) URL untuk diunduh. Required.

#### `type`

untuk `extract` file ZIP atau `clone` sebuah repo. Jika dihilangkan, itu otomatis mendeteksi apakah itu `github.com` atau `gitlab.com` URL dan Lakukan `clone` sebagai ganti `extract`.

#### `directory` (`type: extract` only)

menentukan nama folder yang akan dipindahkan dari file ZIP setelah ekstraksi. Ini juga dapat ditentukan dari hash `url`. Jika dihilangkan, tidak ada operasi pemindahan yang dilakukan.

> Untuk alasan warisan, sebuah `directory` di root config juga akan berfungsi, itu akan dikonversi sebagai `source.directory`.

#### `branch` (`type: clone` only)

menentukan clone branch untuk diperiksa. Ini juga dapat ditentukan dari direktori atau `url`'s hash. Jika dihilangkan, cabang default akan dicentang.

#### `shallow` (`type: clone` only)

melakukan shallow clone? bawaan ke `true`. Disarankan untuk membiarkannya seperti itu untuk menjaga `.git` ukuran internal rendah.

#### `submodules` (`type: clone` only)

melakukan Recursive clone of submodules? bawaan ke `false`.

---

### `features`

Type: Array of `string` atau `object`. Jika satu item adalah string, itu akan dikonversi menjadi objek (misal. `mysql on` menjadi `{ mysql: on }`).

Ini mengonfigurasi semua fitur yang tersedia untuk host di DOM Cloud.

#### mysql

Menconfigurasi MariaDB (MySQL).

- `mysql` atau `mysql on` Mengaktifkan MariaDB dan membuat default DB.
- `mysql create <dbname>` Membuat database baru `<dbname>`.
- `mysql drop <dbname>` menghapus database `<dbname>`.
- `mysql off`. Menonaktifkan fitur `mysql`. **PERHATIAN: ini juga menghapus DataBase secara permanen**.

#### postgresql

Menconfigurasi PostgreSQL.

- `postgresql` atau `postgresql on` Mengaktifkan PostgreSQL dan membuat default DB.
- `postgresql create <dbname>` Membuat database baru `<dbname>`.
- `postgresql drop <dbname>` menghapus database `<dbname>`.
- `postgresql off`. Menonaktifkan fitur `postgresql`. **PERHATIAN: ini juga menghapus DataBase secara permanen**.

#### dns

Menconfigurasi BIND DNS Server.

- `dns` atau `dns on` Mengaktifkan DNS server.
- `dns add <type> <value>` Menambahkan record.
- `dns rem <type> <value>` Menghapus record.
- `dns off`. Menonaktifkan fitur `dns`. **Caution: Also clears all DNS records**.

DNS records untuk child server ditangani secara otomatis.

#### firewall

mengkonfigurasi Whitelist Firewall.

- `firewall on` atau `firewall` mengaktifkan firewall.
- `firewall off` menonaktifkan firewall.

Firewall adalah perlindungan tambahan untuk memastikan host tidak akan mengirim permintaan keluar yang tidak ditentukan dalam [whitelist](). Direkomendasikan untuk host mana pun yang tidak menggunakan API pihak ketiga.

**Harap dicatat karena kelemahan PHP yang jelas dalam keamanan, itu wajib untuk mengaktifkan fitur ini untuk WordPress atau server PHP yang dilindungi dengan lemah**.

#### ssl

Coba perbarui SSL certificate.

#### root

Atur directory root path.

#### php

Atur PHP (FastCGI) version. Pilihan yang tersedia:

- `php 5.6`
- `php 7.4` (default)
- `php 8.0`

Ingat `php 8.x` adalah rilis aktif. Secara otomatis akan bertambah seiring waktu.

#### node

Install spesifik versi NodeJS.

untuk default adalah Node 14.x

#### python

Install spesifik versi Python.

untuk default adalah Python 3.6

#### ruby

Install spesifik versi Ruby.

Untuk default adalah Ruby 2.7

---

### `nginx`

Type: `object`.

Ini adalah konfigurasi NGINX untuk host tertentu.

#### `ssl`

HTTPS options: `on` (default), `enforce` (selalu redirect HTTP ke HTTPS), `off` (tidak direkomendasikan).

#### `fastcgi`

Apakah akan mengaktifkan atau tidak mengaktifkan eksekusi file PHP: `on` atau `off`. Jika dihilangkan, `off` adalah default.

Jika Anda mengatur ini ke `on`, tolong pertimbangkan untuk memutar [`firewall`](#firewall) on.

Anda dapat mengganti pengaturan PHP melalui `.user.ini` ile di root dokumen. Setelah diubah tidak akan langsung berubah karena cache, tetapi biasanya membutuhkan waktu tidak lebih dari 5 menit.

#### `error_pages`

Daftar perintah halaman Eror. Ini sangat berguna untuk situs web statis. Baca selengkapnya [the NGINX docs](http://nginx.org/en/docs/http/ngx_http_core_module.html#error_page).

- `404 /404.html`: menunjukan `404` error page as `404.html`.
- `404 =200 /200.html`: menganggap `404` error as 200 OK and menunjukan `200.html` (SPA).
- `500 502 503 504 /50x.html`: menunjukan `50x` error as `50x.html`.

### `passenger`

Jika Anda ingin menjalankan aplikasi Non-PHP, Anda perlu mengatur dengan passenger phusion.
Untuk menonaktifkan aplikasi non-PHP , minimal Anda memerlukan konfigurasi berikut:

```yaml
root: public_html/public
passenger:
  enabled: on
  app_start_command: node server.js --port=$PORT
```

Konfigurasi di atas akan dijalankan `node server.js --port=$PORT` di induk folder root (dalam hal ini, `~/public_html`). Perhatikan bahwa Anda harus melewati `$PORT` dan gunakan itu sebagai port tempat aplikasi Anda. Jika aplikasi Anda menerima port dari environment Anda dapat menggunakan env seperti `env PORT=$PORT node server.js`.

untuk custom environment nilai anda dapat menggunakan `app_envs` tapi disarankan untuk menulis langsung ke `~/.env` sebagai gantinya.

Untuk memulai ulang aplikasi non-PHP, Anda dapat menjalankan `passenger-config restart-app /`.

Pilihan yang tersedia:

- `enabled`: (`on` atau `off`)
- `app_envs`: array of environments
- `app_start_command`: Passenger GLS command
- `app_type`: Type of App (untuk non GLS)
- `startup_file`: Startup filename (untuk non GLS)
- `ruby`: Ruby path (untuk non GLS)
- `nodejs`: Nodejs path (untuk non GLS)
- `python`: Python path (untuk non GLS)
- `meteor_app_settings`: Meteor path (untuk non GLS)
- `friendly_error_pages`: (`on` atau `off`)
- `document_root`: path to public document root (default is `root`)
- `base_uri`: base URL
- `app_root`: path to app root (default is parent of `root`)
- `sticky_sessions`: (`on` atau `off`) aktifkan ini untuk dukungan websocket

### `locations`

Array objects which one or more of:

- `match`: Matching URL location (required)
- `root`: Root path (relative to `~`)
- `alias`: Alias path (relative to `~`)
- `rewrite`: Rewrite URL directive
- `try_files`: Try files URL directive
- `return`: Return code directive
- `fastcgi`: (same as above)
- `passenger`: (same as above)

---

### `commands`

Type: array of `string`.

Daftar perintah SSH secara berurutan. Direktori awal selalu `~/public_html`. Jika ada kode keluar yang terdeteksi bukan nol, rantai eksekusi berhenti.

Di sinilah semua hal yang biasanya Anda letakkan setelah eksekusi `source`. Hal-hal seperti menginstal depedencies, menggabungkan frontend atau injecting database credential.

Salah satu contoh yang baik untuk production environment adalah:

#### Installing depedencies

- PHP `composer.json`: `composer install --no-dev --no-cache --optimize-autoloader`
- NodeJS `package.json`: `npm ci` or `yarn install --freeze-lockfile`
- Python `requirements.txt`: `pip install --no-cache-dir --user -r requirements.txt`
- Ruby `gem.bundle`: `bundle install --without development test`
