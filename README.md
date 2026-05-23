# 🎯 Analisis Sentimen Bahasa Indonesia dengan Transformers

Proyek penelitian untuk **perbandingan dan evaluasi model Transformers** dalam melakukan analisis sentimen pada komentar YouTube berbahasa Indonesia. Proyek ini mengimplementasikan dan membandingkan beberapa model pre-trained Indonesia seperti **IndoBERT**, **IndoBERTweet**, dan **IndoRoBERTa**.

## ✨ Fitur Utama

- 🔄 **Komparasi Model**: Perbandingan performa 3 model Transformers Indonesia
  - IndoBERT
  - IndoBERTweet
  - IndoRoBERTa
- 📊 **Pipeline Lengkap**: Data collection → EDA → Preprocessing → Labeling → Training
- 🎨 **Web Application**: Streamlit app untuk testing model secara interaktif
- 📈 **MLFlow Integration**: Tracking eksperimen dan model versioning
- 🧹 **Text Preprocessing**: Case Folding, Cleaning, normalisasi, handling slang Indonesia
- 💾 **Model Artifacts**: Format .safetensors untuk deployment efficient

## 📁 Struktur Proyek

```
.
├── notebook/                           # Jupyter Notebooks
│   ├── 1_DataCollection.ipynb         # Pengumpulan data YouTube
│   ├── 2_Eda_Preprocessing.ipynb      # Exploratory Data Analysis & Preprocessing
│   ├── 3_Labeling_Leksion_RoBERTa.ipynb
│   ├── 4_Model_Final.ipynb            # Training model final
│   ├── 5_Model_MLFlow_Logging.ipynb   # MLFlow experiment tracking
│   └── Logging_DagsHub.ipynb          # DagsHub integration
├── deploy-streamlit/                   # Streamlit deployment
│   └── sentiment-app.py
├── requirements.txt
└── README.md
```

## 🚀 Instalasi

### Prerequisites

- Python 3.8+
- pip atau conda

### Setup Environment

1. **Clone repository**

   ```bash
   git clone <repository-url>
   cd "Sentiment Analysis dengan Transformers"
   ```

2. **Buat Virtual Environment**

   ```bash
   python -m venv .venv
   ```

3. **Aktivasi Virtual Environment**
   - Windows (PowerShell):
     ```bash
     .\.venv\Scripts\Activate.ps1
     ```
   - Windows (Command Prompt):
     ```bash
     .venv\Scripts\activate.bat
     ```
   - Linux/Mac:
     ```bash
     source .venv/bin/activate
     ```

4. **Install Dependencies**
   ```bash
   pip install -r requirements.txt
   ```

## 📚 Dataset

### Sumber Data

- **Platform**: YouTube Comments
- **Bahasa**: Indonesian
- **Total Records**: ~11,000 komentar
- **Labels**: Sentimen positif, negatif, neutral

### File Dataset

- `dataset_komentar_youtube_11k.csv` - Dataset lengkap
- `Komentar_Youtube_bersih_berlabel*.csv` - Dataset yang sudah dibersihkan dan berlabel
- `slang_indonesia.csv` - Mapping slang Indonesia untuk normalisasi

## 🏗️ Pipeline Pengerjaan

### 1. Data Collection (`1_DataCollection.ipynb`)

Mengumpulkan komentar YouTube dari berbagai channel

### 2. EDA & Preprocessing (`2_Eda_Preprocessing.ipynb`)

- Exploratory Data Analysis
- Text cleaning (lowercase, remove punctuation, etc.)
- Normalisasi slang Indonesia
- Tokenisasi dan pembersihan teks

### 3. Labeling (`3_Labeling_Leksion_RoBERTa.ipynb`)

Proses pelabelan sentimen (Positif/Negatif/Neutral)

### 4. Model Training (`4_Model_Final.ipynb`)

- Training 3 model Transformers
- Fine-tuning pada dataset Indonesian
- Evaluasi metrik (Accuracy, Precision, Recall, F1-Score)

### 5. Experiment Tracking (`5_Model_MLFlow_Logging.ipynb`)

- MLFlow logging untuk tracking semua eksperimen
- Model versioning dan comparison

## 🤖 Model yang Digunakan

| Model            | Basis   | Pre-training         | Catatan                            |
| ---------------- | ------- | -------------------- | ---------------------------------- |
| **IndoBERT**     | BERT    | Indonesian Wikipedia | General-purpose                    |
| **IndoBERTweet** | BERT    | Indonesian Twitter   | Domain-specific untuk social media |
| **IndoRoBERTa**  | RoBERTa | Indonesian Corpus    | Improved RoBERTa architecture      |

