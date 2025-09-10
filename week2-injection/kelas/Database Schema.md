# Database Schema

- Tujuan: Mengambil seluruh definisi skema database.
- Jenis Serangan: SQL Injection - Serangan UNION

Tantangan ini tidak bisa diselesaikan hanya dengan bypass login. Jadi perlu menemukan titik injeksi lain yang menampilkan output dari database kembali ke halaman. Tempat terbaik untuk ini adalah bilah pencarian (search bar).

## Analisis dan Langkah Pengerjaan
- Menemukan Jumlah Kolom:
  - Pergi ke bilah pencarian dan suntikkan payload untuk menentukan jumlah kolom.
  - Payload: https://localhost:3000/#/search?q=test') ORDER BY 10-- (mulai dari angka tinggi). Jika error, turunkan angkanya sampai tidak ada error.


Menemukan Kolom yang Menampilkan Data:
Gunakan UNION SELECT untuk melihat kolom mana yang ditampilkan.
Payload: blah') UNION SELECT 'a','b','c','d','e'-- (sesuaikan jumlah string dengan jumlah kolom yang ditemukan).
Anda akan melihat beberapa dari huruf tersebut muncul di hasil pencarian. Ini adalah kolom yang bisa kita gunakan untuk mengekstrak data. Mari kita asumsikan kolom ke-2 dan ke-3 menampilkan data.
Mengekstrak Skema (untuk SQLite):
SQLite memiliki tabel master khusus bernama sqlite_master yang berisi semua skema. Kolom sql di dalamnya berisi perintah CREATE TABLE lengkap.
Payload untuk mendapatkan semua skema tabel:
code
SQL
blah') UNION SELECT NULL,sql,NULL,NULL,NULL FROM sqlite_master--
Hasil: Anda akan melihat seluruh perintah CREATE TABLE untuk setiap tabel di database ditampilkan di hasil pencarian, yang secara efektif menyelesaikan tantangan untuk "mengambil seluruh definisi skema DB".
Write-up: Database Schema
Untuk mengekstrak skema database, kerentanan SQL Injection berbasis UNION pada fungsionalitas pencarian dieksploitasi. Pertama, jumlah kolom dalam query pencarian ditentukan menggunakan ORDER BY, yang mengungkapkan adanya lima kolom. Selanjutnya, payload UNION SELECT digunakan untuk mengidentifikasi kolom mana yang menampilkan data ke pengguna. Akhirnya, dengan menargetkan tabel sqlite_master (khusus untuk SQLite), payload ' UNION SELECT NULL,sql,NULL,NULL,NULL FROM sqlite_master-- disuntikkan. Ini mengekstrak konten dari kolom sql, yang berisi pernyataan CREATE TABLE lengkap untuk setiap tabel, sehingga seluruh skema database berhasil dieksfiltrasi dan ditampilkan di halaman hasil pencarian.
