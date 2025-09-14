# Manipulate Basket

Tujuan tantangan ini adalah menambahkan sebuah produk ke dalam keranjang belanja (shopping basket) milik pengguna lain.

## Langkah langkah pengerjaan

1. Tangkap Request Penambahan Keranjang
  - Di halaman utama, klik tombol "Add to Basket" pada produk apa pun.
  - Burp akan menangkap request POST ke /api/BasketItems/.

<img width="695" height="29" alt="image" src="https://github.com/user-attachments/assets/205daeaa-0bbe-4f0e-8809-2f62181546bc" />

2. Modifikasi dan Kirim Ulang Request
  - Kirim request yang ditangkap ke Repeater .
  - Pindah ke tab Repeater.
  - Di panel Request, lihat bagian body. akan menemukan JSON seperti:

<img width="605" height="96" alt="image" src="https://github.com/user-attachments/assets/7373d9e5-463d-459e-b348-13613532ff79" />

3. Ubah nilai dari key "BasketId" menjadi ID keranjang pengguna lain. Coba "1" (untuk admin) atau "2" (untuk Jim).
  - Klik tombol "Send" di Repeater.
  - Verifikasi Hasil
  - Jika panel Response menunjukkan status sukses (200 OK), berarti produk berhasil ditambahkan ke keranjang pengguna lain. Tantangan pun akan langsung terselesaikan.

<img width="1315" height="447" alt="image" src="https://github.com/user-attachments/assets/808df80e-94f2-4c1d-ac65-5458d911e1a3" />

4. jika sudah succes maka tantangan sudah berhasil

<img width="1329" height="117" alt="image" src="https://github.com/user-attachments/assets/717e24dc-4f7c-4bb1-b7c6-d42334cbbc32" />
