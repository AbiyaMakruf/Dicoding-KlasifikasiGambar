# Penilaian Proyek
Proyek ini berhasil mendapatkan bintang 5/5 pada submission dicoding course Belajar Pengembangan Machine Learning.

<img src="https://raw.githubusercontent.com/AbiyaMakruf/Dicoding-KlasifikasiGambar/main/images/nilai.png" width="500">

Kriteria tambahan yang saya kerjakan sehingga mendapat nilai terbaik:
1. Mengimplementasikan Callback
2. Gambar-gambar pada dataset memiliki resolusi yang tidak seragam.
3. Dataset yang digunakan berisi lebih dari 10000 gambar.
4. Akurasi pada training set dan validation set minimal 95%.
5. Memiliki 3 buah kelas atau lebih.
6. Melakukan inference menggunakan salah satu model (TF-Lite, TFJS atau savedmodel dengan tf serving).

# Penjelasan Proyek
Proyek ini merupakan proyek untuk membuat sebuah model yang dapat melakukan klasifikasi gambar. Diberikan kebebasan untuk memilih dataset yang ingin digunakan.

## Dataset
Dataset merupakan data yang diambil dari [GitHub](https://github.com/spMohanty/PlantVillage-Dataset/tree/master). Dataset memiliki total 14 tanaman yang terbagi menjadi 38 kelas berbeda. Secara default resolusi dari gambar adalah 256x256, namun untuk memenuhi kriteria maka dataset secara acak diubah menjadi ukuran dengan range minimum 200x200 hingga 256x256.

## Preview Image
Karena keterbatasan hardware untuk melakukan training, hanya tanaman tomat saja yang dipilih. Berikut adalah contoh gambar dari masing-masing kelas tanaman tomat:

<img src="https://raw.githubusercontent.com/AbiyaMakruf/Dicoding-KlasifikasiGambar/main/images/preview_tomat.png" width="700">

## Distribusi Gambar
Dari 10 kelas tomat dipilih kembali 4 kelas dengan distribusi masing-masing kelas sebagai berikut:

| Condition                     | Number of Images |
|-------------------------------|------------------|
| Tomato_Yellow_Leaf_Curl_Virus | 5357             |
| Late_blight                   | 1909             |
| Healthy                       | 1591             |
| Septoria_leaf_spot            | 1771             |
| **Total**                     | **10628**        |


# Model Evaluasi
## Arsitektur Model
1. **MobileNetV2 Pre-trained**:
    - Menggunakan MobileNetV2 yang telah dilatih pada ImageNet dengan menghapus top layer (`include_top=False`).
    - Ukuran input model adalah `(150, 150, 3)`.

2. **Layer Beku**:
    - Semua layer MobileNetV2 dibekukan (`layer.trainable = False`) untuk mempertahankan bobot dan fitur yang telah dilatih.

3. **Layer Tambahan**:
    - Layer `Conv2D` dengan 32 filter, kernel size 3x3, dan aktivasi ReLU, diikuti oleh `MaxPooling2D` dengan pool size 2x2.
    - Layer `Conv2D` dengan 64 filter, kernel size 3x3, dan aktivasi ReLU, diikuti oleh `MaxPooling2D` dengan pool size 2x2.

4. **Layer Flatten dan Fully Connected**:
    - Fitur yang didapatkan dari layer sebelumnya di-flatten menggunakan `Flatten`.
    - Layer `Dropout` dengan rate 0.5 untuk mencegah overfitting.
    - Layer `Dense` dengan 128 unit dan aktivasi ReLU.
    - Layer output `Dense` dengan 4 unit dan aktivasi softmax untuk klasifikasi multi-kelas.

## Grafik Akurasi dan Loss 

<img src="https://raw.githubusercontent.com/AbiyaMakruf/Dicoding-KlasifikasiGambar/main/images/grafik.png" width="700">


| Epoch | Loss   | Accuracy | Val Loss | Val Accuracy |
|-------|--------|----------|----------|--------------|
| 1/10  | 0.0547 | 0.9843   | 0.0778   | 0.9755       |
| 2/10  | 0.0547 | 0.9843   | 0.0778   | 0.9755       |
| 3/10  | 0.0547 | 0.9843   | 0.0778   | 0.9755       |
| 4/10  | 0.0547 | 0.9843   | 0.0778   | 0.9755       |
| 5/10  | 0.0547 | 0.9843   | 0.0778   | 0.9755       |
| 6/10  | 0.0547 | 0.9843   | 0.0778   | 0.9755       |
| 7/10  | 0.0547 | 0.9843   | 0.0778   | 0.9755       |
| 8/10  | 0.0547 | 0.9843   | 0.0778   | 0.9755       |
| 9/10  | 0.0547 | 0.9843   | 0.0778   | 0.9755       |
| 10/10 | 0.0547 | 0.9843   | 0.0778   | 0.9755       |

## Predict
| No | True                           | Predicted                      |
|----|-------------------------------|--------------------------------|
| 1  | Healthy                        | Healthy                        |
| 2  | Late_blight                    | Late_blight                    |
| 3  | Septoria_leaf_spot             | Septoria_leaf_spot             |
| 4  | Tomato_Yellow_Leaf_Curl_Virus  | Tomato_Yellow_Leaf_Curl_Virus  |

# How To Inference
