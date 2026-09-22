# Analisis-Fitur-untuk-Klasifikasi-Gulma-pada-Tanaman-Rumput-Menggunakan-SVM
Project ini merupakan implementasi **image classification** untuk membedakan antara **gulma (weed)** dan **background** pada area tanaman rumput.
Pada project ini, citra dianalisis berdasarkan tiga jenis karakteristik, yaitu **warna, tekstur, dan bentuk**. Ketiga fitur tersebut kemudian digabungkan dan digunakan sebagai input untuk model **Support Vector Machine (SVM)**.

## Overview

Pipeline yang digunakan dalam project ini:

```text
Image Dataset
      ↓
Pre-processing
      ↓
Feature Extraction
 ┌────┼────┐
 ↓    ↓    ↓
HSV  GLCM  HOG
 └────┼────┘
      ↓
Feature Fusion
      ↓
Standardization
      ↓
PCA
      ↓
SVM
      ↓
Weed / Background
```

## Feature Extraction

### HSV — Color

HSV digunakan untuk mengambil informasi warna dari citra.

Fitur yang digunakan berupa:

* Mean Hue
* Standard Deviation Hue
* Mean Saturation
* Standard Deviation Saturation
* Mean Value
* Standard Deviation Value

Dari beberapa percobaan, kombinasi **mean + standard deviation** memberikan hasil yang paling baik untuk representasi warna.

### GLCM — Texture

GLCM digunakan untuk mendapatkan informasi tekstur dari citra grayscale.

Fitur yang digunakan:

* Contrast
* Correlation
* Energy
* Homogeneity

Pengujian dilakukan menggunakan beberapa nilai `distance`, dengan hasil terbaik pada **distance = 2**.

### HOG — Shape

HOG digunakan untuk menangkap informasi bentuk dan edge pada objek.

Beberapa kombinasi parameter HOG diuji, dengan konfigurasi terbaik yang digunakan dalam tahap akhir:

```text
orientations      = 9
pixels_per_cell   = (8, 8)
cells_per_block   = (2, 2)
```

## Feature Fusion

Setelah masing-masing fitur diperoleh, HSV, GLCM, dan HOG digabungkan menggunakan **feature concatenation**.

Hasil penggabungan menghasilkan **8.110 fitur** untuk setiap citra. Karena jumlah fitur cukup besar, dilakukan standardisasi dan reduksi dimensi menggunakan PCA.

## PCA

PCA digunakan untuk mengurangi jumlah fitur sekaligus mempertahankan sebagian besar informasi dari data.

Dari hasil analisis, digunakan **1.199 komponen PCA** untuk mempertahankan sekitar 90% variasi data.

Dengan demikian, jumlah fitur dapat dikurangi secara signifikan sebelum masuk ke proses klasifikasi.

## Classification

Model klasifikasi yang digunakan adalah **Support Vector Machine (SVM)** dengan:

```text
kernel       = RBF
C            = 10
gamma        = scale
class_weight = balanced
```

Pemilihan parameter dilakukan menggunakan `GridSearchCV` dengan **5-fold cross validation** dan `macro F1` sebagai metrik utama.

## Evaluation

Performa model dievaluasi menggunakan:

* Accuracy
* Macro F1-Score
* Classification Report
* Confusion Matrix

Evaluasi dilakukan pada **validation set** dan **test set** untuk melihat performa model pada data yang berbeda dari data training.

Selain evaluasi numerik, project ini juga menampilkan beberapa contoh prediksi yang benar dan salah untuk melihat bagaimana model mengklasifikasikan citra secara langsung.

## Dataset

Dataset yang digunakan adalah **Grass Weeds** dari Roboflow. Dataset dibagi menjadi:

```text
train
valid
test
```

Pada tahap preprocessing, citra diproses melalui beberapa langkah seperti cropping ROI, resize menjadi **128 × 128 piksel**, Gaussian Blur, serta konversi warna sebelum dilakukan ekstraksi fitur.

## Tech Stack

* Python
* OpenCV
* NumPy
* Pandas
* Matplotlib
* Scikit-image
* Scikit-learn
* PyWavelets
* Roboflow

## Project Structure

```text
.
├── Source_Code_Projek_PCD_Kelompok_1.ipynb
└── README.md
```

Image Processing & Machine Learning Project
Python | Computer Vision | Feature Extraction | SVM
