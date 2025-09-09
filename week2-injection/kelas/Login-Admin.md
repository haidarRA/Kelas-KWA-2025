# Login Admin

## Deskripsi
Log in with the administrator's user account.

## Resource
https://portswigger.net/web-security/sql-injection

## Langkah - langkah
1. Masuk ke halaman login terlebih dahulu.
   <img width="1919" height="1077" alt="image" src="https://github.com/user-attachments/assets/7f3b2208-cb58-4533-a83c-b313a9d2b507" />

2. Setelah itu, masuk ke halaman login. Masukkan payload `administrator' OR 1=1--` di kolom "Username". Untuk password bebas diisi apa saja.
   <img width="1919" height="1088" alt="image" src="https://github.com/user-attachments/assets/85e37169-61be-4ee3-a2f3-25c114904bb2" />
   Hasilnya adalah sebagai berikut.
   <img width="1918" height="1082" alt="image" src="https://github.com/user-attachments/assets/430382a0-7286-4c0e-8a39-96b29cf1251f" />

## Catatan
Struktur query SQL yang rentan terhadap SQL injection biasanya seperti berikut.
```
SELECT * FROM users WHERE username = '$username' AND password = '$password'
```

Jika digunakan payload sebelumnya yang berhasil, maka hasil querynya adalah sebagai berikut.
```
SELECT * FROM users WHERE username = 'administrator' OR 1=1--' AND password = 'aaaa'
```

Setelah username administrator, ditambahkan `OR 1=1--` yang menyebabkan nilai menjadi `true` dan adanya comment di belakang yang mengabaikan password sehingga bisa login sebagai administrator meskipun passwordnya salah.

Untuk soal ini juga bisa dikerjakan dengan Burp Suite untuk memodifikasi request. Tetapi karena langkah - langkahnya yang cukup mudah, soal ini bisa dikerjakan meski tidak menggunakan Burp Suite maupun tools lainnya.
<img width="1919" height="1134" alt="image" src="https://github.com/user-attachments/assets/bbd71aff-746d-4146-8eec-55a485b13f7b" />

Setelah intercept request, ubah parameter "username" menjadi `administrator'--`.
<img width="1919" height="1139" alt="image" src="https://github.com/user-attachments/assets/077bd8f2-af07-4a74-81ad-845f27dd944d" />

Hasil:
<img width="1919" height="1138" alt="image" src="https://github.com/user-attachments/assets/b93e3977-59ff-4ada-aae6-d93c14438559" />
