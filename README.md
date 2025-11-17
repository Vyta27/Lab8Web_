Nama    : Navyta Budi Yulia

NIM     : 312410184

Kelas   : TI.24.A2

# Lab8Web_

## Langkah 1 
  - Membuat Database `latihan1`
  - Membuat Tabel `data_barang` : Tabel ini untuk menyimpan seluruh informasi barang.

```
CREATE TABLE data_barang (
  id_barang int(10) auto_increment PRIMARY KEY,
  kategori varchar(30),
  nama varchar(30),
  gambar varchar(100),
  harga_beli decimal(10,0),
  harga_jual decimal(10,0),
  stok int(4)
);
```


<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/cfc16de2-6d55-4b5b-a3c2-bb7bf0f48a8e" />

<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/0a0664be-5e91-4a01-acbb-b9285afc2a27" />

## Langkah 2
  - Mengisi data awal : Menampilkan data awal di halaman index.

```
INSERT INTO data_barang (kategori, nama, gambar, harga_beli, harga_jual, stok) VALUES 
('Elektronik', 'HP Samsung Android', 'hp_samsung.jpg', 2000000, 2400000, 5),
('Elektronik', 'HP Xiaomi Android', 'hp_xiaomi.jpg', 1000000, 1400000, 5),
('Elektronik', 'HP OPPO Android', 'hp_oppo.jpg', 1800000, 2300000, 5);
```

<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/55ecf77c-343e-4bfa-822d-8612e7a6e12b" />

<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/ed44f3d0-18c0-488f-a841-9215134e2ac6" />


## Langkah 3 
    - Mmmbuat folder `lab8_php_database` di htdocs
    - Membuat file `koneksi.php` : menghubungkan PHP dengan databse.

```
<?php
error_reporting(E_ALL);
ini_set('display_errors', 1);

$host = "127.0.0.1:3307";
$user = "root";
$pass = "";
$db   = "latihan1";

$conn = mysqli_connect($host, $user, $pass, $db);

if (!$conn) {
echo "Koneksi ke server gagal.";
die();
} #else echo "Koneksi berhasil";
?>
```

<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/c7198311-6c80-4b9a-9da8-786eab36c506" />

<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/4843122b-de74-4d96-9441-eb382100875f" />

<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/4dbaa24c-d998-4732-9e01-32ca2cc99636" />

<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/48e75a00-08e2-4c3d-a6d9-756093189cfb" />

<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/4553dbca-8de9-4e84-8cbf-3fdacd799fe0" />

<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/1a185f4e-1e92-49dc-a2db-39930bdfa55c" />

<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/7bb497fe-e498-451e-b11b-5b6f3c3b2e74" />

<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/cdc9f9b2-4ef9-4e44-98e6-a7b8f8ad7217" />

<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/a8635ede-1a22-4ff4-8780-930cd139cc71" />
