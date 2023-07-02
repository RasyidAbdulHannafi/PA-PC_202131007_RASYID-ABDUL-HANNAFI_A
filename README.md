
# PENJELASAN KODINGAN 

1. Impor Library:

`import cv2`: Library `cv2` digunakan untuk pengolahan citra dan komputer grafis menggunakan OpenCV. Ini adalah library utama yang akan digunakan dalam program ini.

`import numpy as np`: Library `numpy` digunakan untuk melakukan operasi matematika pada array multidimensi. Alias `np` digunakan untuk memudahkan penggunaan fungsi-fungsi dari library ini.

`import matplotlib.pyplot as plt`: Library `matplotlib.pyplot` digunakan untuk membuat visualisasi grafis dari data. Alias `plt` digunakan untuk memudahkan penggunaan fungsi-fungsi dari library ini.

2. Membaca dan Meresize Gambar:

 `img = cv2.imread('rasyid_abdul_hannafi.jpg')`: Baris ini membaca gambar dengan nama file "rasyid_abdul_hannafi.jpg" menggunakan fungsi `cv2.imread()`. Hasil pembacaan gambar disimpan dalam variabel `img`. Fungsi `cv2.imread()` digunakan untuk membaca gambar dari file dan mengembalikan citra dalam format BGR (Blue-Green-Red).

 `img = cv2.cvtColor(img, cv2.COLOR_BGR2RGB)`: Baris ini mengubah format warna gambar dari BGR (Blue-Green-Red) menjadi RGB (Red-Green-Blue). Fungsi `cv2.cvtColor()` digunakan untuk melakukan konversi format warna gambar. Dalam hal ini, parameter `cv2.COLOR_BGR2RGB` digunakan untuk mengubah gambar dari format BGR menjadi format RGB.

 `h, w, c = img.shape`: Baris ini mengambil dimensi gambar yang telah dibaca dan diubah format warnanya sebelumnya. Fungsi `img.shape` mengembalikan tuple dengan tiga nilai: tinggi (h), lebar (w), dan jumlah saluran warna (c) dari gambar. Variabel `h`, `w`, dan `c` akan menyimpan nilai-nilai tersebut.

 `if w > 1000:`: Baris ini memeriksa apakah lebar gambar (`w`) lebih besar dari 1000 piksel.

 `new_w = 1000`: Baris ini menginisialisasi nilai lebar baru (`new_w`) dengan 1000 piksel. Jika lebar gambar asli lebih besar dari 1000 piksel, maka gambar akan diresize sehingga lebarnya menjadi 1000 piksel.

 `ar = w / h`: Baris ini menghitung rasio aspek (aspek ratio) gambar, yaitu lebar dibagi dengan tinggi.

 `new_h = int(new_w / ar)`: Baris ini menghitung tinggi baru (`new_h`) dengan membagi lebar baru (`new_w`) dengan rasio aspek (`ar`). Hasil perhitungan diubah menjadi bilangan bulat menggunakan fungsi `int()`.

 `img = cv2.resize(img, (new_w, new_h), interpolation=cv2.INTER_AREA)`: Baris ini mengubah ukuran gambar (`img`) menjadi ukuran baru dengan menggunakan fungsi `cv2.resize()`. Parameter pertama adalah gambar yang akan diresize, parameter kedua adalah ukuran baru yang diinginkan (lebar, tinggi), dan parameter `interpolation=cv2.INTER_AREA` digunakan untuk mengatur metode interpolasi saat meresize gambar. Dalam hal ini, metode `cv2.INTER_AREA` digunakan untuk meresize gambar dengan mengambil rata-rata piksel dalam area yang lebih besar.

3. Pre-processing:

`img_gray = cv2.cvtColor(img, cv2.COLOR_RGB2GRAY)`: Baris ini mengubah gambar RGB (`img`) menjadi citra grayscale menggunakan fungsi `cv2.cvtColor()`. Parameter pertama adalah gambar yang akan diubah warnanya, dan parameter kedua `cv2.COLOR_RGB2GRAY` menentukan konversi warna dari RGB ke grayscale.

