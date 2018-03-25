## Materi Pertemuan 2

### Bootsrap

#### Apa Itu Bootsrap ???

Bootstrap adalah suatu CSS framework interface (kerangka kerja tampilan) website yang sangat populer dikalangan web designer untuk memperindah tampilan website mereka. Kalo kalian tanya : Manfaatnya apa sih ? Jawaban singkatnya : Bootstrap memiliki teknologi ultra responsive yang membuat tampilan website lebih ringan ketika diload, otomatis menyesuaikan tampilan website ketika dibuka menggunakan device berbeda (PC,Laptop,maupun Smartphone) sehingga tampilan website akan optimal dimata pengunjung website/visitor. Dengan alasan tersebut,maka tidak sedikit web designer yang menggunakan framework ini. Nah, mungkin penjelasan diatas sudah bisa menerangkan pengertian dari Bootstrap itu sendiri.

#### Bootstrap Grids

Bootstrap grid system memungkinkan hingga 12 kolom di halaman. 


#### Grid Classes
  - xs (untuk ponsel)
  - sm (untuk tablet)
  - md (untuk desktop)
  - lg (untuk desktop yang lebih besar) 

 Kelas-kelas di atas dapat dikombinasikan untuk membuat layout yang lebih dinamis dan fleksibel. 

####  Struktur dasar dari Bootstrap Grid 


```html
 <div class="row">
  <div class="col-sm-4">.col-sm-4</div>
  <div class="col-sm-4">.col-sm-4</div>
  <div class="col-sm-4">.col-sm-4</div>
</div>
```

#### Tabel
```html
<div class="table-responsive">          
  <table class="table">
    <thead>
      <tr>
        <th>#</th>
        <th>Firstname</th>
        <th>Lastname</th>
        <th>Age</th>
        <th>City</th>
        <th>Country</th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td>1</td>
        <td>Anna</td>
        <td>Pitt</td>
        <td>35</td>
        <td>New York</td>
        <td>USA</td>
      </tr>
    </tbody>
  </table>
  </div>
```


#### Gambar


img-rounded
Membuat sudut tumpul pada gambar


```html
 <img src="cinqueterre.jpg" class="img-rounded" alt="Cinque Terre" width="304" height="236">
```

img-circle
Membuat gambar menjadi bulat

```html
 <img src="cinqueterre.jpg" class="img-circle" alt="Cinque Terre" width="304" height="236">
 ```

img-thumbnail
Membuat Gambar thumbnail

```html
 <img src="cinqueterre.jpg" class="img-thumbnail" alt="Cinque Terre" width="304" height="236">
  ```
 
 Gambar responsif 
 ```html
 <img class="img-responsive" src="img_chania.jpg" alt="Chania"> 
  ```

#### Jumbotron

Jumbotron merupakan sebuah style yang telah tersedia di Bootstrap yang bisa kita gunakan untuk meletakkan content yang berukuran cukup besar misalnya seperti untuk membuat header web ataupun content-content gambar yang di rasa membutuhkan ukuran yang cukup besar bisa menggunakan bootstrap agar tampilannya lebih rapi.

```html
<div class="container">
  <div class="jumbotron">
    <h1>Bootstrap Tutorial</h1>      
    <p>Bootstrap is the most popular HTML, CSS, and JS framework for developing responsive, mobile-first projects on the web.</p>
  </div>
  <p>This is some text.</p>      
  <p>This is another text.</p>      
</div>
```