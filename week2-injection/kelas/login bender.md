# login bender

- Tujuan: Login menggunakan akun pengguna "Bender".
- Jenis Serangan: SQL Injection - Bypass Otentikasi Bertarget

## Analisis dan Langkah Pengerjaan

Tahap 1: Pengintaian (Reconnaissance) - Menemukan Email Bender

- Kembali ke Ulasan Produk: Lanjutkan menjelajahi ulasan-ulasan produk di seluruh situs.
- Cari Ulasan Bender: Secara spesifik, cari ulasan yang ditinggalkan oleh pengguna bernama "Bender".

<img width="619" height="165" alt="image" src="https://github.com/user-attachments/assets/8dfba00a-5841-42ce-857e-bf2c2d65ef88" />

Tahap 2: Eksekusi Serangan - Bypass Login
- Buka Halaman Login: Kembali ke halaman login.
- Masukkan Payload:
  - Di kolom Email, masukkan: bender@juice-sh.op'--
  - Di kolom Password, masukkan teks acak.
- Analisis Payload: Payload ini bekerja dengan cara yang sama persis seperti sebelumnya, menargetkan baris data milik Bender di tabel pengguna dan menonaktifkan pemeriksaan password.
- Kirim Form: Klik login. sekarang akan masuk sebagai pengguna Bender.

<img width="764" height="439" alt="image" src="https://github.com/user-attachments/assets/e2e85bef-cb10-4538-83cb-364cb8d8d92b" />

<img width="644" height="284" alt="image" src="https://github.com/user-attachments/assets/19d467d2-af97-4196-8f14-4e7f02f9854d" />


Tantangan ini diselesaikan dengan Pertama, alamat email pengguna "Bender" (bender@juice-sh.op) diidentifikasi dengan menganalisis ulasan produk di dalam aplikasi. Selanjutnya, kerentanan SQL Injection pada halaman login dieksploitasi dengan menyuntikkan payload bender@juice-sh.op'-- ke dalam field email. Serangan ini berhasil mengelabui logika backend untuk mengabaikan validasi password, yang mengarah pada pengambilalihan akun Bender yang berhasil.
