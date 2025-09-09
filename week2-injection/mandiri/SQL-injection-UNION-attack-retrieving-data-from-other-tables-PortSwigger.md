# Lab: SQL injection UNION attack, retrieving data from other tables

## Deskripsi
This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. To construct such an attack, you need to combine some of the techniques you learned in previous labs.

The database contains a different table called users, with columns called username and password.

To solve the lab, perform a SQL injection UNION attack that retrieves all usernames and passwords, and use the information to log in as the administrator user.

## Resource

Link soal: https://portswigger.net/web-security/sql-injection/union-attacks/lab-retrieve-data-from-other-tables

Link resource: https://portswigger.net/web-security/sql-injection/union-attacks

## Langkah - langkah
1. Masuk ke halaman lab terlebih dahulu menggunakan browser bawaan Burp Suite.
   <img width="1919" height="1134" alt="image" src="https://github.com/user-attachments/assets/ce3af965-f410-42cf-b86c-73a062807f4e" />

2. Setelah itu, aktifkan intercept di Burp Suite, kemudian klik salah satu kategori barang.
   <img width="1919" height="1134" alt="image" src="https://github.com/user-attachments/assets/6b38a027-b983-4a5f-8a24-6e02643ba04e" />

3. Karena payload nanti akan menggunakan dua kolom (`username` dan `password`), maka cek terlebih dahulu apakah query SQL yang ada menggunakan 2 kolom dengan menggunakan payload `'+UNION+SELECT+'a','a'--`
   <img width="1919" height="1137" alt="image" src="https://github.com/user-attachments/assets/670a7605-6fbe-4fb1-b82d-f9ecf4ec6ed0" />
   Hasil:
   <img width="1919" height="1139" alt="image" src="https://github.com/user-attachments/assets/0de42734-c55b-4e4a-b26b-5e1b6df863bb" />

4. Setelah verifikasi kolom query SQL, dapatkan kolom `username` dan `password` dengan payload `'+UNION+SELECT+username,password+FROM+users--`
   <img width="1919" height="1130" alt="image" src="https://github.com/user-attachments/assets/2fd70c22-e0d0-407a-910d-81ced92ce499" />
   Hasil:
   <img width="1919" height="1130" alt="image" src="https://github.com/user-attachments/assets/ba9c93b5-5d98-4a38-9c8d-1c599a3a8997" />
   Password dari user `administrator` adalah `6znnay3b18l2xokl1rfa`.

5. Login sebagai user `administrator` dengan credential yang didapatkan. 
   <img width="1919" height="1136" alt="image" src="https://github.com/user-attachments/assets/32b15c28-2dde-4283-ab08-0c7fcd3e4673" />

## Catatan
Di soal ini, karena payload sudah pasti menggunakan dua kolom `username` dan `password` dan UNION membutuhkan tabel yang digabungkan untuk mempunyai jumlah kolom yang sama, ada baiknya untuk melakukan verifikasi terlebih dahulu. Hal ini karena bisa saja query SQL menggunakan 3 kolom, yang artinya untuk payload yang digunakan akan menggunakan kolom `username`, `password`, dan `NULL`. Karena di sini hanya ada dua kolom saja, maka tidak perlu `NULL` sebagai kolom tambahan.
