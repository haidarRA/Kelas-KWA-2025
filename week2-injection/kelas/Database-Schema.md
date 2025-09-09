# Login Bender

## Deskripsi
Exfiltrate the entire DB schema definition via SQL Injection.

## Resource
https://portswigger.net/web-security/sql-injection/examining-the-database
https://portswigger.net/web-security/sql-injection/union-attacks

## Langkah - langkah
1. Cari endpoint yang kemungkinan vulnerable selain di page login.
   <img width="1919" height="1136" alt="image" src="https://github.com/user-attachments/assets/221971e4-9967-4d93-947b-5cc6b9879201" />
   Dari sekian banyak request yang diintercept, ini yang paling meyakinkan karena ada parameter `q` yang digunakan sebagai query SQL untuk filter product.

2. Kirimkan payload ke repeater agar request dapat dimodifikasi dan dikirim secara berulang - ulang.
   <img width="1919" height="1141" alt="image" src="https://github.com/user-attachments/assets/9a40d574-3c96-4b5c-ae66-0fd5a854c1bd" />

3. Setelah itu, coba modifikasi request.
   <img width="1919" height="1136" alt="image" src="https://github.com/user-attachments/assets/a8c0a61b-af78-4417-b3bd-cac1e324b353" />

4. Setelah mengetahui struktur query, karena nanti akan menggunakan `UNION` yang mana jumlah kolom dari kedua tabel yang digabung harus sama, hitung jumlah kolom terlebih dahulu.
   Gunakan payload `orange'))+UNION+SELECT+'a'--` untuk cek jumlah kolom.
   <img width="1919" height="1133" alt="image" src="https://github.com/user-attachments/assets/d8d974a2-248c-4aca-902a-7786bf791d2f" />
   Karena masih ada error, tambahkan bagian `'a'` pada payload hingga tidak ada error lagi.
   <img width="1919" height="1134" alt="image" src="https://github.com/user-attachments/assets/ee6dd2dd-f909-44d1-840e-411f24082cda" />
   Dengan menggunakan payload `orange'))+UNION+SELECT+'a','a','a','a','a','a','a','a','a'--`, tidak ada pesan error. Hal ini menunjukkan bahwa jumlah kolomnya adalah 9.

5. Masukkan payload `orange'))+UNION+SELECT+'a','a','a','a','a','a','a','a',sql+FROM+sqlite_schema--` untuk mendapatkan database schema.
   <img width="1919" height="1140" alt="image" src="https://github.com/user-attachments/assets/59ac8d25-3050-4922-a6a8-ff5aee2ba325" />

## Catatan
Karena struktur query SQL tidak diketahui, maka cari jumlah kolom yang sesuai untuk UNION.

Setelah mendapatkan jumlah kolom, dapatkan seluruh schema database dari tabel sqlite_schema. Hal ini menunjukkan bahwa database yang digunakan adalah SQLite.
