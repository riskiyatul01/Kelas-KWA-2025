# SSRF

Membuat server aplikasi untuk meminta (request) sebuah sumber daya tersembunyi atas nama kita.

## Langkah-langkah Penyelesaian dengan Burp Suite
Temukan Fitur yang Rentan:
Kerentanan SSRF di Juice Shop ada pada fitur untuk mengimpor gambar dari URL. Fitur ini hanya tersedia untuk pengguna Deluxe. Anda harus menjadi anggota Deluxe terlebih dahulu.
Login sebagai pengguna Deluxe, lalu cari fitur tersebut (biasanya terkait dengan avatar profil atau sejenisnya).
Nyalakan Intercept di Burp.
Tangkap dan Analisis Request:
Masukkan URL gambar yang valid (misalnya https://example.com/image.png) ke dalam fitur tersebut dan submit.
Burp akan menangkap request POST ke sebuah endpoint API (misalnya /api/ImageUrl) yang body-nya berisi JSON dengan URL yang Anda masukkan: {"url": "https://example.com/image.png"}.
Lakukan Serangan SSRF:
Kirim request tersebut ke Repeater (Ctrl+R).
Di Repeater, kita akan memodifikasi URL tersebut untuk memaksa server mengakses file internal yang tidak bisa kita akses dari luar. Targetnya adalah http://localhost:3000/ftp/coupons.md.
Ubah request body menjadi: {"url": "http://localhost:3000/ftp/coupons.md"}.
Klik "Send".
Verifikasi:
Server akan mematuhi perintah Anda, mengakses file coupons.md dari localhost-nya sendiri, dan mengembalikan isinya di dalam response yang Anda terima di Repeater. Dengan membaca isi file tersembunyi ini, tantangan pun selesai.
Dampak dan Kerentanan
Kerentanan: SSRF (Server-Side Request Forgery). Aplikasi secara buta mempercayai URL yang diberikan pengguna dan membuat permintaan ke URL tersebut dari sisi server.
Dampak Singkat: Ini adalah kerentanan kritis. Penyerang bisa memindai jaringan internal perusahaan, mengakses database atau layanan internal yang tidak terproteksi, membaca file sensitif di server, dan di lingkungan cloud, SSRF bisa digunakan untuk mencuri credentials dan mengambil alih seluruh infrastruktur cloud.