`ret, thresh = cv2.threshold(img_gray, 80, 255, cv2.THRESH_BINARY_INV)`: Baris ini melakukan proses thresholding pada citra grayscale (`img_gray`). Thresholding adalah proses mengubah citra menjadi citra biner, yaitu setiap piksel diubah menjadi hitam atau putih berdasarkan nilai threshold. Fungsi `cv2.threshold()` digunakan untuk melakukan thresholding. Parameter pertama adalah citra grayscale yang akan di-threshold, parameter kedua `80` adalah nilai threshold yang digunakan untuk memisahkan piksel menjadi hitam dan putih, parameter ketiga `255` adalah nilai maksimum yang diberikan kepada piksel yang melebihi threshold, dan parameter keempat `cv2.THRESH_BINARY_INV` menentukan jenis thresholding yang digunakan yaitu threshold binary yang menghasilkan gambar biner dengan latar belakang hitam dan objek putih.


4. Segmentasi Karakter:

`kernel = np.ones((3, 15), np.uint8)`: Baris ini mendefinisikan sebuah kernel yang digunakan untuk operasi dilasi pada gambar biner. Kernel yang digunakan adalah matriks berukuran 3x15 dengan semua elemennya bernilai 1 (np.ones). Tipe data kernel diatur sebagai np.uint8.

`dilated = cv2.dilate(thresh, kernel, iterations=1)`: Baris ini melakukan operasi dilasi pada gambar biner (`thresh`) menggunakan kernel yang telah didefinisikan sebelumnya. Fungsi `cv2.dilate()` digunakan untuk melakukan operasi dilasi, yang memperluas area objek pada gambar. Parameter pertama adalah gambar biner yang akan didilasi, parameter kedua adalah kernel yang digunakan, dan parameter ketiga `iterations=1` menentukan jumlah iterasi operasi dilasi yang dilakukan.

`characters_list = []`: Baris ini mendefinisikan sebuah list kosong `characters_list` yang akan digunakan untuk menyimpan koordinat kotak pembatas karakter.

`(cnt, _) = cv2.findContours(dilated.copy(), cv2.RETR_EXTERNAL, cv2.CHAIN_APPROX_NONE)`: Baris ini menggunakan fungsi `cv2.findContours()` untuk mencari kontur pada gambar hasil dilasi (`dilated`). Fungsi ini mengembalikan kontur yang ditemukan dalam list `cnt` dan variabel `_` digunakan untuk menangkap nilai yang tidak diperlukan. Parameter pertama adalah gambar yang akan dicari konturnya, parameter kedua `cv2.RETR_EXTERNAL` menentukan mode pengambilan kontur yang hanya mengambil kontur eksternal, dan parameter ketiga `cv2.CHAIN_APPROX_NONE` menentukan metode aproksimasi kontur yang digunakan.

`sorted_contour_chars = sorted(cnt, key=lambda cntr: cv2.boundingRect(cntr)[0])`: Baris ini mengurutkan kontur-kontur yang ditemukan berdasarkan koordinat x dari kotak pembatasnya. Fungsi `sorted()` digunakan dengan parameter pertama adalah list kontur yang akan diurutkan (`cnt`), dan parameter kedua `key=lambda cntr: cv2.boundingRect(cntr)[0]` menentukan kunci pengurutan berdasarkan koordinat x dari kotak pembatas setiap kontur.

`for character in sorted_contour_chars:`: Baris ini melakukan iterasi pada setiap kontur yang telah diurutkan.

`x, y, w, h = cv2.boundingRect(character)`: Baris ini mendapatkan koordinat kotak pembatas (x, y, w, h) dari setiap kontur menggunakan fungsi `cv2.boundingRect()`. Koordinat ini mengelilingi setiap kontur dengan kotak pembatas minimum yang melingkupinya.

