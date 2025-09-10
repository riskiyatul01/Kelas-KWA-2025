<img width="1181" height="537" alt="image" src="https://github.com/user-attachments/assets/49ae6ae9-9694-485d-93a2-c6ed235679ce" /># Login Jim

- Tujuan: Login menggunakan akun pengguna "Jim".
- Jenis Serangan: SQL Injection - Bypass Otentikasi Bertarget

## Analisis dan Langkah Pengerjaan

Tahap 1: Pengintaian (Reconnaissance) - Menemukan Email Jim
- Jelajahi Aplikasi: Titik awal terbaik untuk mencari informasi pengguna adalah di area di mana pengguna berinteraksi. Di aplikasi e-commerce seperti Juice Shop, tempat terbaik adalah ulasan produk (product reviews).
- Cari Ulasan: Buka halaman detail untuk beberapa produk dan periksa bagian ulasan. Cari ulasan yang ditulis oleh pengguna dengan nama "Jim".
- Temukan Email: Aplikasi Juice Shop, secara default, menampilkan alamat email pengguna yang memberikan ulasan. Anda akan menemukan sesuatu seperti:

*Ulasan oleh: jim@juice-sh.op*

<img width="1181" height="537" alt="image" src="https://github.com/user-attachments/assets/887df476-755c-4db9-b0b6-9982cad90b2f" />


Tahap 2: Eksekusi Serangan - Bypass Login
- Buka Halaman Login: Navigasikan ke halaman login aplikasi.
- Masukkan Payload:
  - Di kolom Email, masukkan: jim@juice-sh.op'--
  - Di kolom Password, masukkan teks apa saja (misalnya, 12345).
- Cara Kerja Payload:
  - jim@juice-sh.op: Mengidentifikasi baris pengguna yang benar di database.
  - ': Menutup string email dalam query SQL.
  - --: Mengomentari sisa query, secara efektif menonaktifkan bagian AND password = '...'.
  - Kirim Form: Klik tombol login. Aplikasi akan memvalidasi keberadaan email, mengabaikan pemeriksaan password, dan memberi akses ke akun Jim.

<img width="723" height="484" alt="image" src="https://github.com/user-attachments/assets/b7a40b18-de56-4f3a-955c-865d6d7f8f93" />


<img width="537" height="262" alt="image" src="https://github.com/user-attachments/assets/58a4515f-6075-44fd-85ba-130991a416c8" />

Untuk mendapatkan akses ke akun Jim, serangan SQL Injection dua tahap dilakukan. Tahap pertama adalah pengintaian, di mana alamat email target (jim@juice-sh.op) ditemukan dengan memeriksa data yang tersedia untuk umum di bagian ulasan produk. Tahap kedua adalah eksekusi, di mana payload '-- ditambahkan ke akhir email di form login. Payload ini berhasil memanipulasi query database untuk melewati validasi password, sehingga memberikan akses tidak sah ke akun Jim hanya dengan mengetahui alamat emailnya.
