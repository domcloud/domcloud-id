# DOM Cloud

Selamat datang di DOM Cloud! Hosting provider yang inovatif untuk developer IT di Indonesia.

Kami menggunakan GitHub untuk [Pertanyaan](./issues) dan [Diskusi](./discussion). Untuk pertanyaan privat silahkan kirimkan email ke [support@domcloud.id](mailto:support@domcloud.id).

> For english communication please use [the other repository](https://github.com/domcloud/domcloud-io/).

## FAQ Layanan

### Siapa orang dibalik DOM Cloud?

[Saya sendiri](https://github.com/willnode)

### Menggunakan server apa dan dimana?

DOM Cloud menggunakan server dari [Digital Ocean](https://m.do.co/c/facab6f3ac67) dan saat ini melayani server berlokasi:

+ Singapura [sga.domcloud.id] dengan domain default *.dom.my.id
+ New York City [nyc.domcloud.id] dengan domain default *.domcloud.io

### Apakah benar-benar gratis?

Ya. Paket Freedom adalah paket gratis, memiliki kapasitas 256 MB dan bandwidth 1 GB per bulan. Website gratis anda tidak akan kadarluarsa selama anda memperbarui masa ekspirasi minimal 2 bulan sekali.

Jika anda ingin lebih anda bisa [berlangganan paket lain](https://domcloud.id/price), berlaku per website:

| Fitur | Freedom | Lite | Pro | Super | Ultimate |
|---|:-:|:-:|:-:|:-:|:-:|
| Storage | 256 MB | 1 GB | 4 GB | 7 GB | 10 GB |
| Bandwidth | 12 GB | 45 GB | 150 GB | 450 GB | 1500 GB |
| Database | 1 | 3 | 10 | 25 | 50 |
| Custom Domain | ⛔ | ✅ | ✅ | ✅ | ✅ |
| Max Subserver | ⛔ | ⛔ | 8 | 25 | 50 |
| Multi Akun | ⛔ | ⛔ | ⛔ | 25 | 50 |


### Apa yang membedakan DOM Cloud dengan Provider Hosting Lainnya?

DOM Cloud menggunakan NGINX sebagai webserver, Virtualmin sebagai panel manager, mendukung dua jenis database (MySQL/MariaDB atau PostgreSQL), mendukung PHP serta bahasa lain (Python, Node.JS, Ruby, Go) melalui Phusion Passenger, menggunakan konfigurasi optimal secara default (HTTP/2.0, GZip, TLS v1.3, HTTP Etag Cache), mendukung FTP dan SSH serta interface online (Webmin File Manager, PhpMyAdmin/PhpPgAdmin dan Web SSH), minimum marketing dan bisa digunakan secara gratis untuk siapapun.

Dibandingkan provider hosting lainnya, kebanyakan masih menggunakan Apache, hanya mendukung MySQL, hanya mendukung PHP, konfigurasi web yang masih mentah, tidak mendukung SSH, banyak iklan atau elemen marketing dan tidak mempunyai opsi gratis atau trial sebelum membeli.

Meski melayani pembelian domain, DOM Cloud tidak melayani email, karena email sendiri beresiko tinggi terkena spam atau blacklist. Anda dapat menggunakan [ForwardEmail](https://forwardemail.net) atau [SendinBlue](https://sendinblue.com) sebagai alternatif layanan email gratis.

### Kenapa bisa begitu murah?

DOM Cloud tidak mempunyai staf tetap, dan tidak memiliki server khusus, sehingga semua keuntungan diambil dari DOM Cloud hanya digunakan untuk mengganti biaya server yang digunakan.

Karena hanya ditangani oleh satu orang, semua pelayanan dilakukan secara otomatis, termasuk proses registrasi, transfer, reminder, pembayaran, bahkan penghapusan akun. Anda harusnya dapat melakukan semua itu secara mandiri di portal.

### Apakah data saya aman?

Data anda akan aman berada di server selama anda tidak memberitahukan password hosting anda kepada orang lain. Meski berada dalam satu shared server hosting, satu akun tidak bisa membuka data akun lain karena cara Linux file permission bekerja dalam level kernel.

Lalu bagaimana dengan kami? Sudah kami jabarkan diawal, DOM Cloud tidak mempunyai staf khusus. Tidak ada yang dapat mengakses server dalam akses root kecuali saya sendiri, dan saya sendiri tidak tertarik untuk menggeledah isi server kecuali hal fatal atau darurat terjadi (contoh serangan DoS atau pelanggaran ToS).

Lalu bagaimana dengan keamanan website anda sendiri? Sudah menjadi kewajiban anda sendiri untuk membuat website anda aman dari serangan internet. Namun apabila anda terlanjur terkena serangan yang membuat data server anda rusak (misal RCE atau SQL Injection) anda masih dapat meminta bantuan untuk mengembalikan server ke point backup sebelum kejadian fatal tersebut terjadi.

## FAQ Tutorial Teknis

### Bagaimana Cara Mengedit Website?

Anda dapat masuk melalui [portal](https://portal.domcloud.id) lalu membuka tab "Login". Disitu anda dapat melihat beberapa macam kredensial login untuk mengakses:

+ **Webmin** untuk akses pengaturan utama hosting, termasuk upload file, mengatur akses database dan DNS.
+ **FTP** untuk akses file menggunakan aplikasi FTP (seperti FileZilla Client atau WinScpy).
+ **Database** untuk akses database menggunakan aplikasi MySQL atau PostgreSQL. Anda juga bisa menggunakan editor online PhpMyAdmin atau PhpPgAdmin.
+ **SSH** untuk akses terminal menggunakan aplikasi SSH (seperti OpenSSH Client atau PuTTY) atau SSH Online (menggunakan Web SSH).

### Apa itu Fitur Deployment?

Fitur ini digunakan untuk mengatur aspek yang tidak ada di Virtualmin. Diantaranya:

+ Menyalakan fitur SSL atau database MySQL/PostgreSQL.
+ Mengatur Server NGINX termasuk Alamat Root Dokumen.
+ Instalasi aplikasi server melalui perintah bash script.

Anda dapat mempelajari syntax deployment di repo [dom-templates](https://github.com/willnode/dom-templates/#readme).

### Bagaimana caranya memperbarui SSL?

Anda dapat deploy `features: ['ssl']` dan ia akan diperbarui segera.

Anda juga dapat melakukannya melalui Webmin > Server Configuration > SSL Configuration.

### Bagaimana caranya mengganti Versi PHP?

Anda dapat menggantinya di Webmin > Server Configuration > PHP Options.

Versi PHP yang tersedia: `5.6`, `7.4` (default), `8.0`.

Ada 2 opsi mode PHP yakni `FastCGI` dan `PHP-FPM` (default) namun sangat dianjurkan anda tidak mengubah pengaturan default ini.

### Bagaimana caranya memasang Framework PHP?

Nginx tidak menggunakan file `.htaccess`.

Biasanya anda perlu menginstall depedensi composer `composer install` melalui SSH (bila ada) lalu memusatkan server pada `index.php` di dalam folder root dan mengatur folder root dengan benar.

```yml
root: public_html/public
nginx:
  locations:
  - match: /
    try_files: $uri $uri/ /index.php$is_args$args
```

Root path tergantung pada framework masing-masing. Contoh WordPress atau CI 3 menggunakan `public_html` saja sedangkan Laravel atau CI 4 menggunakan `public_html/public`.

Jangan lupa untuk menghapus `index.html` bawaan.

### Bagaimana caranya mengatur pengaturan PHP (php.ini)?

Anda dapat meng-override pengaturan PHP melalui file `.user.ini` didalam document root. Setelah dirubah tidak akan berubah langsung karena cache, namun biasanya membutuhkan tidak lebih dari 5 menit.

### Bagaimana caranya memasang aplikasi non-PHP?

Setelah anda memasang depedensi yang dibutuhkan (misal `npm i` untuk Node.JS, `pip install` untuk Python, dll), anda dapat mengatur Nginx untuk memanggil aplikasi tersebut dengan cara:

```yml
root: public_html/public
nginx:
  passenger:
    enabled: on
```

Dari sini Passenger akan menjadi file startup seperti `app.js`, `passenger_wsgi.py` atau `config_ru` di dalam `public_html`. Jika struktur project anda tidak seperti ini anda juga dapat menggunakan Passenger Generic Support Language (GLS) seperti berikut:

```yml
root: public_html/public
nginx:
  passenger:
    enabled: on
    app_start_command: node app.js --port $PORT
    # disini $PORT adalah angka port yang bebas,
    # diatur secara otomatis oleh Passenger GLS
```

Runtime yang didukung dalam server DOM Cloud:

| Aplikasi | Keterangan |
|---|---|
| `php` / `php74` / `composer` | PHP 7.4 |
| `php56` | PHP 5.6 |
| `php80` | PHP 8.0 |
| `python` / `pip` <br> `python3.8` / `pip3.8` | Python 3.8 |
| `python3` / `pip3` <br>  `python3.6` / `pip3.6` | Python 3.6 |
| `node` / `npm` | NodeJS 14 |
| `ruby` | Ruby 2.5 |
| `go` | Go 1.14 |
| `rustc` / `cargo` | Rust 1.47 |


Aplikasi yang dijalankan melalui Passenger akan terus berjalan sampai inaktif dalam 5 menit. Apabila anda ingin merestart langsung server anda dapat merestartnya melalui SSH:

```bash
passenger-config restart-app /
```

Pelajari lebih banyak pengaturan Passenger di [official docs](https://www.phusionpassenger.com/docs/references/config_reference/nginx/).

### Bagaimana caranya menambah Enviroment Variables?

anda dapat menulisnya di `~/.env` lalu menulisnya seperti ini:

```bash
export ENVIRONMENT='development'
export FOO='bar'
```

### Bagaimana caranya mengarahkan DNS ke DOM Cloud?

Jika anda upgrade

## FAQ Troubleshooting

### Website down `ERR_NAME_NOT_RESOLVED`

Ini berarti ada masalah dengan sistem DNS. Jika anda menggunakan DOM Cloud pastikan:

+ Sudah memenuhi syarat/dokumen yang diperlukan (termasuk konfirmasi email) dari Registrar.
+ Sudah mengarahkan Name Server dengan benar (scroll ke atas jika belum).
+ Fitur DNS untuk server sudah menyala (atur melalui webmin).
+ Rekord A/CNAME untuk domain yang dituju sudah benar.

Anda dapat [mengontak kami](mailto:support@domcloud.id) apabila masih belum benar.

### HTTPS Error `ERR_CERT_AUTHORITY_INVALID`

Ini berarti sertifikat SSL anda belum diatur atau sudah expired. Untuk memperbaruinya silahkan [scroll ke atas](#bagaimana-caranya-memperbarui-ssl).

Jika anda menemui `ERR_CERTIFICATE_TRANSPARENCY_REQUIRED` setelah update SSL, anda tak perlu panik karena error dikarenakan cache dan tersebut akan hilang dalam beberapa menit.

### Halaman Nginx Error `403 Forbidden`

Ini berarti NGINX tidak dapat mengakses file tersebut karena kesalahan pengaturan mode linux dalam file tersebut. Agar dapat membetulkan ini dengan mudah anda dapat menjalankan ini di SSH:

```bash
chmod -R 750 ~/
```

### Halaman Nginx Error `404 Found`

Ada 2 kemungkinan:

+ Jika ini terjadi untuk semua URL halaman, kemungkinan anda lupa untuk mengatur NGINX untuk [memusatkan index.php]() atau [menyalakan mode passenger]() untuk non-PHP.
+ Jika ini terjadi untuk beberapa file saja, mungkin ada kesalahan seperti tipo di URL, salah Base URL atau tidak memperhatikan besar-kecil nama file.


### Halaman Kosong `HTTP Error 500`

Ini berarti ada error dalam PHP anda, namun tidak seperti XAMPP, secara default PHP tidak akan memunculkan error di website.

Anda mempunyai 2 pilihan untuk melihat error tersebut:
+ Melalui Webmin > Logs and Reports > Nginx Error Log
+ [Set di .user.ini](): `display_errors = On`

### Halaman Nginx Error `502 Bad Gateway`

Kemungkinan file yang anda upload terlalu besar atau script PHP yang anda jalankan terlalu lama. Anda bisa membetulkan ini melalui [file .user.ini]().

## Pertanyaan Lain

Silahkan gunakan [fasilitas diskusi](./discussion) :)
