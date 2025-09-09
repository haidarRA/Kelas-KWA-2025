# Login Bender

## Deskripsi
Log in with Bender's user account.

## Resource
https://portswigger.net/web-security/sql-injection

## Langkah - langkah
1. Cari user dengan nama "Bender" terlebih dahulu.
   <img width="1919" height="1088" alt="image" src="https://github.com/user-attachments/assets/027d6c5c-8a73-4de9-98b0-cda82af26f78" />
   Dari salah satu review, email dari user "Bender" adalah `bender@juice-sh.op`.

2. Masuk ke halaman login.
   <img width="1919" height="1077" alt="image" src="https://github.com/user-attachments/assets/7f3b2208-cb58-4533-a83c-b313a9d2b507" />

3. Setelah itu, masuk ke halaman login. Masukkan payload `bender@juice-sh.op'--` di kolom "Email". Untuk password bebas diisi apa saja.
   <img width="1919" height="1082" alt="image" src="https://github.com/user-attachments/assets/1daff0a2-a493-4081-837e-35e0ff89b790" />
   Hasilnya adalah sebagai berikut.
   <img width="1919" height="1088" alt="image" src="https://github.com/user-attachments/assets/6be4782d-86ca-48e9-b2f2-e09db748339d" />

## Catatan
Struktur query SQL yang rentan terhadap SQL injection biasanya seperti berikut.
```
SELECT * FROM users WHERE email = '$email' AND password = '$password'
```

Jika digunakan payload sebelumnya yang berhasil, maka hasil querynya adalah sebagai berikut.
```
SELECT * FROM users WHERE email = 'bender@juice-sh.op'--' AND password = 'aaaa'
```

Setelah email `bender@juice-sh.op`, ditambahkan comment setelah email yang mengabaikan password sehingga bisa login sebagai user Bender meskipun passwordnya salah.

Untuk soal ini juga bisa dikerjakan dengan Burp Suite untuk memodifikasi request. Tetapi karena langkah - langkahnya yang cukup mudah, soal ini bisa dikerjakan meski tidak menggunakan Burp Suite maupun tools lainnya.
<img width="1919" height="1138" alt="image" src="https://github.com/user-attachments/assets/09e74ffc-c771-4634-8d41-b8addcb225b7" />

Setelah intercept request, ubah parameter "email" menjadi `bender@juice-sh.op'--`.
<img width="1919" height="1140" alt="image" src="https://github.com/user-attachments/assets/cc8de883-c9e6-4511-ba77-6b18375e879b" />

Hasil:
<img width="1919" height="1134" alt="image" src="https://github.com/user-attachments/assets/3c88eb95-87c0-420f-825c-840a49fc89e7" />
