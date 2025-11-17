Nama    : Navyta Budi Yulia

NIM     : 312410184

Kelas   : TI.24.A2

# Lab8Web_

## Langkah 1 
  - Membuat Database `latihan1` : Menyediakan ruang penyimpanan untuk aplikasi CRUD. 
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

<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/c7198311-6c80-4b9a-9da8-786eab36c506" />


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

<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/4843122b-de74-4d96-9441-eb382100875f" />

<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/4dbaa24c-d998-4732-9e01-32ca2cc99636" />

## Langkah 4
  - Membuat halaman index : menampilkan tabel barang dan menjadi halaman utama CRUD.
    
  Perubahan :
  - menampilkan seluruh data dari tabel
  - menambahkan tombol tambah barang
  - mengatur ulang kolom aksi : ubah | hapus

```
<?php
include("koneksi.php");
$sql = 'SELECT * FROM data_barang';
$result = mysqli_query($conn, $sql);
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Data Barang</title>
</head>
<body>
<div class="container">
    <h1>Data Barang</h1>

    <a href="tambah.php">Tambah Barang</a>
    <br><br>

    <div class="main">
        <table>
            <tr>
                <th>Gambar</th>
                <th>Nama Barang</th>
                <th>Katagori</th>
                <th>Harga Jual</th>
                <th>Harga Beli</th>
                <th>Stok</th>
                <th>Aksi</th>
            </tr>

            <?php if($result): ?>
            <?php while($row = mysqli_fetch_array($result)): ?>
            <tr>
                <td><img src="<?= $row['gambar']; ?>" width="80"></td>
                <td><?= $row['nama'];?></td>
                <td><?= $row['kategori'];?></td>
                <td><?= $row['harga_jual'];?></td>
                <td><?= $row['harga_beli'];?></td>
                <td><?= $row['stok'];?></td>
                <td>
                    <a href="ubah.php?id=<?= $row['id_barang']; ?>">Ubah</a> |
                    <a href="hapus.php?id=<?= $row['id_barang']; ?>">Hapus</a>
                </td>
            </tr>
            <?php endwhile; else: ?>
            <tr>
                <td colspan="7">Belum ada data</td>
            </tr>
            <?php endif; ?>
        </table>
    </div>
</div>
</body>
</html>
```

<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/4553dbca-8de9-4e84-8cbf-3fdacd799fe0" />

## Langkah 5
  - Membuat halaman tambah data
  - file : tambah.php

  Perubahan :
  - Menambah data baru
  - Menghilangkan echo di koneksi.php
  - Memperbaiki proses upload gambar
  - Cek error update file
  - Menambahkan `exit()` setelah redirect

```
<?php
error_reporting(E_ALL);
ini_set('display_errors', 1);

include_once 'koneksi.php';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {

    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];

    $gambar = "";

    // pastikan folder gambar ada
    $folder = __DIR__ . "/gambar/";
    if (!is_dir($folder)) {
        mkdir($folder, 0777, true);
    }

    if (!empty($_FILES['file_gambar']['name'])) {
        $filename = str_replace(' ', '_', $_FILES['file_gambar']['name']);
        $destination = $folder . $filename;

        if (move_uploaded_file($_FILES['file_gambar']['tmp_name'], $destination)) {
            $gambar = $filename;  // hanya nama file
        }
    }

    $sql = "INSERT INTO data_barang 
            (nama, kategori, harga_jual, harga_beli, stok, gambar)
            VALUES 
            ('$nama', '$kategori', '$harga_jual', '$harga_beli', '$stok', '$gambar')";

    mysqli_query($conn, $sql);

    header("Location: index.php");
    exit;
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<link href="style.css" rel="stylesheet" type="text/css" />
<title>Tambah Barang</title>
</head>
<body>
<div class="container">
<h1>Tambah Barang</h1>
<div class="main">

<form action="tambah.php" method="post" enctype="multipart/form-data">

    <div class="input">
        <label>Nama Barang</label>
        <input type="text" name="nama" required>
    </div>

    <div class="input">
        <label>Kategori</label>
        <select name="kategori">
            <option value="Komputer">Komputer</option>
            <option value="Elektronik">Elektronik</option>
            <option value="Hand Phone">Hand Phone</option>
        </select>
    </div>

    <div class="input">
        <label>Harga Jual</label>
        <input type="number" name="harga_jual" required>
    </div>

    <div class="input">
        <label>Harga Beli</label>
        <input type="number" name="harga_beli" required>
    </div>

    <div class="input">
        <label>Stok</label>
        <input type="number" name="stok" required>
    </div>

    <div class="input">
        <label>File Gambar</label>
        <input type="file" name="file_gambar">
    </div>

    <div class="submit">
        <input type="submit" value="Simpan">
    </div>

</form>

</div>
</div>
</body>
</html>
```

