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

- Passenger Phusion Node.JS looks for `app.js`, `passenger_wsgi.py` or `config.ru` in parent of root path to start the app.
- The GLS feature from Passenger Phusion can start any app, even binary files.
- Passenger Phusion Node.JS works with CJS. If your project use ESM you need to use GLS.
- Passenger Phusion will be activated if `passenger_enabled on` and no files present in destination path.
- You can tell if Phusion is serving a file by checking `server: nginx + Phusion Passenger` HTTP header.
- Static files is often better to be handled within NginX alone to make use of `E-Tag` cache.
- You can add environment variables by writing in `~/.env` (and use commands like this: `export NODE_ENV=production`)

## Common Errors Troubleshooting

For PHP, Error logs is available in error logs provided by Virtualmin.

For Non-PHP (via Phusion Passenger), error logs is not available but if your app is failed to start up, a nice detailed explanation for the crash is available.

#### Website down `ERR_NAME_NOT_RESOLVED`

This means that there is a problem with the DNS system. If you are using DOM Cloud make sure:

- Has fulfilled the requirements / required documents (including email confirmation) from the Registrar.
- Already pointing the Name Server correctly (scroll up if not already).
- The DNS feature for the server is turned on (set up via webmin).
- The A / CNAME records for the intended domain are correct.

You can [contact us](mailto:support@domcloud.id) if it is still not correct.

#### HTTPS Error `ERR_CERT_AUTHORITY_INVALID`

This means that your SSL certificate has not been set or has expired. To update it please [scroll up](#how-to-renew-ssl).

If you encounter `ERR_CERTIFICATE_TRANSPARENCY_REQUIRED` after an SSL update, you don't need to panic because of an error due to cache and it will disappear in a few minutes.

#### Nginx Error `403 Forbidden` page

This means that NGINX cannot access the file due to a linux mode setting error in the file. In order to fix this easily you can run this on SSH:

`bash chmod -R 750 ~/`

#### Nginx Error page `404 Found`

There are 2 possibilities:

- If this happens for all page URLs, chances are you forgot to set NGINX to [setup index.php](#how-to-install-php-framework) or [turn on passenger mode](#passenger) for non-PHP.
- If this happens for only a few files, there may be errors such as typo in the URL, wrong Base URL or not paying attention to the file name size.

#### Blank page `HTTP Error 500`

This means that there is an error in your PHP, but unlike XAMPP, PHP by default will not raise an error on the website.

You have 2 options for viewing the error:

- Via Webmin > Logs and Reports > Nginx Error Log
- [Set in .user.ini](#fastcgi): `display_errors = On`

#### Nginx Error `502 Bad Gateway` page

It is possible that the file you uploaded is too large or the PHP script you are running is taking too long. You can fix this via [.user.ini](#fastcgi) file.

#### "Connection Refused" from server HTTP request

You need to turn off the firewall.
