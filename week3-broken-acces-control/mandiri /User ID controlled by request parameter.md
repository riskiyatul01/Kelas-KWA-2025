# User ID controlled by request parameter

link soal : https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter

## Tujuan Lab
Tujuan dari lab ini adalah untuk mengeksploitasi kerentanan horizontal privilege escalation. Kita akan masuk sebagai pengguna biasa (wiener), kemudian memanipulasi parameter di URL untuk mengakses halaman akun pengguna lain (carlos) dan mencuri API key miliknya.

# Langkah-langkah Penyelesaian
1. Login ke Akun yang Disediakan
  - Akses lab dan masuk menggunakan kredensial yang diberikan:
    - Username: wiener
    - Password: peter

  <img width="1053" height="463" alt="image" src="https://github.com/user-attachments/assets/32686186-dfc2-4970-aa10-d91efe6b9dd9" />


2. Buka Halaman Akun dan Amati URL
  - Setelah berhasil login, klik pada tautan "My account" (Akun saya).
  - Sekarang, perhatikan baik-baik URL di address bar browser. URL tersebut terlihat seperti ini:
```
https://nama-lab-anda.net/my-account?id=wiener](https://0abc0032032f7d1781e2ace3004700f8.web-security-academy.net/my-account?id=wiener
```
  - Perhatikan bagian ?id=wiener. Ini adalah petunjuk besar. Aplikasi ini tampaknya menggunakan parameter id di URL untuk menentukan data pengguna mana yang akan ditampilkan. Ini adalah praktik yang tidak aman karena parameter ini dapat dikontrol oleh pengguna.

<img width="1274" height="809" alt="image" src="https://github.com/user-attachments/assets/722cd178-065a-42cf-a4df-42d24f7998c6" />

3. Manipulasi Parameter id
  - Karena kita tahu target kita adalah pengguna carlos, kita bisa mencoba mengganti nilai parameter id di URL.
  - Ubah wiener menjadi carlos langsung di address bar browser. URL baru akan menjadi:
```
https://nama-lab-anda.net/my-account?id=wiener](https://0abc0032032f7d1781e2ace3004700f8.web-security-academy.net/my-account?id=carlos
```
  - Tekan Enter untuk memuat halaman dengan URL yang sudah dimodifikasi.

<img width="1199" height="780" alt="image" src="https://github.com/user-attachments/assets/4ed7bc9a-e676-4a62-b326-98696dc15228" />

4. Ambil API Key dan Selesaikan Lab
  - Jika aplikasi rentan, halaman sekarang akan menampilkan detail akun milik carlos, meskipun masih login sebagai wiener.
  - Cari API Key di halaman tersebut. Salin (copy) API key tersebut.
  - Klik tombol "Submit solution" (Kirim solusi), tempel  API key yang sudah  disalin, dan kirimkan untuk menyelesaikan lab.

<img width="1073" height="369" alt="image" src="https://github.com/user-attachments/assets/8df6c416-3eb5-4648-94f0-72aca5a09b4a" />

<img width="1575" height="388" alt="image" src="https://github.com/user-attachments/assets/d6c4b69a-915f-4ca6-a470-3e74994818bd" />

## Dampak:
Seorang penyerang dapat mengeksploitasi kerentanan ini untuk mengakses data sensitif dari semua pengguna lain di sistem hanya dengan menebak atau mengulang (iterating) nilai parameter id (misalnya, id=user1, id=user2, id=admin). Ini dapat menyebabkan kebocoran data pribadi massal, seperti email, alamat, dan kredensial lainnya (seperti API key).
