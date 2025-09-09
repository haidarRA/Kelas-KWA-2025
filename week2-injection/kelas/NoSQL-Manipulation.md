# NoSQL Manipulation

## Deskripsi
Update multiple product reviews at the same time.

## Resource
https://portswigger.net/web-security/nosql-injection

## Langkah - langkah
1. Buka dan tulis review di salah satu product.
   <img width="1919" height="1137" alt="image" src="https://github.com/user-attachments/assets/e90f339c-88c3-4b2c-ac22-ac9d31a05edc" />

2. Setelah menulis review, edit salah satu review yang dibuat.
   <img width="1919" height="1138" alt="image" src="https://github.com/user-attachments/assets/e6c0286e-33ca-4c65-91f2-f51960f95c44" />
   
3. Sebelum submit hasil edit review, aktifkan intercept di Burp Suite terlebih dahulu.
   <img width="1919" height="1137" alt="image" src="https://github.com/user-attachments/assets/cac77d2b-75fc-449b-a1fa-9b466eb4fc28" />
   Ada sebuah request edit review dengan metode `PATCH`.

4. Ubah isi parameter `id` menjadi `{"$ne": null}`.
   <img width="1919" height="1132" alt="image" src="https://github.com/user-attachments/assets/25b6a72f-6f28-4363-8b88-d921da7ba2d6" />
   Hasil:
   <img width="1919" height="1134" alt="image" src="https://github.com/user-attachments/assets/2a7b6db6-b746-4b9c-a171-ec2c4fed37a1" />
   <img width="1919" height="1137" alt="image" src="https://github.com/user-attachments/assets/68552b96-03fc-41fc-96f8-ea8fae15644d" />
   Semua review langsung berubah.

## Catatan
Adanya payload `{"$ne": null}` pada payload NoSQL secara efektif mengabaikan fungsi yang seharusnya memperbarui review tertentu saja, sehingga mengakibatkan pembaruan massal untuk semua review dalam database.
