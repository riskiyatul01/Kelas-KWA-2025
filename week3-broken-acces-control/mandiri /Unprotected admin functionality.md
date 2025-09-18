# Unprotected admin functionality
 
 link soal : https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality

Lab ini menunjukkan kerentanan umum di mana sebuah situs web secara tidak sengaja mengekspos lokasi panel adminnya, sehingga siapa pun dapat mengaksesnya tanpa perlu login.

## Tujuan Lab
Tujuan dari lab ini adalah untuk menemukan panel admin yang tidak terproteksi dan menggunakannya untuk menghapus pengguna bernama carlos.

## Langkah-langkah Penyelesaian

1. Mencari petunjuk di robots.txt
  - File robots.txt adalah file teks yang ditempatkan di direktori root sebuah situs web untuk memberitahu web crawler (seperti bot Google) halaman mana yang tidak boleh mereka akses atau indeks.
  - Terkadang, pengembang web mendaftarkan direktori sensitif, seperti panel admin, di file ini dengan harapan agar tidak muncul di hasil pencarian. Namun, ini justru menjadi petunjuk bagi siapa saja yang ingin tahu di mana direktori tersembunyi itu berada.
  - Untuk melihat file ini, tambahkan /robots.txt di akhir URL .
  - Di dalam file robots.txt tersebut, Anda akan menemukan baris yang berisi Disallow:. Baris ini akan menunjukkan path atau alamat dari panel admin yang disembunyikan. Berdasarkan gambar, path tersebut kemungkinan besar adalah /administrator-panel.

<img width="893" height="119" alt="image" src="https://github.com/user-attachments/assets/7280614b-19d1-4ca6-b8c6-8ad63f96ae1f" />

2. Mengakses Panel Admin
  - Setelah me+ngetahui lokasi panel admin dari file robots.txt, disini bisa langsung mengaksesnya.
  - Ganti /robots.txt di URL dengan path yang ditemukan.
  - Karena panel ini tidak terproteksi, jadi akan langsung masuk ke halaman admin tanpa perlu memasukkan username atau password.

<img width="1590" height="531" alt="image" src="https://github.com/user-attachments/assets/5b4cf629-b8c6-4bd3-a4ff-a0bc52d900af" />

3. Menghapus Pengguna 'carlos'
  - Di dalam panel admin, Anda akan melihat daftar pengguna situs tersebut.
  - Cari pengguna dengan nama carlos.
  - Akan ada tombol atau tautan untuk menghapus pengguna tersebut. Klik tombol tersebut untuk menghapus carlos.

<img width="1554" height="407" alt="image" src="https://github.com/user-attachments/assets/95468a5a-a1dc-44b6-b90d-0d2024f0db06" />

Setelah berhasil menghapus pengguna carlos, lab akan dianggap selesai.

## Dampak:
Dengan mengakses panel admin yang tidak terproteksi, seorang penyerang memiliki hak akses penuh setara dengan administrator. Mereka dapat melakukan tindakan berbahaya seperti:
  - Menghapus data pengguna (seperti yang dilakukan dalam lab ini dengan menghapus pengguna carlos).
  - Mengubah atau mencuri informasi sensitif pengguna lain.
  - Mengubah konten situs web.
  - Mengambil alih seluruh fungsionalitas situs.
