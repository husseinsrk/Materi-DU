## PHP : Input Data Ke Database

### 1 . Membuat Page pendaftar.php

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

method, pada bagian ‘method‘ yang di huruf tebal di atas boleh kalian isi ‘post‘ atau ‘get‘, perbedaan POST dan GET ini kurang lebih penjelasannya sebagai berikut.
Kalau kalian perhatikan pada form di atas terdapat 3 value, yaitu name="", data input dari ketiga value ini yang akan di proses di file send_pendaftar.php nanti, kalau kalian menggunakan GET, setelah di submit maka nilai dari value yang kalian masukan akan tampil di URL browser kalian.

    http://localhost/satu.php?name=otong&email=otong@gmail.com&telp=082373843

Sedangkan kalau kalian menggunakan POST pada method-nya, maka nilai dari value yang kalian masukan tidak akan di tampilkan.

    http://localhost/send_pendaftar.php

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



### 6. Membuat Database di phpmyadmin

```sql

--
-- Table structure for table `massage`
--

CREATE TABLE `massage` (
  `id_user` int(10) UNSIGNED NOT NULL,
  `nama` varchar(200) NOT NULL,
  `email` varchar(200) NOT NULL,
  `massage` text NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=latin1;

--
-- Dumping data for table `massage`
--

INSERT INTO `massage` (`id_user`, `nama`, `email`, `massage`) VALUES
(1, 'ONPHPID', 'admin', '21232f297a57a5a743894a0e4a801fc3'),
(2, 'Regha', 'member', 'aa08769cdcb26674c6706093503ff0a3'),
(3, 'admin', 'admin', '0192023a7bbd73250516f069df18b500'),
(4, 'aku', 'akukamu', 'a42d640365f9ba18208ab059cd068e09');

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
(1, 'ONPHPID', 'admin', '21232f297a57a5a743894a0e4a801fc3'),
(2, 'Regha', 'member', 'aa08769cdcb26674c6706093503ff0a3'),
(3, 'admin', 'admin', '0192023a7bbd73250516f069df18b500'),
(4, 'aku', 'akukamu', 'a42d640365f9ba18208ab059cd068e09'),
(5, 'dijah', 'dijah', 'fa13f6c9173b03b1709d9352f186515b'),
(6, 'sdadjsh', 'sadhsjdhj', '123'),
(7, 'asdjasdjjh', 'ashdjash', 'asdjasd'),
(8, 'aku', 'ashdjash', 'asdjasd'),
(9, 'aku', '', 'asdjasd'),
(10, 'rizki', '', 'asdas'),
(11, 'jojo', 'jojo2gmail,com', 'ini pesan kiu'),
(12, 'aku', 'kamu@gmail.com', '09192919');

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
-- Dumping data for table `users`
--

INSERT INTO `users` (`id_user`, `nama`, `username`, `password`, `level_user`) VALUES
(11, 'admin', 'admin', '21232f297a57a5a743894a0e4a801fc3', 'member');

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
  MODIFY `id_user` int(10) UNSIGNED NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=5;
--
-- AUTO_INCREMENT for table `pendaftar`
--
ALTER TABLE `pendaftar`
  MODIFY `id_user` int(10) UNSIGNED NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=13;
--
-- AUTO_INCREMENT for table `users`
--
ALTER TABLE `users`
  MODIFY `id_user` int(10) UNSIGNED NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=12;
/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;

```
