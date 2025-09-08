# Lab: SQL injection UNION attack, determining the number of columns returned by the query

## Deskripsi
This lab contains a SQL injection vulnerability in the product category filter. The results from the query are returned in the application's response, so you can use a UNION attack to retrieve data from other tables. The first step of such an attack is to determine the number of columns that are being returned by the query. You will then use this technique in subsequent labs to construct the full attack.

To solve the lab, determine the number of columns returned by the query by performing a SQL injection UNION attack that returns an additional row containing null values.

## Resource
https://portswigger.net/web-security/sql-injection/union-attacks

## Langkah - langkah
1. Masuk ke halaman lab terlebih dahulu menggunakan browser bawaan Burp Suite.
   <img width="1919" height="1140" alt="image" src="https://github.com/user-attachments/assets/940a8f1e-f061-44d2-87f8-c0e5baa55510" />

2. Setelah itu, aktifkan intercept di Burp Suite, kemudian klik salah satu kategori barang.
   <img width="1919" height="1138" alt="image" src="https://github.com/user-attachments/assets/9d5aaa14-8fb4-48c9-ac9e-3ce61314ec10" />

3. Masukkan payload `'+UNION+SELECT+NULL--` pada parameter `category` di request.
   <img width="1919" height="1136" alt="image" src="https://github.com/user-attachments/assets/50288a66-22d7-49b6-9d83-b87e102ace62" />
   Hasil:
   <img width="1919" height="1136" alt="image" src="https://github.com/user-attachments/assets/be6f6f2f-db5c-4f02-a8de-af17cf6736a7" />

4. Karena ada internal server error, maka tambahkan NULL di payload hingga tidak ada error.
   * `'+UNION+SELECT+NULL,NULL--`
     <img width="1919" height="1138" alt="image" src="https://github.com/user-attachments/assets/d3a5f12c-230e-469c-981d-420a98da6854" />
     Hasil:
     <img width="1919" height="1134" alt="image" src="https://github.com/user-attachments/assets/740df38a-6ed3-4013-b5cf-32d497c16a10" />
   * `'+UNION+SELECT+NULL,NULL,NULL--`
     <img width="1919" height="1137" alt="image" src="https://github.com/user-attachments/assets/f9ee5fe3-be8f-4d0d-9f95-5198fda0b628" />
     Hasil:
     <img width="1919" height="1136" alt="image" src="https://github.com/user-attachments/assets/dccfec07-d544-4944-8158-ecef6537d604" />

## Catatan
Di soal ini, karena query SQL belum diketahui, maka bisa dicari jumlah kolom tabelnya dengan menggunakan UNION.

Karena UNION menggabungkan antar tabel yang mempunyai jumlah kolom yang sama, UNION bisa digunakan untuk mencari jumlah kolom.
