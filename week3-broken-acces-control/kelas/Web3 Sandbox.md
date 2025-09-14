# Web3 Sandbox

Tujuan tantangan ini adalah menemukan halaman sandbox (lingkungan uji coba) yang tidak sengaja ikut terpasang di aplikasi. Halaman ini berfungsi untuk menulis dan menguji smart contract secara langsung.

## Langkah-langkah Penyelesaian

1. Buka Developer Tools:
  - Pilih tab "Sources" (di Chrome) atau "Debugger" (di Firefox).
2. Cari di File JavaScript:
  - Di panel file, cari file JavaScript utama aplikasi, seringkali bernama main.js atau main.[hash].js.
3. Temukan Path Halaman:
  - Gunakan fitur pencarian (tekan Ctrl+F atau Cmd+F) di dalam panel kode.
  - Cari kata kunci yang relevan dengan tantangan. Kata kunci yang paling efektif adalah sandbox atau solidity .

<img width="555" height="93" alt="image" src="https://github.com/user-attachments/assets/ce83e344-da1a-4c22-9a80-52fff6cf4e2d" />

4. Akses Halaman Sandbox:
  - Dari kode tersebut, kita mengetahui bahwa path atau jalur URL untuk halaman yang kita cari adalah web3-sandbox.
  - Sekarang, tambahkan jalur ini ke URL Juice Shop di address bar :
```
http://localhost:3000/#/web3-sandbox
```
  - Tekan Enter. Halaman sandbox akan terbuka, dan Anda akan langsung menyelesaikan tantangan ini.

<img width="1497" height="750" alt="image" src="https://github.com/user-attachments/assets/0bf51bca-1734-4c2f-a08e-12c9f529fa2c" />

## Dampak dan Kerentanan
- Kerentanan: Information Disclosure (Pengungkapan Informasi) dan Broken Access Control. Fitur yang seharusnya hanya untuk pengembangan internal diekspos ke publik dan dapat diakses oleh siapa saja yang mengetahui URL-nya.
- Dampak: Penyerang bisa menemukan dan mengeksploitasi fungsi-fungsi tersembunyi yang mungkin tidak seaman fitur utama. Ini dapat membuka celah untuk serangan lebih lanjut atau memberikan informasi tentang teknologi yang digunakan oleh aplikasi.
