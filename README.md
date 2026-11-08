# Spaceship Titanic — VS Code Project

Proyek klasifikasi untuk kompetisi **Spaceship Titanic** yang dijalankan secara lokal menggunakan Visual Studio Code, Jupyter Notebook, dan Python 3.14.

Notebook utama: **`Spaceship_Titanic_VSCode_Evaluasi_Lengkap.ipynb`**

## Struktur folder

Struktur folder yang direkomendasikan:

```text
Spaceship_Titanic_VSCode/
├── .vscode/                                # Pengaturan workspace VS Code (opsional)
├── data/
│   ├── train.csv
│   ├── test.csv
│   └── sample_submission.csv
├── models/                                 # Model hasil training (lokal, di-ignore Git)
├── outputs/                                # Hasil evaluasi, plot, dan submission (lokal)
├── README.md
├── requirements.txt
├── .gitignore
├── spaceship_titanic_outputs.zip           # Arsip hasil eksperimen (di-ignore Git)
└── Spaceship_Titanic_VSCode_Evaluasi_Lengkap.ipynb
```

Notebook mencari ketiga CSV di folder `data/` relatif terhadap folder kerja proyek. Buka **folder utama proyek** di VS Code melalui `File → Open Folder`, bukan hanya file notebook-nya.

## Persyaratan

- Windows 10/11
- Python 3.14.x (versi yang digunakan saat proyek ini disiapkan: Python 3.14.10)
- Visual Studio Code
- Extension **Python** dan **Jupyter** di VS Code
- Koneksi internet saat instalasi package; XGBoost perlu tersedia di environment Python

Notebook menggunakan Python lokal dan tidak membutuhkan Google Colab, Google Drive, maupun Kaggle API.

## 1. Periksa Python dan pip

Buka terminal PowerShell di VS Code pada folder utama proyek:

```powershell
python --version
python -c "import sys; print(sys.executable)"
python -m pip --version
```

Pastikan perintah `python` menunjuk ke interpreter Python 3.14 yang ingin digunakan. Contoh lokasi pada komputer pengembang:

```text
C:\Users\ASUS\AppData\Local\Python\pythoncore-3.14-64\python.exe
```

Lokasi interpreter di komputer lain dapat berbeda.

## 2. Instal semua dependensi

Virtual environment tidak wajib. Untuk instalasi pada Python Global, jalankan:

```powershell
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Jika menggunakan interpreter tertentu, jalankan pip melalui path interpreter tersebut, misalnya:

```powershell
& "C:\path\ke\python.exe" -m pip install -r requirements.txt
```

Jangan gunakan `--break-system-packages`. Jika instalasi global ditolak oleh kebijakan environment Python, gunakan virtual environment khusus proyek.

## 3. Daftarkan dan pilih Jupyter kernel

Daftarkan kernel satu kali:

```powershell
python -m ipykernel install --user --name spaceship-titanic-314 --display-name "Python 3.14 - Spaceship Titanic"
```

Di VS Code:

1. Buka `Spaceship_Titanic_VSCode_Evaluasi_Lengkap.ipynb`.
2. Klik **Select Kernel**.
3. Pilih **Python 3.14 - Spaceship Titanic**.
4. Jalankan **Restart Kernel**, kemudian **Run All**.

## Model yang dibandingkan

Semua model berikut dimasukkan ke satu proses training dan perbandingan:

1. Logistic Regression
2. Random Forest
3. Extra Trees
4. Gradient Boosting
5. Hist Gradient Boosting
6. XGBoost

XGBoost bukan eksperimen opsional yang berdiri sendiri. Semua kandidat menerima data training dan validation split yang sama.

## Alur eksperimen dan evaluasi

1. Membaca `train.csv`, `test.csv`, dan `sample_submission.csv`.
2. Melakukan pemeriksaan data dan visualisasi EDA.
3. Membuat fitur turunan, antara lain `GroupSize`, `Deck`, `CabinNum`, `Side`, `TotalSpend`, dan `AgeGroup`.
4. Melakukan stratified train-validation split 80:20.
5. Melatih keenam model dan membandingkan hasilnya.
6. Memilih model terbaik berdasarkan **validation accuracy**.
7. Menampilkan evaluasi multi-metrik dan grafik.
8. Menjalankan 5-Fold Stratified Cross-Validation untuk semua model jika `RUN_CV = True`.
9. Melatih ulang model terpilih menggunakan seluruh data training.
10. Menghasilkan `submission.csv`.

### Metrik evaluasi

- Accuracy
- Balanced Accuracy
- Precision
- Recall / Sensitivity
- Specificity
- F1-Score
- ROC-AUC
- PR-AUC (Average Precision)
- Log Loss
- Matthews Correlation Coefficient (MCC)

### Grafik evaluasi

- Perbandingan metrik seluruh model
- ROC Curve seluruh model
- Precision–Recall Curve seluruh model
- Confusion Matrix seluruh model
- Confusion Matrix dan normalized confusion matrix model terbaik
- Boxplot accuracy dari 5-fold cross-validation

Cross-validation memerlukan waktu komputasi tambahan. Pengaturannya berada pada cell `RUN_CV`.

## Output

Setelah notebook berhasil dijalankan, notebook membuat direktori berikut:

```text
outputs/
├── submission.csv
├── validation_results.csv
├── cross_validation_results.csv
├── run_info.json
├── models/
│   └── <model_terpilih>_model.joblib
└── plots/
    ├── 01_model_metric_comparison.png
    ├── 02_roc_curves.png
    ├── 03_precision_recall_curves.png
    ├── 04_confusion_matrices_all_models.png
    ├── 05_best_model_confusion_matrix.png
    └── 06_cv_accuracy_boxplot.png
```

Notebook juga membuat file **`spaceship_titanic_outputs.zip`** di folder utama proyek. Arsip ini berisi seluruh isi folder `outputs/`.


## Catatan

- Model terbaik ditentukan dari validation accuracy. Hasil 5-fold cross-validation merupakan evaluasi kestabilan tambahan, bukan metrik pemilihan otomatis pada notebook ini.
- Validation accuracy lokal bukan skor leaderboard Kaggle. Skor kompetisi yang sebenarnya diperoleh setelah submission diunggah.
- Jangan commit token, password, credential, atau file rahasia ke GitHub.
