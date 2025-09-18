# Unprotected admin functionality with unpredictable URL

link soal : https://portswigger.net/web-security/access-control/lab-unprotected-admin-functionality-with-unpredictable-url

## Tujuan Lab
Tujuannya adalah menemukan lokasi panel admin yang tersembunyi dengan cara memeriksa kode sumber (source code) halaman web, lalu mengakses panel tersebut untuk menghapus pengguna carlos.

## Langkah-langkah Penyelesaian

1. Buka Halaman Lab dan Periksa Kode Sumber
  - Akses lab seperti biasa. Karena URL admin tidak bisa ditebak, kita perlu mencari petunjuk di dalam kode halaman itu sendiri.
  - Klik kanan di mana saja pada halaman utama lab dan pilih "Inspect" (Inspeksi) untuk membuka Developer Tools.
    
2. Cari URL Admin di dalam Kode JavaScript
  - Setelah halaman kode sumber terbuka, kita perlu mencari di mana URL admin disembunyikan. Seringkali, informasi dinamis seperti ini dimuat atau didefinisikan dalam skrip JavaScript.
  - Gunakan fitur pencarian (biasanya Ctrl + F atau Cmd + F) di dalam tab kode sumber dan cari kata kunci seperti admin, panel, atau path.
  - disini akan menemukan sebuah skrip JavaScript yang mendefinisikan variabel atau objek. Di dalamnya, akan ada path URL yang terlihat acak yang merujuk ke panel admin.
kode yang ditemukan:
```
/admin-qdpx2r

```
Salin path yang terlihat acak tersebut.

<img width="1732" height="750" alt="image" src="https://github.com/user-attachments/assets/1f507313-5034-412f-8ddb-70f4a442353d" />

3. Akses Panel Admin dan Hapus Pengguna 'carlos'
  - Sekarang setelah memiliki path rahasianya, tambahkan path tersebut ke akhir URL utama lab.
  - Di dalam panel admin, temukan daftar pengguna, cari pengguna carlos, dan klik tombol untuk menghapusnya.

<img width="1343" height="458" alt="image" src="https://github.com/user-attachments/assets/9fa29b28-fdac-416e-b00a-4fa9b1517138" />

<img width="1420" height="363" alt="image" src="https://github.com/user-attachments/assets/61c6d3d7-c3ec-4608-9568-8dbfb1216802" />

Setelah pengguna carlos berhasil dihapus, lab akan selesai.

## Dampak:
Dampaknya sama seriusnya dengan lab sebelumnya. Setelah penyerang menemukan URL rahasia tersebut, mereka dapat mengakses panel admin tanpa memerlukan autentikasi apa pun. Ini memberikan mereka hak istimewa sebagai administrator, yang memungkinkan mereka untuk:
  - Menghapus atau memodifikasi data pengguna.
  - Mencuri informasi pribadi.
  - Mengontrol fungsionalitas dan konten situs web.
