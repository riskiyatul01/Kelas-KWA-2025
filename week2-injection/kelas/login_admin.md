# Login admin

Tujuan: Login menggunakan akun administrator.
Jenis Serangan: SQL Injection - Bypass Otentikasi

Analisis dan Langkah Pengerjaan
- Asumsi Query: Query login di backend kemungkinan besar terlihat seperti ini:
```
SELECT * FROM Users WHERE email = '[EMAIL_INPUT]' AND password = '[PASSWORD_INPUT]'
```
- Payload: Kita perlu membuat bagian WHERE dari query menjadi TRUE untuk pengguna admin, sambil menonaktifkan pemeriksaan password. Payload yang paling umum dan efektif adalah:
- Email: admin@juice-sh.op'-- (atau email admin yang sesuai jika diketahui).
- Password: Apa saja (misalnya, password).

Cara Kerja Payload:
- admin@juice-sh.op: Ini adalah email target.
- ' : Tanda kutip ini "menutup" string email di dalam query.
- -- : Ini adalah karakter komentar SQL, yang menyebabkan sisa query (yaitu, AND password = '...') diabaikan sepenuhnya oleh database.
- Query yang Dieksekusi:
```
SELECT * FROM Users WHERE email = 'admin@juice-sh.op'--' AND password = '...'
```
- Database hanya akan memproses `SELECT * FROM Users WHERE email = 'admin@juice-sh.op'`, menemukan pengguna admin, dan karena tidak ada pemeriksaan password, aplikasi akan memberikan akses.

<img width="600" height="460" alt="image" src="https://github.com/user-attachments/assets/dae06ba9-d3b8-4e94-9cb3-ad82e49d06fe" />

<img width="782" height="198" alt="image" src="https://github.com/user-attachments/assets/e7509cd1-d8c6-4aa5-b5f4-83ccc5313411" />

Kerentanan SQL Injection pada fungsi login memungkinkan bypass otentikasi. Dengan memasukkan payload admin@juice-sh.op'-- ke dalam field email, query SQL di backend dimanipulasi. Tanda kutip tunggal (') menutup string email, dan karakter komentar (--) menonaktifkan klausa pengecekan password. Akibatnya, sistem hanya memvalidasi keberadaan email admin@juice-sh.op dan memberikan akses tanpa memerlukan password yang benar, sehingga berhasil login sebagai administrator.
