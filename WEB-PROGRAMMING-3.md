## PHP : Input Data Ke Database

### 1 . Membuat Page pendaftar.php


Coding Pertemuan 2 bisa kalian download di https://github.com/rizkimufti/Materi-DU/tree/master/assets/Coding-pt2


```php

<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Halaman Registrasi</title>

    <link href="css/bootstrap.min.css" rel="stylesheet">
    <link href="css/style.css" rel="stylesheet">

    <link href="cssblog/bootstrap.min.css" rel="stylesheet">
    

  </head>
  <body>
    <div class="col-md-4 col-md-offset-4 form-login">
    
        <?php
        /* handle error */
        if (isset($_GET['error'])) : ?>
            <div class="alert alert-warning alert-dismissible" role="alert">
            <button type="button" class="close" data-dismiss="alert" aria-label="Close">
                <span aria-hidden="true">&times;</span>
            </button>
            <strong>Warning!</strong> <?=base64_decode($_GET['error']);?>
            </div>
        <?php endif;?>

        <div class="outter-form-login">
           
            <form action="send_pendaftar.php" class="inner-login" method="post">
            <h3 class="text-center title-login">Pesan</h3>
                <div class="form-group">
                    <input type="text" class="form-control" name="nickname" placeholder="Nama">
                </div>

                <div class="form-group">
                    <input type="email" class="form-control" name="email" placeholder="email">
                </div>


                <div class="form-group">
                    <input type="text" class="form-control" name="nohp" placeholder="No Handphone">
                </div>


                <input type="submit" class="" value="Daftar" />
                
               
            </form>
        </div>
    </div>

    <script src="assets/js/jquery.min.js"></script>
    <script src="assets/js/bootstrap.min.js"></script>
  </body>
</html>


```

 method, pada bagian ‘method‘ yang di huruf tebal di atas boleh kalian isi ‘post‘ atau ‘get‘, perbedaan POST dan GET ini kurang lebih penjelasannya sebagai berikut.Kalau kalian perhatikan pada form di atas terdapat 3 value, yaitu name="", data input dari ketiga value ini yang akan di proses di file send_pendaftar.php nanti, kalau kalian menggunakan GET, setelah di submit maka nilai dari value yang kalian masukan akan tampil di URL browser kalian 


    http://localhost/satu.php?name=otong&email=otong@gmail.com&telp=082373843

Sedangkan kalau kalian menggunakan POST pada method-nya, maka nilai dari value yang kalian masukan tidak akan di tampilkan.

    http://localhost/send_pendaftar.php

 action, action pada form di atas bisa di istilahkan sebagai navigator, pada saat tombol Submit di klik nanti si tombol ini akan ‘nanya‘ ke action, data-data yang sudah kalian input ini mau di proses dimana, lalu si action bilang, di "send_pendaftar.php" aja oleh karena itu untuk action kita isi nama file "send_pendaftar.php" agar data input-nya bisa di proses di file tersebut.


 type, type atau tipe input dalam istilah yang lebih mudahnya yaitu jenis kolom, kalau kolomnya untuk input nama berarti tipe input-nya ‘text‘, kalau buat email berarti tipe inputnya ‘email‘, kalau buat nomor berarti tipe input-nya ‘number‘, begitu seterusnya, attributnya sendiri bermacam-macam, jelasnya kalian bisa lihat di link berikut.

 https://www.w3schools.com/tags/att_input_type.asp

 name, name akan berfungsi sebagai nama dari kolom input itu sendiri, name perlu di isi karena nanti berfungsi sebagai ‘inisial‘ di dalam file proses php

### 2. Membuat send_pendaftar.php

