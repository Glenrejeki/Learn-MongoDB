# 📘 MongoDB Dasar

Dokumentasi ini berisi konsep dasar **MongoDB** dengan format **Definisi, Syntax, Kondisi, dan Contoh** untuk mempermudah belajar.

---

📚 **Sumber Belajar**: [YouTube MongoDB Tutorial](https://youtu.be/c2M-rlkkT5o?si=VKm76cxYl2I96G_E)

---

## 📑 Daftar Isi

1. [Database](#1-database)  
2. [Collection](#2-collection)  
3. [Insert Data](#3-insert-data)  
4. [Data Types](#4-data-types)  
5. [Find Data](#5-find-data)  
6. [Update Data](#6-update-data)  
7. [Delete Data](#7-delete-data)  
8. [Sorting](#8-sorting)  
9. [Limit](#9-limit)  
10. [Error Umum](#10-error-umum)  
11. [Comparison Operators](#11-comparison-operators)  
12. [Logical Operators](#12-logical-operators)  
13. [Indexes](#13-indexes)  
14. [Collections (Advanced)](#14-collections-advanced)  
15. [Catatan](#-catatan)  

---

## 1. Database

### Definisi
Database adalah wadah utama di MongoDB yang menyimpan koleksi (collection) dan dokumen.  
Satu server MongoDB bisa memiliki banyak database.

### Syntax
```javascript
use <nama_database>
show dbs
db
```

### Kondisi
* Digunakan saat membuat atau berpindah database.
* Jika database belum ada, akan otomatis dibuat setelah collection ditambahkan.

### Contoh
```javascript
use school
show dbs
db
```

---

## 2. Collection

### Definisi
Collection adalah kumpulan dokumen di dalam sebuah database, mirip seperti tabel pada SQL.

### Syntax
```javascript
db.createCollection("<nama_collection>")
show collections
```

### Kondisi
* Dipakai untuk membuat wadah data dalam database.
* Tidak perlu didefinisikan schema secara ketat (schema-less).

### Contoh
```javascript
db.createCollection("students")
show collections
```

---

## 3. Insert Data

### Definisi
Digunakan untuk menambahkan dokumen (data) baru ke dalam collection.

### Syntax
```javascript
db.<collection>.insertOne({ ... })
db.<collection>.insertMany([{ ... }, { ... }])
```

### Kondisi
* insertOne → untuk 1 data.
* insertMany → untuk banyak data sekaligus.

### Contoh
```javascript
db.students.insertOne({name:"Larry", age:32, gpa:2.8, fullTime:false})
db.students.insertMany([
  {name:"Anna", age:21, gpa:3.5, fullTime:true},
  {name:"Ben", age:23, gpa:3.2, fullTime:false}
])
```

---

## 4. Data Types

### Definisi
MongoDB mendukung berbagai tipe data, mirip JSON.

### Syntax
* String  
* Number (Int, Double, Long, Decimal128)  
* Boolean  
* Date  
* Array  
* Object  
* Null  

### Kondisi
Dipakai sesuai jenis data yang ingin disimpan.

### Contoh
```javascript
db.students.insertOne({
  name: "Clara",
  age: 20,
  gpa: 3.9,
  fullTime: true,
  registerDate: new Date(),
  graduation: null,
  courses: ["Biology", "Chemistry", "Calculus"],
  address: { street: "123 Fake St.", city: "Batam", zip: 12345 }
})
```

---

## 5. Find Data

### Definisi
Untuk membaca/mencari dokumen dalam collection.

### Syntax
```javascript
db.<collection>.find()
db.<collection>.findOne()
```

### Kondisi
* find → menampilkan semua data.
* findOne → hanya satu dokumen.

### Contoh
```javascript
db.students.find()
db.students.findOne({name:"Anna"})
```

---

## 6. Update Data

### Definisi
Digunakan untuk mengubah data dalam collection.

### Syntax
```javascript
db.<collection>.updateOne({filter}, {$set:{field:value}})
db.<collection>.updateMany({filter}, {$set:{field:value}})
```

### Kondisi
* updateOne → hanya update 1 dokumen.
* updateMany → update lebih dari satu dokumen.

### Contoh
```javascript
db.students.updateOne({name:"Larry"}, {$set:{gpa:3.0}})
db.students.updateMany({fullTime:false}, {$set:{graduation:"TBD"}})
```

---

## 7. Delete Data

### Definisi
Menghapus dokumen dari collection.

### Syntax
```javascript
db.<collection>.deleteOne({filter})
db.<collection>.deleteMany({filter})
```

### Kondisi
* deleteOne → hapus satu dokumen.
* deleteMany → hapus semua dokumen yang sesuai filter.

### Contoh
```javascript
db.students.deleteOne({name:"Ben"})
db.students.deleteMany({fullTime:false})
```

---

## 8. Sorting

### Definisi
Mengurutkan hasil pencarian (find).

### Syntax
```javascript
db.<collection>.find().sort({field:1})   // ascending
db.<collection>.find().sort({field:-1})  // descending
```

### Kondisi
* 1 → ascending.
* -1 → descending.

### Contoh
```javascript
db.students.find().sort({gpa:1})
db.students.find().sort({age:-1})
```

---

## 9. Limit

### Definisi
Membatasi jumlah data yang ditampilkan.

### Syntax
```javascript
db.<collection>.find().limit(n)
```

### Kondisi
Digunakan untuk menampilkan data terbatas agar lebih ringkas.

### Contoh
```javascript
db.students.find().limit(2)
```

---

## 10. Error Umum

### Definisi
Beberapa kesalahan umum saat menulis perintah di MongoDB.

### Syntax / Pesan Error
* `SyntaxError: Unexpected token` → biasanya ada salah koma atau tanda kurung.
* `MongoNetworkError: connect ECONNREFUSED` → MongoDB server tidak jalan.

### Kondisi
* Jika syntax salah, cek koma, tanda kurung, atau titik.
* Jika koneksi gagal, jalankan service MongoDB terlebih dahulu.

### Contoh
```bash
sudo systemctl start mongod
```

---

## 11. Comparison Operators

### Definisi
Operator perbandingan digunakan untuk membandingkan nilai field dalam query.

### Syntax
```javascript
{ field: { $ne: value } }   // tidak sama dengan
{ field: { $lt: value } }   // kurang dari
{ field: { $lte: value } }  // kurang atau sama dengan
{ field: { $gt: value } }   // lebih besar dari
{ field: { $gte: value } }  // lebih besar atau sama dengan
{ field: { $in: [val1, val2] } }   // ada dalam array
{ field: { $nin: [val1, val2] } }  // tidak ada dalam array
```

### Kondisi
* Digunakan saat ingin memfilter data dengan kondisi tertentu.
* `$in` → cocok untuk pencarian banyak nilai.
* `$nin` → kebalikan dari `$in`.

### Contoh
```javascript
db.students.find({name: {$ne: "Glen"}})
db.students.find({age: {$lt: 40}})
db.students.find({age: {$lte: 30}})
db.students.find({age: {$gt: 25}})
db.students.find({name: {$in: ["Glen", "Fluxy", "Kobra"]}})
db.students.find({name: {$nin: ["Glen", "Fluxy", "Kobra"]}})
```

---

## 12. Logical Operators

### Definisi
Operator logika digunakan untuk menggabungkan lebih dari satu kondisi.

### Syntax
```javascript
{ $and: [ {field1:cond1}, {field2:cond2} ] }
{ $or:  [ {field1:cond1}, {field2:cond2} ] }
{ $nor: [ {field1:cond1}, {field2:cond2} ] }
{ $not: { <expression> } }
```

### Kondisi
* `$and` → semua kondisi harus terpenuhi.
* `$or` → minimal satu kondisi terpenuhi.
* `$nor` → semua kondisi tidak boleh terpenuhi.
* `$not` → negasi dari kondisi tertentu.

### Contoh
```javascript
db.students.find({ $and: [ { fullTime: false }, { age: { $lte: 45 } } ] })
db.students.find({ $or: [ { age: 25 }, { gpa: 3.5 } ] })
db.students.find({ $nor: [ { age: { $lt: 30 } }, { fullTime: true } ] })
```

---

## 13. Indexes

### Definisi
Index digunakan untuk mempercepat pencarian data di collection.  
Tanpa index, MongoDB melakukan **COLLSCAN** (scan semua dokumen).

### Syntax
```javascript
db.<collection>.createIndex({ field: 1 })   // ascending
db.<collection>.createIndex({ field: -1 })  // descending
db.<collection>.getIndexes()
```

### Kondisi
* Index meningkatkan kecepatan pencarian.
* Setiap collection otomatis memiliki index pada `_id`.
* Bisa dibuat pada satu field atau kombinasi beberapa field.

### Contoh
```javascript
db.students.find({name:"Glen"}).explain("executionStats")
db.students.createIndex({name:1})
db.students.getIndexes()
```

---

## 14. Collections (Advanced)

### Definisi
Collection bisa dibuat dengan opsi tambahan seperti **capped** (ukuran terbatas) atau pengaturan index otomatis.

### Syntax
```javascript
db.createCollection("<nama>", {capped:true, size:1000000, max:100})
db.<collection>.drop()
```

### Kondisi
* **Capped Collection** → ukuran terbatas, cocok untuk log.
* Bisa dihapus dengan `.drop()` jika tidak dibutuhkan.

### Contoh
```javascript
db.createCollection("teachers", {capped:true, size:1000000, max:100})
db.createCollection("Course")
db.Course.drop()
show collections
```

---

## 📌 Catatan

* Gunakan **mongosh** untuk shell MongoDB modern.
* Perintah sensitif dengan koma dan tanda kurung.
* Selalu cek apakah database/collection sudah aktif sebelum insert.
