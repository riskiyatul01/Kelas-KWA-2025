# User ID controlled by request parameter with data leakage in redirect

link soal : https://portswigger.net/web-security/access-control/lab-user-id-controlled-by-request-parameter-with-data-leakage-in-redirect

## Tujuan Lab
Tujuannya adalah untuk mendapatkan API key milik carlos. Kerentanannya masih berupa IDOR (mengganti ID di parameter), tetapi kali ini aplikasi akan mencoba mengalihkan (redirect) kita saat kita mencoba mengakses akun pengguna lain. Kunci untuk menyelesaikan lab ini adalah dengan memeriksa apa yang terjadi selama proses pengalihan tersebut.

## Langkah langkah penyelesaian

1. Login dan Intercept Permintaan
  - Konfigurasikan browser Anda untuk menggunakan Burp Suite sebagai proxy.
  - Nyalakan mode Intercept di Burp Suite (Intercept is on).
  - Login ke lab menggunakan kredensial:
    - Username: wiener
    - Password: peter
Klik pada "My account". Burp Suite akan menangkap permintaan HTTP untuk halaman tersebut. Permintaannya akan terlihat seperti ini:

<img width="1481" height="626" alt="image" src="https://github.com/user-attachments/assets/a55253e9-3867-4147-9311-5514949497cf" />

2. Kirim Permintaan ke Repeater
  - klik kanan di dalam jendela permintaan dan pilih "Send to Repeater" (Kirim ke Repeater).
  - Sekarang matikan mode Intercept (Intercept is off) agar browser bisa digunakan seperti biasa.
  
3. Modifikasi dan Kirim Ulang Permintaan
  - Di dalam Repeater, ubah nilai parameter id dari wiener menjadi carlos.
  - Klik tombol "Send" (Kirim).

<img width="1428" height="553" alt="image" src="https://github.com/user-attachments/assets/108d8e57-d83d-4114-a550-9a2e0e918590" />

4. Analisis Respons yang Bocor
  - Lihat panel respons di sebelah kanan. disini akan melihat beberapa hal penting:
    - Status Code: HTTP/1.1 302 Found. Ini adalah kode untuk redirect.
    - Header: Akan ada header Location: / yang memerintahkan browser untuk pindah ke halaman utama.
    - Response Body Gulir ke bawah ke bagian body/isi dari respons. Meskipun ini adalah respons redirect, disini akan melihat seluruh kode HTML dari halaman akun milik carlos ada di sini! Server telah memuat data sensitifnya sebelum memutuskan untuk melakukan redirect.

<img width="710" height="245" alt="image" src="https://github.com/user-attachments/assets/1f940eb8-afed-4b2c-aada-b551fe53f2f5" />

5. Ambil API Key dan Selesaikan Lab
  - Di dalam response body tersebut, cari API key milik carlos.
  - Salin API key tersebut, lalu kirimkan sebagai solusi untuk menyelesaikan lab.

<img width="768" height="282" alt="image" src="https://github.com/user-attachments/assets/cccb904a-0c14-4dea-939a-a92df0f366ca" />

<img width="1176" height="196" alt="image" src="https://github.com/user-attachments/assets/41a09cb5-6b66-4f60-90a6-303d31a076f9" />

## Dampak:
Meskipun ada upaya untuk mencegah akses tidak sah, data sensitif (seperti API key, detail pribadi, dll.) dari setiap pengguna tetap bocor melalui jaringan. Penyerang dapat mengotomatiskan proses ini untuk mencuri data dari semua pengguna di sistem.
