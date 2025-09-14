# Product Tampering

Mengubah tautan (href) di dalam deskripsi produk "OWASP SSL Advanced Forensic Tool (O-Saft)" menjadi https://owasp.slack.com.

# Langkah-langkah Penyelesaian dengan Burp Suite
1. Dapatkan Data Produk Asli:
  - Login ke Juice Shop dan pastikan browser terhubung melalui Burp Suite Proxy.
  - Cari detail semua produk biasanya terdapat pada GET /rest/products/search?q=

  - isi ulasan bebas pada produk OWASP SSL Advanced Forensic Tool (O-Saft) dan hidupkan intercept di burpsuite
  - Temukan request berikut

<img width="795" height="41" alt="image" src="https://github.com/user-attachments/assets/848320c8-8a63-41e6-a90c-d39077838c8b" />



<img width="537" height="308" alt="image" src="https://github.com/user-attachments/assets/a93511ba-601b-42c1-b5fc-74f0141e2b6d" />


2. Modifikasi dan Kirim Ulang Request:
  - Klik kanan pada request GET tadi di HTTP history dan pilih "Send to Repeater" .
  - Pindah ke tab Repeater.
  - Ubah metode request dari GET menjadi PUT.
  - Tempelkan (paste) seluruh data JSON yang disalin tadi ke dalam request body di Repeater.
  - Di dalam JSON tersebut, cari key "description" dan ubah nilai href-nya menjadi https://owasp.slack.com.
  - Pastikan header Content-Type: application/json dan Authorization: Bearer <token> sudah ada.
  - Klik "Send".

3. Verifikasi:
  - Jika Anda mendapatkan respons 200 OK, berarti data produk berhasil diubah. Tantangan pun akan terselesaikan.

## Dampak dan Kerentanan
- Kerentanan: Broken Access Control. API untuk mengubah data produk (PUT) tidak memvalidasi apakah pengguna memiliki hak akses admin, sehingga pengguna biasa pun bisa melakukannya.
- Dampak : Penyerang dapat mengubah harga produk (menyebabkan kerugian finansial), menyisipkan tautan phishing di deskripsi, atau merusak halaman produk (defacement), yang merusak reputasi dan kepercayaan pelanggan.