```php
<?php
if (isset($_POST['nickname']) && $_POST['nickname']) {
    // memasukan file koneksi ke database
    require_once 'config.php';
    // menyimpan variable yang dikirim Form
    $nama = $_POST['nickname'];
    $email = $_POST['email'];
    $nohp = $_POST['nohp'];
    // $repassword = $_POST['repassword'];
    // cek nilai variable
    if (empty($nama)) {
        header('location: ./pendaftar.php?error=' .base64_encode('Nama tidak boleh kosong'));
    }
    if (empty($email)) {
        header('location: ./pendaftar.php?error=' .base64_encode('Email tidak boleh kosong'));   
    }
    if (empty($nohp)) {
        header('location: ./pendaftar.php?error=' .base64_encode('Password tidak boleh kosong'));   
    }

    $sql = "INSERT INTO pendaftar (nama, email, nohp) VALUES ('$nama', '$email', '$nohp' )";
    $insert = $dbconnect->query($sql);
    // jika gagal
    if (! $insert) {    
        echo "<script>alert('".$dbconnect->error."'); window.location.href = './pendaftar.php';</script>";
    } else {
        echo "<script>alert('Insert Data Berhasil'); window.location.href = './index.php';</script>";
    }
}
?>

```

isset digunakan untuk menyatakan variabel sudah diset atau tidak. Jika variabel sudah diset makan variabel akan mengembalikan nilai true, sebaliknya akan bernilai false maka dia akan berjalan ke proses false yang kita buat

require_once digunakan ketika file tidak ditemukan maka script akan berhenti

Insert into digunakan untuk memasukan data yang kita buat kedalam tabel database

echo, perintah PHP yang akan menampilkan deskripsi yang kita masukan. Pada skrip di atas, perintah ECHO akan menampilkan ‘Input data berhasil‘ jika nilai berhasil di input jika salah dia akan menampilkan kalimat error yang kita buat sebelumnya die config.php

### 3. Membuat config.php

```php

<?php

define('DBHOST', 'localhost');
define('DBUSER', 'root');
define('DBPASS', 'admin');
define('DBNAME', 'doscvdev');

/**
 * $dbconnect : koneksi kedatabase
 */
$dbconnect = new mysqli(DBHOST, DBUSER, DBPASS, DBNAME);

/**
 * Check Error yang terjadi saat koneksi
 * jika terdapat error maka die() // stop dan tampilkan error
 */
if ($dbconnect->connect_error) {
	die('Database Not Connect. Error : ' . $dbconnect->connect_error);
}

```

Skrip di atas merupakan skrip untuk koneksi ke database-nya, sebenarnya ada banyak skrip yang bisa kita gunakan untuk mengkoneksikan PHP ke database, salah satunya skrip di atas.

$dbconnect, berfungsi untuk memberikan akses ke dalam database dengan memberikan autentikasi yang berupa nama host, user dan pass sebagai data login-nya.

or die;, akan menampilkan pesan error jika terjadi kesalahan pada saat proses autentikasi.

### 4. Membuat form pesan di index.php

```php

<?php
        /* handle error */
        if (isset($_GET['error'])) : ?>
            <div class="alert alert-warning alert-dismissible" role="alert">
            <button type="button" class="close" data-dismiss="alert" aria-label="Close">
                <span aria-hidden="true">&times;</span>
            </button>
            <strong>Warning!</strong> <?=base64_decode($_GET['error']);?>
            </div>
        <?php endif;?>

    <form action="send_pesan.php" class="inner-login" method="post">

    <div class="col-sm-7 slideanim">
      <div class="row">
        <div class="col-sm-6 form-group">
        <input type="text" class="form-control" name="nickname" placeholder="Nama">
        </div>
        <div class="col-sm-6 form-group">
        <input type="email" class="form-control" name="email" placeholder="email">
        </div>
      </div>
      
      <input type="text" class="form-control" name="pesan" placeholder="Pesan"><br>

      <div class="row">
        <div class="col-sm-12 form-group">

        <input type="submit" class="col-sm-6" value="Kirim" >
        </div>
      </div>
    </div>
    </form>
  </div>
</div>


```

### 5. Membuat send_pesan.php

