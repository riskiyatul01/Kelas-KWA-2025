# Product Tampering

Mengubah tautan (href) di dalam deskripsi produk "OWASP SSL Advanced Forensic Tool (O-Saft)" menjadi https://owasp.slack.com.

# Langkah-langkah Penyelesaian dengan Burp Suite
1. Dapatkan Data Produk Asli:
  - Login ke Juice Shop dan pastikan browser terhubung melalui Burp Suite Proxy.
  - Cari detail semua produk biasanya terdapat pada GET /rest/products/search?q=
  - copy payload yang produk OWASP SSL Advanced Forensic Tool (O-Saft) tersebut untuk langkah selanjutnya

![WhatsApp Image 2025-09-14 at 23 19 54_6d9509d1](https://github.com/user-attachments/assets/543e8a2d-1088-4a66-ab50-9a99f4c32852)

2. Modifikasi dan Kirim Ulang Request:
  - isi ulasan bebas pada produk OWASP SSL Advanced Forensic Tool (O-Saft) dan hidupkan intercept di burpsuite
  - Temukan request berikut

<img width="795" height="41" alt="image" src="https://github.com/user-attachments/assets/848320c8-8a63-41e6-a90c-d39077838c8b" />

  - Klik kanan pada request PUT tadi di HTTP history dan pilih "Send to Repeater" .
  - Pindah ke tab Repeater.
  - Tempelkan (paste) seluruh data JSON yang disalin tadi ke dalam request body di Repeater.
  - Di dalam JSON tersebut, cari key "description" dan ubah nilai href-nya menjadi https://owasp.slack.com.
  - Ubah bagian URL nya menjadi api/Products/9
  - Klik "Send".

![WhatsApp Image 2025-09-14 at 23 37 58_ce6ac05e](https://github.com/user-attachments/assets/b8cde2ba-d106-470e-a9d4-9de936feb140)


3. Verifikasi:
  - Jika Anda mendapatkan respons 200 OK, berarti data produk berhasil diubah. Tantangan pun akan terselesaikan.

![WhatsApp Image 2025-09-14 at 23 38 22_3dd880fb](https://github.com/user-attachments/assets/081f4096-75f9-435d-a8f3-6e74f0e4d9a3)


## Dampak dan Kerentanan
- Kerentanan: Broken Access Control. API untuk mengubah data produk (PUT) tidak memvalidasi apakah pengguna memiliki hak akses admin, sehingga pengguna biasa pun bisa melakukannya.
- Dampak : Penyerang dapat mengubah harga produk (menyebabkan kerugian finansial), menyisipkan tautan phishing di deskripsi, atau merusak halaman produk (defacement), yang merusak reputasi dan kepercayaan pelanggan.
