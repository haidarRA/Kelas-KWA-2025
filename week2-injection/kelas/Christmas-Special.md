# Christmas Special

## Deskripsi
Order the Christmas special offer of 2014.

## Resource
https://portswigger.net/web-security/sql-injection/union-attacks

## Langkah - langkah
1. Karena soal ini  menggunakan endpoint yang sama seperti soal `Database Schema`, buat payload yang akan digunakan berdasarkan table schema yang diperoleh, yaitu `aaaa'))+UNION+SELECT+*+FROM+Products--`.
   <img width="1919" height="1139" alt="image" src="https://github.com/user-attachments/assets/ec5f2fc8-34ed-4fa6-8ed6-30b38d2f62fe" />
   Dari sini, didapatkan ID dari product christmas special adalah `10`.

2. Coba tambahkan salah satu barang ke keranjang, kemudian intercept requestnya.
   <img width="1919" height="1133" alt="image" src="https://github.com/user-attachments/assets/e75e9b07-3504-4cbb-aba6-5710afd7814d" />
   Ubah parameter `ProductId` jadi `10`.
   <img width="1919" height="1134" alt="image" src="https://github.com/user-attachments/assets/1ef4889c-6530-4ee7-9153-50934c5bd5d2" />

3. Cek keranjang. Sekarang ada product christmas special.
   <img width="1919" height="1142" alt="image" src="https://github.com/user-attachments/assets/f5aa9117-1150-420d-8898-268c08122990" />
   
## Catatan
Karena table schema sudah didapatkan dari soal `Database Schema`, maka soal ini menggunakan payload yang menggunakan format yang sama.

Selain itu, karena query SQL yang digunakan hanya menampilkan product yang belum dihapus (deletedAt bernilai NULL), maka payload `aaaa'))+UNION+SELECT+*+FROM+Products--` menampilkan seluruh produk yang ada terlepas sudah dihapus atau tidak.
