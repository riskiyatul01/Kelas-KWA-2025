# SSRF

## Tujuan Lab
Tujuan dari lab ini adalah menemukan fungsionalitas di aplikasi yang bisa disalahgunakan untuk membuat server mengirimkan permintaan HTTP ke dirinya sendiri (localhost). Dengan melakukan ini, kita akan mengakses panel admin yang tersembunyi dan tidak dapat diakses dari internet, lalu menggunakannya untuk melakukan tindakan terlarang (seperti menghapus pengguna).

## Langkah-langkah Penyelesaian
1. Login dan Navigasi ke Halaman Profil
  - Pertama, pastikan sudah login di OWASP Juice Shop.
  - Klik menu "Account" di pojok kanan atas, lalu pilih "User Profile". 

<img width="1088" height="753" alt="image" src="https://github.com/user-attachments/assets/d73d3645-3664-4f83-9680-fb346c11eda6" />

2. Identifikasi Titik Masuk Serangan
  - titik masuknya adalah kolom input berlabel "Image URL". Fungsionalitas ini dimaksudkan agar pengguna bisa menautkan gambar dari internet, tetapi kita akan menyalahgunakannya.

3. Menemukan Target Internal (Apa yang Harus Diakses?)
  - Salah satu tantangan SSRF yang paling umum di Juice Shop adalah mengakses file yang "terlupakan" di dalam direktori FTP server. Jadi, target kita adalah path /ftp/.
  - file targetnya adalah acquisitions.md

<img width="1439" height="367" alt="image" src="https://github.com/user-attachments/assets/94884e8a-eabf-44d1-9666-8f967779b4e4" />

4. Merancang Payload SSRF
  - Sekarang gabungkan alamat internal (localhost) dengan path target (/ftp/acquisitions.md).

<img width="641" height="64" alt="image" src="https://github.com/user-attachments/assets/681cf4fd-8e53-4a88-8071-7dfee5d89a42" />

5. Eksekusi Serangan
  - Kembali ke halaman User Profile.
  - Di dalam kotak "Image URL", hapus URL yang sudah ada dan tempelkan (paste) payload yang sudah dirancang.
  - Klik tombol "Link Image".

<img width="682" height="152" alt="image" src="https://github.com/user-attachments/assets/5c72ad86-1c36-438b-ac29-6600ef074478" />

6. Verifikasi Keberhasilan
  - Aplikasi akan mencoba mengambil "gambar" dari URL yang Anda berikan. Tentu saja, server akan gagal menampilkan file .md sebagai gambar, jadi gambar profil mungkin akan rusak atau tidak berubah.
Jika serangan berhasil,akan melihat notifikasi pop-up hijau di bagian atas layar yang memberitahu Anda bahwa telah menyelesaikan sebuah tantangan.