<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/7bb497fe-e498-451e-b11b-5b6f3c3b2e74" />

## Langkah 6
  - Membuat halaman ubah data : Mengubah isi data barang
  - file : ubah.php

  Perubahan :
  - Mengambil data berdasarkan ID
  - Menampilkan data lama di input
  - Memperbaiki pilihan kategori (selected)
  - Memperbaiki update gambar (opsional)
  - Redirect setelah update

```
<?php
error_reporting(E_ALL);
include_once 'koneksi.php';

if (isset($_POST['submit']))
{
    $id = $_POST['id'];
    $nama = $_POST['nama'];
    $kategori = $_POST['kategori'];
    $harga_jual = $_POST['harga_jual'];
    $harga_beli = $_POST['harga_beli'];
    $stok = $_POST['stok'];
    $file_gambar = $_FILES['file_gambar'];

    $gambar = null;

    if ($file_gambar['error'] == 0)
    {
        $filename = str_replace(' ', '_', $file_gambar['name']);
        $destination = dirname(__FILE__) . '/gambar/' . $filename;

        if (move_uploaded_file($file_gambar['tmp_name'], $destination))
        {
            $gambar = $filename;
        }
    }

    $sql = "UPDATE data_barang SET 
            nama = '{$nama}',
            kategori = '{$kategori}',
            harga_jual = '{$harga_jual}',
            harga_beli = '{$harga_beli}',
            stok = '{$stok}'";

    if (!empty($gambar)) {
        $sql .= ", gambar = '{$gambar}' ";
    }

    $sql .= " WHERE id_barang = '{$id}'";

    mysqli_query($conn, $sql);

    header('location: index.php');
}

$id = $_GET['id'];
$sql = "SELECT * FROM data_barang WHERE id_barang = '{$id}'";
$result = mysqli_query($conn, $sql);

if (!$result) die("Error: Data tidak ditemukan");
$data = mysqli_fetch_array($result);
function is_select($var, $val) {
    return $var == $val ? 'selected="selected"' : '';
}
?>
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <link href="style.css" rel="stylesheet" type="text/css" />
    <title>Ubah Barang</title>
</head>
<body>
<div class="container">
    <h1>Ubah Barang</h1>
    <div class="main">

        <form method="post" action="ubah.php" enctype="multipart/form-data">

            <div class="input">
                <label>Nama Barang</label>
                <input type="text" name="nama" value="<?= $data['nama']; ?>" />
            </div>

            <div class="input">
                <label>Kategori</label>
                <select name="kategori">
                    <option value="Komputer"   <?= is_select('Komputer', $data['kategori']); ?>>Komputer</option>
                    <option value="Elektronik" <?= is_select('Elektronik', $data['kategori']); ?>>Elektronik</option>
                    <option value="Hand Phone" <?= is_select('Hand Phone', $data['kategori']); ?>>Hand Phone</option>
                </select>
            </div>

            <div class="input">
                <label>Harga Jual</label>
                <input type="text" name="harga_jual" value="<?= $data['harga_jual']; ?>" />
            </div>

            <div class="input">
                <label>Harga Beli</label>
                <input type="text" name="harga_beli" value="<?= $data['harga_beli']; ?>" />
            </div>

            <div class="input">
                <label>Stok</label>
                <input type="text" name="stok" value="<?= $data['stok']; ?>" />
            </div>

            <div class="input">
                <label>File Gambar</label>
                <input type="file" name="file_gambar" />
            </div>

            <div class="submit">
                <input type="hidden" name="id" value="<?= $data['id_barang']; ?>" />
                <input type="submit" name="submit" value="Simpan" />
            </div>

        </form>

    </div>
</div>
</body>
</html>
```
<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/1a185f4e-1e92-49dc-a2db-39930bdfa55c" />

<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/cdc9f9b2-4ef9-4e44-98e6-a7b8f8ad7217" />

## Langkah 7
  - Membuat halaman hapus data
  - file : hapus.php

```
<?php
include_once 'koneksi.php';

$id = $_GET['id'];

$sql = "DELETE FROM data_barang WHERE id_barang = '{$id}'";
mysqli_query($conn, $sql);

header('location: index.php');
?>
```

<img width="1920" height="1008" alt="Image" src="https://github.com/user-attachments/assets/a8635ede-1a22-4ff4-8780-930cd139cc71" />