```php
<?php
if (isset($_POST['nickname']) && $_POST['nickname']) {
    // memasukan file koneksi ke database
    require_once 'config.php';
    // menyimpan variable yang dikirim Form
    $nama = $_POST['nickname'];
    $email = $_POST['email'];
    $pesan = $_POST['pesan'];
    // $repassword = $_POST['repassword'];
    // cek nilai variable
    if (empty($nama)) {
        header('location: ./index.php?error=' .base64_encode('Nama tidak boleh kosong'));
    }
    if (empty($email)) {
        header('location: ./index.php?error=' .base64_encode('Email tidak boleh kosong'));   
    }
    if (empty($pesan)) {
        header('location: ./index.php?error=' .base64_encode('Pesan tidak boleh kosong'));   
    }


    $sql = "INSERT INTO pesan (nama, email, pesan) VALUES ('$nama', '$email', '$pesan' )";
    $insert = $dbconnect->query($sql);
    // jika gagal
    if (! $insert) {    
        echo "<script>alert('".$dbconnect->error."'); window.location.href = './index.php';</script>";
    } else {
        echo "<script>alert('Insert Data Berhasil'); window.location.href = './index.php';</script>";
    }
}
?>


```

isset digunakan untuk menyatakan variabel sudah diset atau tidak. Jika variabel sudah diset makan variabel akan mengembalikan nilai true, sebaliknya akan bernilai false maka dia akan berjalan ke proses false yang kita buat

require_once digunakan ketika file tidak ditemukan maka script akan berhenti

Insert into digunakan untuk memasukan data yang kita buat kedalam tabel database

echo, perintah PHP yang akan menampilkan deskripsi yang kita masukan. Pada skrip di atas, perintah ECHO akan menampilkan ‘Input data berhasil‘ jika nilai berhasil di input jika salah dia akan menampilkan kalimat error yang kita buat sebelumnya die config.php


### 6. Membuat Database di phpmyadmin

```sql

-- phpMyAdmin SQL Dump
-- version 4.5.4.1deb2ubuntu2
-- http://www.phpmyadmin.net
--
-- Host: localhost
-- Generation Time: Apr 01, 2018 at 04:44 PM
-- Server version: 5.7.20-0ubuntu0.16.04.1
-- PHP Version: 7.0.22-0ubuntu0.16.04.1

SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
SET time_zone = "+00:00";


/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8mb4 */;

--
-- Database: `doscvdev`
--



-- --------------------------------------------------------

--
-- Table structure for table `pendaftar`
--

CREATE TABLE `pendaftar` (
  `id_user` int(10) UNSIGNED NOT NULL,
  `nama` varchar(200) NOT NULL,
  `email` varchar(200) NOT NULL,
  `nohp` text NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

--
-- Dumping data for table `pendaftar`
--

INSERT INTO `pendaftar` (`id_user`, `nama`, `email`, `nohp`) VALUES
(13, 'rizki', 'rizkimufti@gmail.com', '019209');

-- --------------------------------------------------------

--
-- Table structure for table `pesan`
--

CREATE TABLE `pesan` (
  `id_user` int(10) UNSIGNED NOT NULL,
  `nama` varchar(200) NOT NULL,
  `email` varchar(200) NOT NULL,
  `pesan` text NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

--
-- Dumping data for table `pesan`
--

INSERT INTO `pesan` (`id_user`, `nama`, `email`, `pesan`) VALUES
(18, 'Rizki ', 'rizkimufti@gmail.com', 'Mantap');

-- --------------------------------------------------------

--
-- Table structure for table `users`
--

CREATE TABLE `users` (
  `id_user` int(10) UNSIGNED NOT NULL,
  `nama` varchar(200) NOT NULL,
  `username` varchar(200) NOT NULL,
  `password` text NOT NULL,
  `level_user` varchar(150) NOT NULL DEFAULT 'member'
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

--
-- Indexes for dumped tables
--

--
-- Indexes for table `massage`
--
ALTER TABLE `massage`
  ADD PRIMARY KEY (`id_user`);

--
-- Indexes for table `pendaftar`
--
ALTER TABLE `pendaftar`
  ADD PRIMARY KEY (`id_user`);

--
-- Indexes for table `pesan`
--
ALTER TABLE `pesan`
  ADD PRIMARY KEY (`id_user`);

--
-- Indexes for table `users`
--
ALTER TABLE `users`
  ADD PRIMARY KEY (`id_user`);

--
-- AUTO_INCREMENT for dumped tables
--

--
-- AUTO_INCREMENT for table `massage`
--
ALTER TABLE `massage`
  MODIFY `id_user` int(10) UNSIGNED NOT NULL AUTO_INCREMENT;
--
-- AUTO_INCREMENT for table `pendaftar`
--
ALTER TABLE `pendaftar`
  MODIFY `id_user` int(10) UNSIGNED NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=20;
--
-- AUTO_INCREMENT for table `pesan`
--
ALTER TABLE `pesan`
  MODIFY `id_user` int(10) UNSIGNED NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=19;
--
-- AUTO_INCREMENT for table `users`
--
ALTER TABLE `users`
  MODIFY `id_user` int(10) UNSIGNED NOT NULL AUTO_INCREMENT;
/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;
```

