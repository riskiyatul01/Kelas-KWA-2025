# view basket

Tujuan dari tantangan ini adalah untuk dapat melihat isi keranjang belanja (shopping basket) milik pengguna lain. Tantangan ini menguji bagaimana aplikasi mengelola akses ke data pengguna dan apakah ada celah yang memungkinkan satu pengguna melihat data milik pengguna lain.

## Langkah-langkah Penyelesaian

1. Persiapan Awal
  - Buka aplikasi OWASP Juice Shop. buka dibagian keranjang.
  - Tambahkan setidaknya satu barang ke keranjang belanja. Ini akan membuat aplikasi membuat sesi keranjang belanja untuk Anda dan menyimpannya.

![WhatsApp Image 2025-09-11 at 10 45 23_68e40b08](https://github.com/user-attachments/assets/13623923-b600-4160-8686-1b719c948e7e)


2. Investigasi Penyimpanan Sisi Klien (Client-Side Storage)
  - Buka Developer Tools (biasanya dengan menekan F12 atau klik kanan lalu "Inspect").
  - Pilih tab "Application"
  - Di panel sebelah kiri, di bawah bagian "Storage", perluas "Session Storage" dan pilih URL dari Juice Shop.

![WhatsApp Image 2025-09-11 at 10 45 50_3ee73e96](https://github.com/user-attachments/assets/a57104a6-88af-40af-af49-240c5d86e3ab)

3. Menemukan dan Memanipulasi Basket Identifier (ID Keranjang)
  - Di dalam Session Storage, disini ada daftar key-value pairs.
  - Nilai (value) dari bid ini adalah nomor identifikasi unik untuk keranjang belanja saat ini.

4. Mengeksploitasi Kerentanan
  - bid saat ini adalah 1. 
  - Klik dua kali pada nilai bid di dalam Developer Tools untuk mengeditnya.
  - Ubah nilainya menjadi angka lain. disini saya mengubahnya menjadi 2
  - Setelah mengubah nilainya, muat ulang (refresh) halaman atau navigasikan kembali ke halaman keranjang belanja (/#/basket).

![WhatsApp Image 2025-09-11 at 10 43 59_b389dd78](https://github.com/user-attachments/assets/9c0d407f-e092-42b3-bfe0-6de488da2e35)

![WhatsApp Image 2025-09-11 at 10 44 32_5c5fc146](https://github.com/user-attachments/assets/9ef1898c-6bbf-4a46-94c9-d362eb4ed26f)

Aplikasi sekarang membaca bid yang telah modifikasi dari Session Storage dan akan menampilkan isi keranjang belanja yang sesuai dengan ID tersebut, yang kemungkinan besar milik pengguna lain. 
