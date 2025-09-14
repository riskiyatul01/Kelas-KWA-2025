# CSRF

Tujuan tantangan ini adalah mengubah nama pengguna (username) dengan melakukan serangan Cross-Site Request Forgery (CSRF) dari "origin" atau situs web yang berbeda.

## Langkah-langkah Penyelesaian

1. Identifikasi Request yang Rentan
Konfigurasi Burp Suite: Pastikan browser Anda sudah dikonfigurasi untuk menggunakan Burp Suite sebagai proxy. Buka Burp dan pastikan intercept dalam keadaan off untuk sementara.
Login ke Juice Shop: Login dengan akun pengguna biasa.
Aksi yang Akan Diserang: Cari fungsi untuk mengubah nama pengguna. Di Juice Shop, ini tidak ada di halaman profil, melainkan di API whoami yang secara internal mengambil data pengguna, termasuk username. Kita akan menyerang API yang mengubah username.
Tangkap Request:
Buka tab Proxy -> HTTP history di Burp Suite.
Untuk menemukan endpoint yang benar, kita perlu mengubah data pengguna. Kita bisa menggunakan endpoint yang sudah kita ketahui dari tantangan sebelumnya atau menemukannya. Endpoint untuk mengubah data pengguna adalah PUT /api/Users/{id}. Namun, untuk CSRF, seringkali endpoint yang lebih sederhana menjadi target. Dalam kasus ini, endpoint untuk mengubah password adalah target yang baik karena juga mengubah data pengguna.
Pergi ke halaman "Change Password".
Nyalakan Intercept di Burp Suite.
Masukkan password saat ini dan password baru, lalu klik "Change".
Burp Suite akan menangkap request PUT ke /api/Users/change-password?userId=... atau sejenisnya. Namun, mari kita fokus pada mengubah username. Endpoint yang lebih relevan adalah PUT /api/Users/{your-user-id}.
Untuk mendapatkan request ini, cara termudah adalah dengan melakukan aksi yang relevan. Jika tidak ada UI untuk mengubah nama, kita bisa membuatnya sendiri. Namun, deskripsi tantangan mengarah pada perubahan nama. Mari asumsikan endpoint-nya adalah PUT /api/Users/{id} dengan payload {"username": "new-name"}.
2. Buat Bukti Konsep (Proof of Concept) CSRF
Temukan request yang relevan di Proxy -> HTTP history di Burp. Mari kita gunakan request PUT /api/Users/7 (jika ID Anda 7) yang mengubah data.
Klik kanan pada request tersebut.
Pilih Engagement tools -> Generate CSRF PoC.
Burp akan membuka jendela baru yang berisi kode HTML. Kode ini akan membuat sebuah halaman web dengan formulir yang, ketika disubmit, akan mengirim ulang request yang sama persis dengan yang Anda tangkap.
3. Lakukan Serangan
Pastikan Anda Masih Login: Serangan CSRF hanya berhasil jika korban (dalam hal ini, Anda sendiri) memiliki sesi aktif (sudah login) di situs target (Juice Shop) di browser yang sama.
Di jendela "Generate CSRF PoC" di Burp, klik tombol "Test in browser".
Burp akan memberikan URL. Salin dan tempel URL tersebut ke browser Anda.
Halaman HTML sederhana akan muncul dengan tombol "Submit request".
Klik tombol "Submit request".
Halaman ini (yang berasal dari origin Burp, bukan Juice Shop) akan mengirimkan request ke server Juice Shop. Karena browser Anda secara otomatis menyertakan cookie sesi untuk Juice Shop, server akan mengira request ini sah dan memprosesnya.
Nama pengguna Anda akan berubah, dan tantangan CSRF pun selesai.

