# Forged Feedback

Tujuan dari tantangan ini adalah untuk mengirimkan sebuah feedback (umpan balik) atas nama pengguna lain. Tantangan ini menguji apakah aplikasi melakukan validasi yang benar di sisi server untuk memastikan bahwa pengguna yang mengirimkan data adalah pengguna yang sama dengan yang sedang login.

## Langkah-langkah Penyelesaian

1. Login dan Temukan Halaman Feedback
  - Pertama, perlu login ke aplikasi dengan akun pengguna mana pun.
  - Buka menu samping (ikon hamburger) dan navigasikan ke halaman "Customer Feedback".

![WhatsApp Image 2025-09-11 at 11 00 55_727012cb](https://github.com/user-attachments/assets/0fefa713-9455-46a8-94a8-3aee6de7730b)

2. Intersep Proses Pengiriman Feedback
  - Buka Developer Tools (F12) dan pilih tab "Network".
  - Di halaman "Customer Feedback", isi semua kolom yang diperlukan:
    - Comment: Tulis komentar apa saja.
    - Rating: Pilih jumlah bintang.
    - Captcha: Selesaikan soal matematika sederhana.
- klik tombol "Submit".

![WhatsApp Image 2025-09-11 at 11 05 35_c5ada57c](https://github.com/user-attachments/assets/5b21cda1-d9dd-461d-8c10-3a5fc0771878)

3. Analisis Request yang Dikirim
  - Setelah menekan "Submit", lihat di tab "Network". disini akan melihat sebuah request baru muncul, disini bisa langsung ke endpoint /api/Feedbacks/.
  - Klik pada request tersebut untuk melihat detailnya dan klik pada bagian payload.
  - Di dalam payload, disini melihat data yang Anda kirimkan dalam format JSON

![WhatsApp Image 2025-09-11 at 11 08 41_682fe9d3](https://github.com/user-attachments/assets/d2cfd334-0ccf-4903-8948-8b1cc2ba9f33)

4. Identifikasi dan Modifikasi User ID
  - Perhatikan parameter "UserId": 1. Angka 1 ini adalah ID unik dari pengguna yang sedang digunakan untuk login.
  - Untuk memalsukan feedback, kita perlu mengubah UserId ini menjadi ID milik pengguna lain.

5. Mengirim Ulang Request yang Sudah Dimodifikasi
  - Buka tab "Console" di Developer Tools.
  - Kita akan menggunakan fetch API untuk mengirim ulang request dengan UserId yang sudah dimodifikasi. 
```
fetch('/api/Feedbacks/', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'Authorization': 'Bearer ' + sessionStorage.getItem('token') 
  },
  body: JSON.stringify({
    captchaId: 0, 
    captcha: "16", 
    comment: "halooo",
    rating: 1,
    UserId: 2
  })
}).then(response => response.json())
  .then(data => console.log(data));
```

  - Setelah menjalankan perintah di atas, server akan memproses feedback tersebut seolah-olah berasal dari pengguna dengan UserId: 2

![WhatsApp Image 2025-09-11 at 11 17 56_db1764b7](https://github.com/user-attachments/assets/81a67786-bb4f-4497-adf5-ccf5b8c4b969)

![WhatsApp Image 2025-09-11 at 11 18 51_595c610d](https://github.com/user-attachments/assets/b4840b4a-316b-4900-bfc8-951d46608375)