Pada kode diatas kamu dapat membuat sebuah tabel dengan perintah CREATE TABLE. Kemudian didalam sql ada tipe data berupa INT, VARCHAR, TEXT, DATETIME dan TIMESTAMP. Untuk tipe data VARCHAR kamu harus menentukan berapa panjang maksimal dari kolom tersebut. Tipe INT dapat kamu tentukan panjang angka yang akan digunakan. Sedangkan TIMESTAMP akan selalu diisi secara otomatis oleh MySQL saat baris baru dibuat.

Kemudian ada juga atribut tambahan NOT NULL dimana kolom tersebut tidak boleh kosong saat proses insert. Kemudian ada penentuan PRIMARY KEY dimana kolom tersebut akan menjadi pembeda antar kolom agar mencegah data dengan id sama memiliki dua baris yang sama.

Auto increment field pada sql adalah suatu tipe field integer yang secara otomatis akan bertambah nilainya jika terjadi penambahan row pada table dimana field tersebut berada.

Otomatis disini artinya pada saat kita memasukkan data baik melalui stement INSERT maupun melalui mekanisme data access lainnya, field tersebut tidak perlu dimasukkan nilainya atau cukup diberi nilai NULL, maka sql akan menentukan sendiri nilai apa yang akan diberikan sebagai akibat penambahan baris data tersebut.

ALTER TABLE adalah salah satu perintah di database MySQL untuk merubah struktur table baik itu merubah nama table, merubah nama kolom, merubah tipe data, dan lain-lain.


## Membuat Admin Section

- membuat register admin

```php

<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Halaman Registrasi</title>

    <link href="css/style.css" rel="stylesheet">
        <link href="css/bootstrap.min.css" rel="stylesheet">

<!-- Custom styles for this template -->
<link href="cssblog/blog-home.css" rel="stylesheet">

  </head>
  <body>


    <div class="col-md-4 col-md-offset-4 form-login">
    
    <?php
    /* handle error */
    if (isset($_GET['error'])) : ?>
        <div class="alert alert-warning alert-dismissible" role="alert">
          <button type="button" class="close" data-dismiss="alert" aria-label="Close">
            <span aria-hidden="true">&times;</span>
          </button>
          <strong>Warning!</strong> <?=base64_decode($_GET['error']);?>
        </div>
    <?php endif;?>

        <div class="outter-form-login">
           
            <form action="send_register.php" class="inner-login" method="post">
            <h3 class="text-center title-login">Registrasi</h3>
                <div class="form-group">
                    <input type="text" class="form-control" name="nickname" placeholder="Nama">
                </div>

                <div class="form-group">
                    <input type="text" class="form-control" name="username" placeholder="Username">
                </div>

                <div class="form-group">
                    <input type="password" class="form-control" name="password" placeholder="Password">
                </div>

                <div class="form-group">
                    <input type="password" class="form-control" name="repassword" placeholder="Re-Password">
                </div>

                <input type="submit" class="btn btn-block btn-custom-green" value="Daftar" />
                
                <div class="text-center forget">
                    <p>Back To <a href="./login.php">Login</a></p>
                </div>
            </form>
        </div>
    </div>

    <script src="assets/js/jquery.min.js"></script>
    <script src="assets/js/bootstrap.min.js"></script>
  </body>
</html>

```

- Membuat send_register

