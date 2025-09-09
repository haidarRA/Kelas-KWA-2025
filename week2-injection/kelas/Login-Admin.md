# Login Admin

## Deskripsi
Log in with the administrator's user account.

## Resource
https://portswigger.net/web-security/sql-injection

## Langkah - langkah
1. Masuk ke halaman login terlebih dahulu.
   <img width="1919" height="1077" alt="image" src="https://github.com/user-attachments/assets/7f3b2208-cb58-4533-a83c-b313a9d2b507" />

2. Setelah itu, masuk ke halaman login. Masukkan payload `admin@juice-sh.op' OR 1=1--` di kolom "Email". Untuk password bebas diisi apa saja.
   <img width="1919" height="1088" alt="image" src="https://github.com/user-attachments/assets/1dab76ae-374a-4ee5-ae46-0d6fbaaeb59e" />
   Hasilnya adalah sebagai berikut.
   <img width="1918" height="1082" alt="image" src="https://github.com/user-attachments/assets/430382a0-7286-4c0e-8a39-96b29cf1251f" />

## Catatan
Struktur query SQL yang rentan terhadap SQL injection biasanya seperti berikut.
```
SELECT * FROM users WHERE email = '$email' AND password = '$password'
```

Jika digunakan payload sebelumnya yang berhasil, maka hasil querynya adalah sebagai berikut.
```
SELECT * FROM users WHERE email = 'admin@juice-sh.op' OR 1=1--' AND password = 'aaaa'
```

Setelah email administrator, ditambahkan `OR 1=1--` yang menyebabkan nilai menjadi `true` dan adanya comment di belakang yang mengabaikan password sehingga bisa login sebagai administrator meskipun passwordnya salah.

Untuk soal ini juga bisa dikerjakan dengan Burp Suite untuk memodifikasi request. Tetapi karena langkah - langkahnya yang cukup mudah, soal ini bisa dikerjakan meski tidak menggunakan Burp Suite maupun tools lainnya.
<img width="1919" height="1131" alt="image" src="https://github.com/user-attachments/assets/27f26d09-08e9-4354-8cec-895708ef24d3" />

Setelah intercept request, ubah parameter "email" menjadi `admin@juice-sh.op' OR 1=1--`.
<img width="1919" height="1134" alt="image" src="https://github.com/user-attachments/assets/0cf49c22-c112-4152-8282-90e56e51b639" />

Hasil:
<img width="1919" height="1138" alt="image" src="https://github.com/user-attachments/assets/b93e3977-59ff-4ada-aae6-d93c14438559" />
