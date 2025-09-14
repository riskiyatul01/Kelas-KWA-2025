# Easter Egg

## Deskripsi Tantangan

Menemukan easter egg yang tersembunyi.

## Langkah-langkah Penyelesaian dengan Burp Suite
1. Eksplorasi Direktori:
  - Tantangan ini adalah tentang menemukan konten tersembunyi. Petunjuknya seringkali ada pada file-file yang diekspos oleh server.
  - Saat  menjelajahi aplikasi, Burp di HTTP history akan mencatat semua file yang dimuat. Perhatikan struktur URL-nya.
  - Coba akses direktori /ftp/ langsung dari browser (http://localhost:3000/ftp/).

<img width="1832" height="440" alt="image" src="https://github.com/user-attachments/assets/b7ec7866-db5b-4e26-9506-d6d13fac2038" />

2. Temukan File Tersembunyi:
  - Saat mengakses /ftp/, server Juice Shop akan menampilkan directory listing (daftar isi direktori) karena konfigurasinya yang tidak aman.
  - Di dalam daftar file tersebut, Anda akan menemukan file yang tidak biasa bernama eastere.gg.

<img width="330" height="46" alt="image" src="https://github.com/user-attachments/assets/1ff963c3-0c87-4d65-b6e1-d6ce98b6ac2d" />


3. Akses Easter Egg:
  - Akses file tersebut secara langsung di browser dengan URL: http://localhost:3000/ftp/eastere.gg.

<img width="1611" height="521" alt="image" src="https://github.com/user-attachments/assets/913a1b8f-abfc-4918-9602-f6eec163efc3" />

4. jika error tambahkan pada URL : http://localhost:3000/ftp/eastere.gg%2500.md, karena disini mintannya harus .md atau .pdf
   
<img width="1758" height="167" alt="image" src="https://github.com/user-attachments/assets/a6329966-f47a-4cd8-9d5e-b7bbbf0fb014" />

<img width="1069" height="98" alt="image" src="https://github.com/user-attachments/assets/0041fc50-4832-4eec-86e7-ab715d8a6473" />


## Dampak dan Kerentanan
  - Kerentanan: Information Disclosure akibat directory listing yang aktif dan penempatan file sensitif di direktori yang dapat diakses publik.
  - Dampak: Meskipun menemukan easter egg itu sendiri tidak berbahaya, kerentanan ini bisa mengekspos file-file sensitif seperti backup kode sumber, file konfigurasi dengan database credentials, atau data pelanggan yang tidak sengaja ditempatkan di sana.
