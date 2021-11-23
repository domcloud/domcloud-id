---
layout: docs
---

# Selamat Datang di Dokumentasi DOM Cloud

Kami menggunakan GitHub untuk [Melacak Masalah](https://github.com/domcloud/domcloud-io/issues) dan [Diskusi](https://github.com/domcloud/domcloud-io/discussions). Untuk pertanyaan pribadi, silakan kirim email ke [support@domcloud.id](mailto:support@domcloud.id).

## Apa itu DOM Cloud? Dan mengapa?

Ini adalah platform sebagai layanan untuk meng-hosting konten Anda ke internet menggunakan virtual machine. Pada dasarnya, DOM Cloud mirip seperti penyedia hosting lainya, tetapi sangat sesuaikan untuk aplikasi modern.

Server DOM Cloud tersedia di NYC (New York City), SGA (Singapura), JPN (Jepang). Semua server memiliki domain default yaitu `*.domcloud.io`.

Untuk gambaran umum, DOM Cloud memungkinkan Anda untuk:

- Host aplikasi web PHP atau Non-PHP
- Kelola Database MySQL atau PostgreSQL
- Menjalankan aplikasi web yang ada menggunakan menggunakan script deployments
- mensinkronkan aplikasi pengembangan web Anda gunakan dengan GitHub

To understand why DOM Cloud exist, we need to compare with existing hosting solutions...

### Untuk Memahami Mengapa DOM Cloud ada, kita perlu membandingkan dengan hosting yang ada...

hosting lainya yang ada biasanya menggunakan Apache (atau \[Open\]LiteSpeed). Apache berkembang pesat dalam bisnis hosting karena portabilitas file konfigurasi dalam bentuk file `.htaccess`. Memang benar bahwa Apache adalah pilihan yang umum, tetapi saat ini, telah dilampaui dengan NginX.

NginX memberikan kinerja yang lebih baik dan kesederhanaan konfigurasi untuk situs web modern. Di era Apache, PHP adalah pilihan yang jelas untuk digunakan dalam pengembangan situs web, terutama dengan WordPress yang menguasai 37% internet. Tetapi hari ini, kami memiliki banyak hal yang dibangun dengan berbagai bahasa seperti Node.JS, Ruby, Go atau Python. NginX sekarang adalah untuk ini, dan sekitar 2019, NginX melampaui penggunaan global di atas Apache.

Tidak seperti penyedia hosting umum, DOM Cloud menggunakan NginX dan dapat menghosting aplikasi non-PHP Anda bahkan secara gratis. Lebih baik lagi, Anda juga dapat menggunakan Postgres melalui MariaDB (MySQL) jika Anda menginginkannya (kami mendukung keduanya).

### Perbandingan serverless hosting providers

[Serverless hosting](https://en.wikipedia.org/wiki/Serverless_computing) providers unggul dalam kinerja web karena dapat direplikasi di seluruh dunia. Namun dengan kemampuan replikasi tersebut, server web harus "stateless" atau "tidak dapat diubah", yang berarti Anda tidak dapat menggunakan file lokal untuk apa pun dan koneksi terus-menerus seperti WebSocket tidak mungkin. Selain itu, serverless deployments dari AWS memiliki batas ukuran server moderat 256 MB.

Ini sebagian besar baik untuk bahasa modern, tetapi untuk bahasa lama yang sangat bergantung pada operasi file seperti PHP, sangat sulit untuk mulai bekerja. Pada dasarnya Anda juga harus mengandalkan solusi eksternal untuk menyimpan database atau bahkan sesi data. Tergantung pada skala proyek Anda, serverless mungkin merupakan pilihan yang tepat, atau mungkin tidak.

DOM Cloud adalah solusi hosting bersama dan _statelessness_ atau _immutability_ bukanlah persyaratan. Anda dapat memeriksa dan mengedit file di tempat, menyiapkan tugas cron, atau melakukan debug cepat dengan SSH dalam produksi.

## Untuk memulai

Anda dapat mulai menghosting situs web dari [portal](https://portal.domcloud.id/en/login). Kemudian:

1. Buat akun atau login
2. Klik "Orde Baru", ini akan membuka formulir untuk membuat host baru.
3. Pilih Username, ini juga akan digunakan pada nama domain default dengan postfix `.domcloud.io`
4. Pilih lokasi Server, pilih lokasi terdekat dari audiens target Anda
5. Pilih template. Anda dapat membiarkannya menjadi proyek kosong, mulai dari template (seperti WordPress) atau kerangka kerja lainnya.
6. Jika Anda setuju dengan paket gratis, lewati ke langkah 9, jika tidak, lanjutkan membaca...
7. Pilih paket. Paket selain gratis akan memberi Anda pilihan untuk memilih domain khusus
8. Pilih jenis domain, biarkan default, gunakan domain yang ada atau beli yang baru.
9. Cek total harga, lalu klik "Order Now".
10. Hosting Anda akan segera dimulai atau setelah pembayaran.

## Manajeman Hosts

Berikut menu yang tersedia:

- Info: Informasi dasar dan statistik tentang host (menunjukkan IP host, penyimpanan, dan bandwidth yang digunakan)
- Manage: Halaman otentikasi untuk mengelola data host (pembuka gerbang ke Virtualmin/DB/SSH/FTP)
- Deploy: Deploy management dan skrip runner (untuk mengotomatiskan instalasi dan tugas lainnya)
- Check:
  - Invoice: Memeriksa status pembayaran dan sisa waktu tenggang
  - DNS: Memeriksa dan Identifikasi masalah umum dengan DNS
  - Firewall:Memeriksa opsi firewall
  - Nginx: Memeriksa dan ubah konfigurasi atau log NginX
  - Passenger: Memeriksa Status dan Log
- Admin:
  - Change Username
  - Change Domain
  - Upgrade
  - Extend
  - Transfer
  - Delete

## Virtualmin

portal virtualmin memberi Anda kemampuan untuk mengelola file, subserver, dan database.

The [Virtualmin page](/docs/virtualmin) menjelaskan hal-hal yang dapat Anda lakukan dengannya.

## SSH

Akses SSH tersedia untuk semua paket dan Anda dapat menggunakan klien SSH lokal atau menggunakan WebSSH. SSH berguna jika Anda ingin menjalankan perangkat lunak tertentu menggunakan antarmuka CLI atau Bash.

Anda juga dapat menggunakan [the runner script](/docs/runner) untuk mengotomatiskan tugas-tugas tertentu yang biasanya Anda lakukan dengan SSH.