### Model Artifacts

Semua model tersimpan dalam format `.safetensors` untuk efisiensi:

- `model/saved_model_IndoBERT/`
- `model/saved_model_indoBERTweet/`
- `model/saved_model_IndoRoBERTa/`

## 💻 Penggunaan

### Menggunakan Streamlit App

```bash
cd deploy-streamlit
streamlit run sentiment-app.py
```

Akses aplikasi di: `http://localhost:8501`

### Menggunakan Model Secara Programatik

```python
from transformers import AutoTokenizer, AutoModelForSequenceClassification
import torch

# Load model
model_name = "path/to/saved_model_IndoBERT"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModelForSequenceClassification.from_pretrained(model_name)

# Predict
text = "Produk ini sangat bagus dan memuaskan!"
inputs = tokenizer(text, return_tensors="pt")
outputs = model(**inputs)
predictions = torch.argmax(outputs.logits, dim=1)

# Output: 0=Negatif, 1=Neutral, 2=Positif
print(f"Sentimen: {predictions.item()}")
```

## 📊 Hasil dan Performa

Model performa dapat dilihat dari MLFlow tracking:

```bash
# Jalankan MLFlow UI
mlflow ui
```

Akses di: `http://localhost:5000`

### Metrik Evaluasi

- Accuracy
- Precision
- Recall
- F1-Score
- Confusion Matrix

## 🔧 Teknologi yang Digunakan

- **Deep Learning**: PyTorch
- **NLP**: Hugging Face Transformers
- **Preprocessing**: NLTK, TextPreprocessing
- **Experiment Tracking**: MLFlow, DagsHub
- **Web App**: Streamlit
- **Data Processing**: Pandas, NumPy
- **Visualization**: Matplotlib, Seaborn

## 📋 Requirements

Lihat file `requirements.txt` untuk detail lengkap dependencies:

```
torch>=1.12.0
transformers>=4.20.0
pandas>=1.3.0
numpy>=1.21.0
scikit-learn>=0.24.0
nltk>=3.6.0
streamlit>=1.20.0
mlflow>=1.20.0
safetensors>=0.2.0
```

Install dengan:

```bash
pip install -r requirements.txt
```

## 🎓 Struktur Notebook

| Notebook                     | Deskripsi                                    |
| ---------------------------- | -------------------------------------------- |
| `1_DataCollection`           | Scraping & pengumpulan data YouTube          |
| `2_Eda_Preprocessing`        | Analisis data dan teknik preprocessing       |
| `3_Labeling_Leksion_RoBERTa` | Proses labeling dengan bantuan model RoBERTa |
| `4_Model_Final`              | Training dan evaluasi semua model            |
| `5_Model_MLFlow_Logging`     | Logging ke MLFlow untuk tracking             |
| `Logging_DagsHub`            | Integrasi dengan DagsHub untuk collaboration |

## 📈 Cara Mereproduksi Hasil

1. **Jalankan Data Collection**

   ```bash
   jupyter notebook notebook/1_DataCollection.ipynb
   ```

2. **Jalankan Preprocessing**

   ```bash
   jupyter notebook notebook/2_Eda_Preprocessing.ipynb
   ```

3. **Jalankan Training**

   ```bash
   jupyter notebook notebook/4_Model_Final.ipynb
   ```

4. **Monitor di MLFlow**
   ```bash
   mlflow ui
   ```

## 🤝 Kontribusi

Kontribusi sangat diharapkan! Silakan:

1. Fork repository ini
2. Buat branch fitur (`git checkout -b feature/AmazingFeature`)
3. Commit perubahan (`git commit -m 'Add some AmazingFeature'`)
4. Push ke branch (`git push origin feature/AmazingFeature`)
5. Buat Pull Request

## ⚠️ Catatan Penting

- **Data Privacy**: Dataset YouTube comments telah diproses dan disanitasi
- **Model Size**: Model IndoBERTweet (~500MB), pastikan memiliki storage yang cukup
- **GPU**: Direkomendasikan menggunakan GPU (CUDA) untuk training
- **Slang Dictionary**: File `slang_indonesia.csv` berisi mapping untuk normalisasi teks

## 📞 Kontak & Support

Untuk pertanyaan atau issues:

- Buka GitHub Issues
- Hubungi author via repository

**Last Updated**: May 2026 | **Status**: Active Development 🚀
