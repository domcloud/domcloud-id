---
layout: docs
---

# Tips dan Pemecahan masalah

## Tips Umumnya dan Praktek

#### Portal

- Skrip runner tersedia sebagai cara alternatif untuk mengelola berbagai hal di Virtualmin, bahkan untuk hal-hal yang tidak tersedia di sana, seperti mengonfigurasi NginX atau Firewall.
- Skrip runner dapat dipicu dengan GitHub Actions CI, berguna untuk menyinkronkan kode host secara otomatis dengan repo GitHub.
- Kami tidak melayani transaksi email, namun Anda dapat menggunakan pihak ketiga.

#### NginX

- Gunakana `ssl: enforce` untuk mengalihkan semua lalu lintas yang tidak aman ke yang aman.
- NginX sendiri mampu membuat `E-Tag` cache untuk mempercepat pengiriman file statis.
- Kami memiliki batas kecepatan burst bawaan dari 50 permintaan untuk 3 permintaan/detik di setiap alamat IP.

#### PHP-FPM

- Anda dapat memilih versi PHP baik dari Virtualmin atau skrip runner.
- Untuk mengonfigurasi `php.ini`, buat `.user.ini` file di jalur root publik.
- Ukuran unggahan default adalah `8MB`, tingkatkan dengan set `upload_max_filesize` dan `post_max_size` di `php.ini`.
- Setiap kesalahan fatal dalam PHP akan menghasilkan kosong `500` error. Anda dapat mengaktifkan pelaporan kesalahan dengan mengatur `display_errors` menjadi 1 di `php.ini`, tapi ini tidak direkomendasikan.
- Saat memasang dependensi dengan composer, gunakan `--no-cache` untuk menghindari penyimpanan yang terbuang oleh cache.

#### Python

- `python` dan `pip` disebut sebagai Python 3. Kami tidak mendukung Python 2.
- Saat memasang dependensi dengan pip, selalu gunakan `--user` ika tidak, Anda akan mendapatkan kesalahan pemasangan karena persyaratan Sudo.

#### Passenger Phusion

- Passenger Phusion Node.JS mencari `app.js`, `passenger_wsgi.py` atau `config.ru` di induk jalur root untuk memulai aplikasi.
- Fitur GLS dari Passenger Phusion dapat memulai aplikasi apa pun, bahkan file biner.
- Passenger Phusion Node.JS bekerja dengan CJS. Jika proyek Anda menggunakan ESM, Anda perlu menggunakan GLS.
- Passenger Phusion akan diaktifkan jika `passenger_enabled on` dan tidak ada file yang ada di jalur path.
- Anda dapat mengetahui apakah Phusion menyajikan file dengan memeriksa `server: nginx + Phusion Passenger` HTTP header.
- File statis seringkali lebih baik ditangani dalam NginX saja untuk memanfaatkan `E-Tag` cache.
- Anda dapat menambahkan environment variables dengan menulis di `~/.env` (dan gunakan perintah seperti ini: `export NODE_ENV=production`)

## Kesalahan Umum dan penanganannya

Untuk PHP, Error logs tersedia di log kesalahan yang disediakan oleh Virtualmin.

Untuk Non-PHP (melalui Phusion Passenger), log error tidak tersedia tetapi jika aplikasi Anda gagal untuk memulai, penjelasan rinci yang bagus untuk kerusakan tersedia.

#### Website down `ERR_NAME_NOT_RESOLVED`

Ini berarti ada masalah dengan sistem DNS. Jika Anda menggunakan DOM Cloud pastikan:

- Telah memenuhi persyaratan/dokumen yang dipersyaratkan (termasuk email konfirmasi) dari Registrar .
- Sudah mengarahkan Name Server dengan benar (scroll ke atas jika belum).
- Fitur DNS untuk server dihidupkan (diatur melalui webmin).
- The A / CNAME untuk domain yang dimaksud sudah benar.

Kamu bisa [Hubungi kami](mailto:support@domcloud.id) jika masih eror.

#### HTTPS Error `ERR_CERT_AUTHORITY_INVALID`

Ini berarti bahwa sertifikat SSL Anda belum disetel atau telah kedaluwarsa. Untuk memperbaruinya, tolong [scroll kebawah](#how-to-renew-ssl).

Jika Anda bertemu `ERR_CERTIFICATE_TRANSPARENCY_REQUIRED` setelah update SSL tidak perlu panik karena error karena cache dan akan hilang dalam beberapa menit.

#### Nginx Error `403 Forbidden` page

Ini berarti NGINX tidak dapat mengakses file karena kesalahan pengaturan mode linux dalam file. Untuk memperbaikinya dengan mudah, Anda dapat menjalankan ini di SSH:

`bash chmod -R 750 ~/`

#### Nginx Error page `404 Found`

Ada 2 kemungkinan:

- Jika ini terjadi untuk semua URL halaman, kemungkinan Anda lupa menyetel NGINX ke [setup index.php](#how-to-install-php-framework) atau [turn on passenger mode](#passenger) untuk non-PHP.
- Jika ini terjadi hanya untuk beberapa file, mungkin ada kesalahan seperti salah ketik pada URL, URL Base salah atau tidak memperhatikan ukuran nama file.

#### Blank page `HTTP Error 500`

Artinya ada error pada PHP anda, namun tidak seperti XAMPP, PHP secara default tidak akan memunculkan error pada website.

Anda memiliki 2 opsi untuk melihat kesalahan:

- Via Webmin > Logs and Reports > Nginx Error Log
- [Set in .user.ini](#fastcgi): `display_errors = On`

#### Nginx Error `502 Bad Gateway` page

Ada kemungkinan file yang Anda unggah terlalu besar atau skrip PHP yang Anda jalankan terlalu lama. Anda dapat memperbaikinya melalui[.user.ini](#fastcgi) file.

#### "Connection Refused" from server HTTP request

Anda harus mematikan firewall.
