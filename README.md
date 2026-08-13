# Semantic Search with Fine-Tuned DistilBERT

Repository ini berisi implementasi penelitian mengenai **Semantic Search untuk pencarian dokumen arsip akademik** menggunakan model **DistilBERT** yang telah melalui proses fine-tuning dan **FAISS** sebagai library untuk pencarian berbasis vektor.

Penelitian dilakukan melalui tiga notebook yang saling berkesinambungan, mulai dari eksplorasi dataset, fine-tuning model, hingga implementasi dan evaluasi semantic search.

---

## 📌 Tujuan Penelitian

Penelitian ini bertujuan untuk mengembangkan sistem pencarian dokumen yang tidak hanya bergantung pada kecocokan kata secara langsung, tetapi dapat mempertimbangkan **kemiripan makna antara query dan dokumen**.

Pendekatan yang digunakan adalah:

**Dataset → Preprocessing → Fine-tuning DistilBERT → Text Embedding → L2 Normalization → FAISS → Semantic Retrieval → Evaluation**

---

## 📂 Dataset

Dataset yang digunakan merupakan kumpulan dokumen arsip akademik dengan total:

- **784 dokumen**
- Format awal: XLSX
- Atribut utama:
  - `title`
  - `description`
  - `keywords`
  - `categoryId`

Dataset terdiri dari beberapa kategori arsip, antara lain:

- Foto & Dokumentasi
- SK & Kebijakan Universitas
- Surat Menyurat & Kunjungan
- Akreditasi & Sertifikasi
- Panduan & Pedoman Akademik
- Laporan Kegiatan
- SOP & Anggaran

Distribusi kategori dianalisis terlebih dahulu melalui tahap Exploratory Data Analysis (EDA).

---

# 📓 Notebook 1 — Exploratory Data Analysis

Notebook pertama digunakan untuk memahami karakteristik dataset sebelum masuk ke proses pelatihan model.

### Tahapan

1. Memuat dataset XLSX
2. Memeriksa struktur dan atribut data
3. Mengidentifikasi jumlah dokumen
4. Memeriksa distribusi kategori berdasarkan `categoryId`
5. Memeriksa data yang kosong atau tidak lengkap
6. Melakukan preprocessing teks
7. Menggabungkan atribut teks menjadi representasi dokumen

### Preprocessing

Beberapa proses preprocessing yang digunakan:

- Lowercase
- Penghapusan karakter khusus
- Normalisasi spasi
- Penggabungan `title`, `description`, dan `keywords`

Output dari notebook ini digunakan sebagai data untuk proses fine-tuning pada Notebook 2.

---

# 📓 Notebook 2 — Fine-Tuning DistilBERT

Notebook kedua berfokus pada proses penyesuaian model bahasa **cahya/distilbert-base-indonesian** agar representasi teks lebih sesuai dengan karakteristik dokumen arsip yang digunakan dalam penelitian.

### Model

**Base Model:**

`cahya/distilbert-base-indonesian`

Model digunakan untuk menghasilkan representasi teks berdimensi **768**.

### Proses Fine-Tuning

Tahapan utama:

1. Memuat dataset hasil preprocessing
2. Membentuk pasangan data untuk proses contrastive fine-tuning
3. Melakukan tokenisasi menggunakan tokenizer DistilBERT
4. Melatih model menggunakan beberapa nilai seed
5. Mengevaluasi train loss dan validation loss
6. Mengukur similarity intra-category
7. Mengukur similarity inter-category
8. Menghitung separation gap
9. Membandingkan hasil antar-seed

### Multi-Seed Experiment

Fine-tuning dilakukan menggunakan lima seed:

```text
42
123
2024
7
99.
```

# 📓 Notebook 3 — Semantic Search & Evaluation
Pipeline :

```
Dokumen
   ↓
Tokenizer DistilBERT
   ↓
DistilBERT
   ↓
Mean Pooling
   ↓
Embedding 768 Dimensi
   ↓
L2 Normalization
   ↓
FAISS IndexFlatIP
   ↓
Semantic Retrieval
   ↓
Top-K Documents
```
