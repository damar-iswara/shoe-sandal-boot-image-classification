# shoe-sandal-boot-image-classification
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.X-orange)
![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Status](https://img.shields.io/badge/Status-Completed-success)
![License](https://img.shields.io/badge/License-MIT-green)

Proyek ini adalah implementasi **Computer Vision** untuk mengklasifikasikan gambar alas kaki menjadi tiga kategori: **Shoe** (Sepatu), **Sandal**, dan **Boot**.

Model dibangun menggunakan **Convolutional Neural Network (CNN)** dengan TensorFlow/Keras. Proyek ini unik karena menerapkan teknik *image processing* berat (Enhancement & Edge Detection) sebelum pelatihan untuk memfokuskan model pada bentuk objek (shape-based features).

## 📋 Daftar Isi
- [Latar Belakang](#-latar-belakang)
- [Dataset](#-dataset)
- [Pipeline Preprocessing](#-pipeline-preprocessing)
- [Arsitektur Model](#-arsitektur-model)
- [Hasil & Evaluasi](#-hasil--evaluasi)
- [Deployment (TFLite & TFJS)](#-deployment)
- [Cara Menjalankan](#-cara-menjalankan)

## 🔍 Latar Belakang
Membedakan jenis alas kaki secara otomatis dapat berguna untuk *e-commerce tagging* atau sistem inventaris cerdas. Tantangan utama dalam dataset ini adalah variasi warna dan latar belakang. Oleh karena itu, pendekatan yang digunakan dalam *notebook* ini mengubah gambar menjadi representasi **Tepi (Edges)** menggunakan Canny Edge Detection agar model lebih robust terhadap variasi warna.

## 📂 Dataset
Dataset yang digunakan berasal dari Kaggle: **[Shoe vs Sandal vs Boot Dataset (15K Images)](https://www.kaggle.com/datasets/hasibalmuzdadid/shoe-vs-sandal-vs-boot-dataset-15k-images)**.

* **Total Gambar:** ~15.000 gambar
* **Kelas:** Boot, Sandal, Shoe
* **Split Data:**
    * Training: 80%
    * Validation: 10%
    * Testing: 10%

## 🛠 Pipeline Preprocessing

Sebelum masuk ke dalam model, gambar melewati beberapa tahap pemrosesan untuk meningkatkan fitur bentuk:

1.  **Image Enhancement**: Meningkatkan kontras (`1.2`) dan ketajaman (`1.1`) serta sedikit menurunkan brightness (`0.9`).
2.  **Grayscale Conversion**: Mengubah gambar RGB menjadi skala abu-abu (1 channel).
3.  **Canny Edge Detection**:  Mengaplikasikan algoritma Canny untuk mendeteksi tepi objek. Ini memaksa model belajar dari *bentuk* sepatu, bukan warnanya.
4.  **Resizing**: Semua gambar diubah ukurannya menjadi `128x128` piksel.

## 🧠 Arsitektur Model

Model menggunakan arsitektur **Sequential CNN** yang didesain dari awal (Custom Architecture): 

* **Input Layer**: 128x128x1 (Grayscale/Edge Image)
* **Feature Extraction**:
    * 3 Blok Konvolusi (Filters: 32, 64, 64).
    * Masing-masing diikuti oleh `MaxPooling2D` dan `BatchNormalization` untuk stabilitas training.
    * `Dropout (0.3)` disisipkan untuk mencegah overfitting.
* **Classifier**:
    * Flatten Layer.
    * Dense Layer (128 units, activation='relu', HeNormal initializer).
    * Output Layer (3 units, activation='softmax').
* **Optimizer**: Adam.
* **Callbacks**: `EarlyStopping` (patience=5) dan `ReduceLROnPlateau`.

## 📊 Hasil & Evaluasi

Evaluasi dilakukan menggunakan data testing yang tidak pernah dilihat model sebelumnya.

| Metrik | Hasil (Approx) |
| :--- | :--- |
| **Akurasi Test** | *~97.33%* |
| **Loss Test** | *0.0862* |

### Visualisasi Performa
<img width="513" height="470" alt="image" src="https://github.com/user-attachments/assets/77ecc1e4-f5da-4d82-ac34-c8f3f827c2bc" />


## 📱 Deployment

Proyek ini tidak hanya berhenti di *notebook*. Model telah dikonversi ke format yang siap produksi:

1.  **SavedModel**: Format standar TensorFlow.
2.  **TensorFlow Lite (.tflite)**:  Dioptimalkan untuk perangkat mobile (Android/iOS) dan IoT. Dilengkapi dengan metadata label.
3.  **TensorFlow.js (TFJS)**: Siap digunakan untuk inferensi langsung di browser (web-based).

## 💻 Cara Menjalankan

1.  **Clone Repository**
    ```bash
    git clone [https://github.com/damar-iswara/shoe-sandal-boot-image-classification.git](https://github.com/damar-iswara/shoe-sandal-boot-image-classification.git)
    cd shoe-sandal-boot-image-classification
    ```

2.  **Install Dependencies**
    ```bash
    pip install -r requirements.txt
    ```

3.  **Jalankan Notebook**
    Buka `main.ipynb` menggunakan Jupyter Notebook atau Google Colab.

    *Catatan: Pastikan Anda memiliki kredensial Kaggle atau mendownload dataset secara manual jika menjalankan di lokal.*

## 👤 Author
**Wahyu Damar Iswara**
* [LinkedIn](https://linkedin.com/in/wahyudamariswara)

---
