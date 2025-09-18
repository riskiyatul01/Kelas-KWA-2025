# User ID controlled by request parameter, with unpredictable user IDs

link soal : https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-unpredictable-user-ids

## Tujuan Lab
Tujuannya masih sama: melakukan horizontal privilege escalation untuk mendapatkan API key milik carlos. Namun, kali ini aplikasi tidak lagi menggunakan username yang mudah ditebak (carlos) sebagai ID di URL. Sebagai gantinya, aplikasi menggunakan GUID (Globally Unique Identifier), yaitu serangkaian karakter acak yang panjang dan tidak mungkin ditebak

## Langkah-langkah Penyelesaian
1. Cari Tahu GUID Milik 'carlos' (Information Gathering)
  - Jangan login dulu. Pertama, perlu mencari informasi di bagian publik situs.
  - Buka halaman utama lab. Biasanya ada bagian blog atau postingan artikel.
  - Cari postingan blog yang ditulis oleh carlos. Klik untuk membuka postingan tersebut.
  - Di dalam halaman postingan, nama penulis (carlos) biasanya berupa tautan (link). Klik pada nama carlos.
  - Perhatikan URL di address bar browser. Browser akan mengarahkan ke halaman arsip penulis, dan URL-nya akan berisi GUID milik carlos. Akan terlihat seperti ini:
```
https://0a010081031bfb99818e93c900570089.web-security-academy.net/blogs?userId=58d43727-de2b-4276-824b-84fd1bc8603a
```
  - Salin (copy) GUID yang panjang dan acak tersebut. Inilah ID unik milik carlos.

<img width="1073" height="655" alt="image" src="https://github.com/user-attachments/assets/6793dd6f-07fb-49b1-ac02-b50c5036feae" />

<img width="1352" height="834" alt="image" src="https://github.com/user-attachments/assets/4a068c30-0a72-491e-9ca1-324810b7741d" />

2. Login dan Akses Halaman Akun Anda
  - Sekarang setelah Anda memiliki ID target, login ke situs menggunakan kredensial yang diberikan:
    - Username: wiener
    - Password: peter
  - Klik pada "My account". Perhatikan URL-nya. disini akan melihat ID sendiri (yang juga berupa GUID) di parameter id. Ini mengonfirmasi bahwa pola kerentanannya masih sama.
```
https://0a010081031bfb99818e93c900570089.web-security-academy.net/my-account?id=63acbed5-494b-420e-95b6-2a78b90195f9
```

<img width="1296" height="585" alt="image" src="https://github.com/user-attachments/assets/733392a3-daf0-47ea-875c-ff1d57bef3e2" />

3. Manipulasi URL dengan GUID 'carlos'
  - Ganti GUID di URL dengan GUID milik carlos yang sudah disalin pada Langkah 1.
  - Tekan Enter.

<img width="1258" height="511" alt="image" src="https://github.com/user-attachments/assets/76980e31-0bbd-41d7-a781-155c7f2682f4" />

4. Ambil API Key dan Selesaikan Lab
  - Halaman sekarang akan menampilkan detail akun milik carlos.
  - Temukan API Key miliknya, salin, dan kirimkan sebagai solusi untuk menyelesaikan lab.

<img width="936" height="360" alt="image" src="https://github.com/user-attachments/assets/67d3b0a3-b512-49b0-b073-88743f134f0a" />

<img width="1328" height="373" alt="image" src="https://github.com/user-attachments/assets/0b1896dd-2d8e-44cf-9d56-c36b91207045" />

## Dampak:
Dampaknya tetap kritis. Seorang penyerang dapat secara sistematis mengumpulkan GUID dari semua pengguna yang aktif di bagian publik situs (seperti penulis blog atau pemberi komentar) dan kemudian menggunakan kerentanan IDOR untuk mengakses dan mencuri data pribadi dari semua akun tersebut.