```php

<?php
if (isset($_POST['nickname']) && $_POST['nickname']) {
    // memasukan file koneksi ke database
    require_once 'config.php';
    // menyimpan variable yang dikirim Form
    $nama = $_POST['nickname'];
    $username = $_POST['username'];
    $password = $_POST['password'];
    $repassword = $_POST['repassword'];
    // cek nilai variable
    if (empty($nama)) {
        header('location: ./register.php?error=' .base64_encode('Nama tidak boleh kosong'));
    }
    if (empty($username)) {
        header('location: ./register.php?error=' .base64_encode('Username tidak boleh kosong'));   
    }
    if (empty($password)) {
        header('location: ./register.php?error=' .base64_encode('Password tidak boleh kosong'));   
    }
    // validasi apakah password dengan repassword sama
    if ($password != $repassword) { // jika tidak sama
        header('location: ./register.php?error=' .base64_encode('Password tidak boleh sama'));   
    }
    // encryption dengan md5
    $password = md5($password);
    $level = 'member'; // default, 
    // SQL Insert
    $sql = "INSERT INTO users (nama, username, password, level_user) VALUES ('$nama', '$username', '$password', '$level')";
    $insert = $dbconnect->query($sql);
    // jika gagal
    if (! $insert) {
        echo "<script>alert('".$dbconnect->error."'); window.location.href = './register.php';</script>";
    } else {
        echo "<script>alert('Insert Data Berhasil'); window.location.href = './login.php';</script>";
    }
}
?>

```

- Membuat Halaman Login

```php

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Halaman Login</title>

    <link href="css/style.css" rel="stylesheet">
    <link href="css/bootstrap.min.css" rel="stylesheet">



  </head>
  <body>

    <div class="col-md-4 col-md-offset-4 form-login">
    
    <?php
    /* handle error */
    if (isset($_GET['error'])) : ?>
        <div class="alert alert-warning alert-dismissible" role="alert">
          <button type="button" class="close" data-dismiss="alert" aria-label="Close">
            <span aria-hidden="true">&times;</span>
          </button>
          <strong>Warning!</strong> <?=base64_decode($_GET['error']);?>
        </div>
    <?php endif;?>

        <div class="outter-form-login">
      
            <form action="check-login.php" class="inner-login" method="post">
            <h3 class="text-center title-login">Login Member</h3>
                <div class="form-group">
                    <input type="text" class="form-control" name="username" placeholder="Username">
                </div>

                <div class="form-group">
                    <input type="password" class="form-control" name="password" placeholder="Password">
                </div>
                
                <input type="submit" class="btn btn-block btn-custom-green" value="Login" />
                
                <div class="text-center forget">
                    <p>Lupa Password ?</p>
                </div>
                <div class="text-center ">
                    <a href="register.php">Belum Daftar ?</a>
                </div>

            </form>
        </div>
    </div>

  </body>
</html>

```
- Membuat check login

```php
<?php
session_start();
require 'config.php';

if ( isset($_POST['username']) && isset($_POST['password']) ) {
    
    $sql_check = "SELECT nama, 
                         level_user, 
                         id_user 
                  FROM users 
                  WHERE 
                       username=? 
                       AND 
                       password=? 
                  LIMIT 1";

    $check_log = $dbconnect->prepare($sql_check);
    $check_log->bind_param('ss', $username, $password);

    $username = $_POST['username'];
    $password = md5( $_POST['password'] );

    $check_log->execute();

    $check_log->store_result();

    if ( $check_log->num_rows == 1 ) {
        $check_log->bind_result($nama, $level_user, $id_user);

        while ( $check_log->fetch() ) {
            $_SESSION['user_login'] = $level_user;
            $_SESSION['sess_id']    = $id_user;
            $_SESSION['nama']       = $nama;
            
        }

        $check_log->close();

        header('location:on-'.$level_user);
        exit();

    } else {
        header('location: login.php?error='.base64_encode('Username dan Password Invalid!!!'));
        exit();
    }

   
} else {
    header('location:login.php');
    exit();
}

```

- membuat Halaman logut

```php
<?php
session_start();
session_destroy();
header('location:login.php');
?>
```