`characters_list.append([x, y, x + w, y + h])`: Baris ini menambahkan koordinat kotak pembatas karakter ke dalam list `characters_list`. Koordinat ini berupa tuple (x, y, x + w, y + h) yang menunjukkan titik kiri atas dan kanan bawah

 dari kotak pembatas karakter.

`cv2.rectangle(img, (x, y), (x + w, y + h), (255, 145, 100), 7)`: Baris ini menggambar kotak pembatas karakter pada gambar asli (`img`) menggunakan fungsi `cv2.rectangle()`. Parameter pertama adalah gambar yang akan digambar kotak pembatasnya, parameter kedua adalah titik kiri atas kotak pembatas (x, y), parameter ketiga adalah titik kanan bawah kotak pembatas (x + w, y + h), parameter keempat `(255, 145, 100)` adalah warna kotak pembatas dalam format BGR, dan parameter kelima `7` adalah ketebalan garis kotak pembatas.

5. Menampilkan Gambar Asli dan Gambar Setelah Diubah:

`fig, axes = plt.subplots(1, 2, figsize=(10, 5))`: Baris ini membuat sebuah figure dengan dua subplot. Parameter pertama `1` menunjukkan jumlah baris subplot, parameter kedua `2` menunjukkan jumlah kolom subplot, dan parameter `figsize=(10, 5)` menentukan ukuran gambar dalam satuan inch.

 `axes[0].imshow(cv2.cvtColor(cv2.imread('rasyid_abdul_hannafi.jpg'), cv2.COLOR_BGR2RGB))`: Baris ini menampilkan gambar asli sebelum diedit pada subplot pertama (indeks 0). Fungsi `cv2.imread()` digunakan untuk membaca gambar dari file `'rasyid_abdul_hannafi.jpg'`, lalu `cv2.cvtColor()` digunakan untuk mengubah warna gambar dari BGR ke RGB. Fungsi `imshow()` digunakan untuk menampilkan gambar pada subplot.

`axes[0].set_title('Gambar Asli')`: Baris ini menambahkan judul "Gambar Asli" pada subplot pertama.

`axes[0].axis('off')`: Baris ini menghilangkan tampilan sumbu pada subplot pertama.

`segmented_img = img.copy()`: Baris ini membuat salinan dari gambar setelah dilakukan operasi segmentasi huruf per huruf (`img`) dan menyimpannya dalam variabel `segmented_img`.

`for character in characters_list:`: Baris ini melakukan iterasi pada setiap karakter dalam `characters_list`, yang berisi koordinat kotak pembatas karakter.

`x1, y1, x2, y2 = character`: Baris ini memecah koordinat kotak pembatas karakter menjadi empat variabel terpisah, yaitu `x1`, `y1`, `x2`, dan `y2`.

`cv2.rectangle(segmented_img, (x1, y1), (x2, y2), (10, 250, 10), 7)`: Baris ini menggambar kotak pembatas karakter pada gambar hasil segmentasi huruf per huruf (`segmented_img`). Fungsi `cv2.rectangle()` digunakan dengan parameter pertama adalah gambar yang akan digambar kotak pembatasnya, parameter kedua adalah titik kiri atas kotak pembatas (x1, y1), parameter ketiga adalah titik kanan bawah kotak pembatas (x2, y2), parameter keempat `(10, 250, 10)` adalah warna kotak pembatas dalam format BGR, dan parameter kelima `7` adalah ketebalan garis kotak pembatas.

`axes[1].imshow(segmented_img)`: Baris ini menampilkan gambar setelah dilakukan segmentasi huruf per huruf pada subplot kedua (indeks 1).

 `axes[1].set_title('Setelah Diubah (Segmentasi Per Huruf)')`: Baris ini menambahkan judul "Setelah Diubah (Segmentasi Per Huruf)" pada subplot kedua.

 `axes[1].axis('off')`: Baris ini menghilangkan tampilan sumbu pada subplot kedua.

`plt.tight_layout()`: Baris ini dig

unakan untuk mengatur tata letak subplot agar tidak tumpang tindih.

 `plt.show()`: Baris ini menampilkan plot yang telah dibuat.

