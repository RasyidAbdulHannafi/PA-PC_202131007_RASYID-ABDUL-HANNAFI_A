
# PENJELASAN KODINGAN 
Kodingan di atas adalah sebuah program untuk melakukan segmentasi karakter pada sebuah gambar menggunakan library OpenCV (Open Source Computer Vision Library). Berikut adalah penjelasan langkah-langkah yang dilakukan oleh program ini:

1. Impor Library:
   - Program ini menggunakan beberapa library, yaitu:
     - cv2: Library utama untuk pengolahan citra dan komputer grafis.
     - numpy (np): Library untuk melakukan operasi matematika pada array multidimensi.
     - matplotlib.pyplot (plt): Library untuk membuat visualisasi grafis dari data.

2. Membaca dan meresize gambar:
   - Gambar dengan nama 'rasyid_abdul_hannafi.jpg' dibaca menggunakan fungsi `cv2.imread()` dan disimpan dalam variabel `img`.
   - Gambar dikonversi dari format warna BGR ke RGB menggunakan fungsi `cv2.cvtColor()`.
   - Variabel `h`, `w`, dan `c` digunakan untuk menyimpan dimensi gambar (tinggi, lebar, dan jumlah saluran warna).

3. Pre-processing:
   - Gambar diubah menjadi grayscale menggunakan fungsi `cv2.cvtColor()` dengan parameter `cv2.COLOR_RGB2GRAY`.
   - Thresholding dilakukan pada gambar grayscale menggunakan fungsi `cv2.threshold()`. Nilai ambang (threshold) yang digunakan adalah 80, dan hasilnya disimpan dalam variabel `thresh`. Metode `cv2.THRESH_BINARY_INV` digunakan untuk menghasilkan citra biner dengan inversi.

4. Segmentasi karakter:
   - Sebuah kernel dengan ukuran 3x15 dibuat menggunakan `np.ones()` dan disimpan dalam variabel `kernel`.
   - Dilasi (dilation) dilakukan pada citra biner hasil thresholding menggunakan fungsi `cv2.dilate()` dengan kernel yang telah dibuat.
   - Fungsi `cv2.findContours()` digunakan untuk menemukan kontur pada citra hasil dilasi. Kontur yang ditemukan disimpan dalam variabel `cnt`.
   - Kontur-kontur tersebut diurutkan berdasarkan koordinat x dari kotak pembatas (bounding box) menggunakan fungsi `sorted()`.
   - Setiap karakter pada kontur yang telah diurutkan diambil koordinat dan ukurannya menggunakan `cv2.boundingRect()`, dan kotak pembatas karakter tersebut ditambahkan ke dalam `characters_list`.
   - Kotak pembatas karakter ditampilkan pada gambar menggunakan `cv2.rectangle()`.

5. Menampilkan gambar asli dan gambar setelah diubah:
   - Objek `fig` dan `axes` dibuat menggunakan `plt.subplots()` untuk menampilkan dua gambar secara berdampingan.
   - Gambar asli sebelum diedit ditampilkan pada `axes[0]` menggunakan `plt.imshow()`.
   - Judul 'Gambar Asli' ditambahkan pada `axes[0]` menggunakan `axes[0].set_title()`.
   - Axis pada `axes[0]` dinonaktifkan menggunakan `axes[0].axis('off')`.
   - Gambar setelah diubah dengan segmentasi huruf per huruf ditampilkan pada `axes[1]` menggunakan `plt.imshow()`.
   - Judul 'Setelah Diubah (Segmentasi Per Huruf)' ditambahkan pada `axes[1]` menggunakan `axes[1].set_title()`.
   - Axis pada `axes[1]` dinonaktifkan menggunakan `axes[1].axis('off')`.
   - Tampilan

 grafis yang telah dibuat ditampilkan menggunakan `plt.show()`.

Dengan demikian, program ini membaca gambar, melakukan segmentasi karakter pada gambar tersebut, dan menampilkan gambar asli beserta gambar setelah dilakukan segmentasi huruf per huruf.