# Login Admin

## Deskripsi
Log in with Jim's user account.

## Resource
https://portswigger.net/web-security/sql-injection

## Langkah - langkah
1. Cari user dengan nama "Jim" terlebih dahulu.
   <img width="1919" height="1086" alt="image" src="https://github.com/user-attachments/assets/bed20c2e-cf08-4c31-9084-85d5e3f57472" />
   Dari salah satu review, email dari user "Jim" adalah `jim@juice-sh.op`.

2. Masuk ke halaman login.
   <img width="1919" height="1077" alt="image" src="https://github.com/user-attachments/assets/7f3b2208-cb58-4533-a83c-b313a9d2b507" />

3. Setelah itu, masuk ke halaman login. Masukkan payload `jim@juice-sh.op' OR 1=1--` di kolom "Email". Untuk password bebas diisi apa saja.
   <img width="1917" height="1091" alt="image" src="https://github.com/user-attachments/assets/3f72ef9b-bad7-426a-bb6d-ab80255efbf4" />
   Hasilnya adalah sebagai berikut.
   <img width="1919" height="1089" alt="image" src="https://github.com/user-attachments/assets/7d203a22-7345-4417-b8a3-5e674f3b9326" />

## Catatan
Struktur query SQL yang rentan terhadap SQL injection biasanya seperti berikut.
```
SELECT * FROM users WHERE email = '$email' AND password = '$password'
```

Jika digunakan payload sebelumnya yang berhasil, maka hasil querynya adalah sebagai berikut.
```
SELECT * FROM users WHERE email = 'jim@juice-sh.op'--' AND password = 'aaaa'
```

Setelah email `jim@juice-sh.op`, ditambahkan comment setelah email yang mengabaikan password sehingga bisa login sebagai user Jim meskipun passwordnya salah.

Untuk soal ini juga bisa dikerjakan dengan Burp Suite untuk memodifikasi request. Tetapi karena langkah - langkahnya yang cukup mudah, soal ini bisa dikerjakan meski tidak menggunakan Burp Suite maupun tools lainnya.
<img width="1919" height="1142" alt="image" src="https://github.com/user-attachments/assets/e69119c3-26bb-43dd-9442-b9b8add40a5a" />

Setelah intercept request, ubah parameter "email" menjadi `jim@juice-sh.op'--`.
<img width="1919" height="1134" alt="image" src="https://github.com/user-attachments/assets/aabcbf4f-7465-4953-84c1-fd8b97db0aec" />

Hasil:
<img width="1919" height="1135" alt="image" src="https://github.com/user-attachments/assets/d7152dfb-a543-4919-8c78-d24d171f2ce6" />
