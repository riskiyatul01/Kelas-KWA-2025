# Christmas Special

Tantangan ini berhasil diselesaikan dengan mengeksploitasi kerentanan SQL Injection berbasis UNION pada fungsionalitas pencarian produk. Tujuannya adalah untuk mengakses dan memesan item yang sudah tidak lagi ditawarkan atau telah dihapus dari katalog publik. Aplikasi ini kemungkinan besar menggunakan mekanisme soft-delete, di mana produk-produk usang tidak benar-benar dihapus dari database, melainkan ditandai sebagai tidak aktif (misalnya, melalui kolom deletedAt).

