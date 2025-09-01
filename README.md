AI Week — Deep Learning Hands-on

Dokumentasi singkat untuk materi praktikum Deep Learning pada repository ini. Proyek berisi dua komponen utama:

1. Chatbot Overview with LangChain
2. Spotify Songs Recommender with FAISS vector database

Tujuan: memberikan contoh implementasi end-to-end untuk aplikasi berbasis embedding dan retrieval serta rekomendasi lagu menggunakan vector search.

## Daftar Isi
- Ringkasan proyek
- Persyaratan & environment
- 1) Chatbot (LangChain)
- 2) Spotify Songs Recommender (FAISS)
- Cara menjalankan (quickstart)
- Struktur folder
- Kontribusi & Lisensi

## Ringkasan proyek
Proyek ini berfokus pada dua studi kasus praktis:

- Chatbot: sistem tanya-jawab berbasis LangChain yang menggunakan model embedding + retriever untuk menjawab pertanyaan dari dokumen/konten terkait. Cocok sebagai contoh pembuatan assistant domain-spesifik.
- Spotify Songs Recommender: sistem rekomendasi lagu sederhana yang menyimpan representasi vektor lagu (mis. embedding fitur audio atau metadata) di FAISS dan melakukan nearest-neighbor search untuk memberikan rekomendasi.

Kedua modul menunjukkan pipeline umum: preprocessing -> embedding -> penyimpanan vector -> retrieval -> (opsional) reranking atau filtering.

## Persyaratan & Environment
- OS: Windows (direkomendasikan WSL2 / Python 3.8+ virtualenv) atau Linux/Mac
- Python 3.8+
- pip, virtualenv / venv

Direkomendasikan membuat virtual environment dan menginstall dependensi yang relevan untuk masing-masing bagian. Contoh paket yang biasa digunakan:

- langchain
- openai (atau SDK model LLM lain yang dipakai)
- faiss-cpu (atau faiss-gpu jika tersedia)
- sentence-transformers (untuk embedding)
- numpy, pandas

Catatan: file requirements lengkap bisa ditambahkan oleh kontributor bila ada implementasi kode di repo.

## 1) Chatbot Overview with LangChain

Tujuan
- Membangun chatbot domain-spesifik yang menjawab pertanyaan dengan basis dokumen.

Arsitektur singkat
- Data source: dokumen teks, FAQ, atau corpus domain.
- Preprocessing: pembersihan, chunking (memecah dokumen menjadi potongan lebih kecil).
- Embedding: ubah setiap chunk menjadi vektor embedding (menggunakan model embedding seperti sentence-transformers atau embedding provider seperti OpenAI).
- Vector DB / Retriever: simpan embedding ke penyimpanan (FAISS, Milvus, Pinecone, dll.) untuk query nearest-neighbor.
- LangChain: komponenisasi pipeline prompt -> retriever -> LLM untuk menghasilkan jawaban yang konteks-sensitif.

Contoh alur quickstart (pseudo):

1. Siapkan dokumen dan lakukan chunking.
2. Hit model embedding untuk setiap chunk dan simpan ke FAISS/local index.
3. Saat user bertanya, embed pertanyaan, lakukan nearest-neighbor search untuk ambil konteks relevan.
4. Konsolidasikan konteks dan kirim ke LLM via LangChain untuk generasi jawaban.

Catatan implementasi
- Jaga ukuran konteks (token) saat menyusun prompt.
- Pertimbangkan reranking hasil retrieval untuk kualitas.

## 2) Spotify Songs Recommender with FAISS

Tujuan
- Menyediakan recommender sederhana yang mengambil lagu-lagu serupa berdasarkan embedding vektor (bisa dari fitur audio atau metadata + lyric embeddings).

Arsitektur singkat
- Data source: dataset lagu (Spotify metadata, audio features, lyric embeddings, dsb).
- Fitur: ekstraksi fitur lagu (mis. audio features dari Spotify API, atau embedding lyric/metadata dengan model tekstual).
- Indexing: simpan vektor ke FAISS index (contoh: IndexFlatL2 untuk baseline).
- Query: hitung embedding untuk lagu target atau preferensi user, lakukan k-NN di FAISS, kembalikan rekomendasi teratas.

Contoh alur quickstart (pseudo):

1. Kumpulkan dataset lagu (CSV/JSON) yang berisi id, judul, artis, dan vektor fitur/embedding.
2. Load dataset lalu build FAISS index: add(vectors).
3. Untuk rekomendasi: embed lagu query / user profile -> faiss.search(queries, k) -> ambil id lagu dan tampilkan metadata.

Tips operasi
- Gunakan normalisasi vektor si jika menggunakan cosine similarity (FAISS dapat memakai inner product pada vektor ter-normalisasi).
- Untuk dataset besar, pilih index FAISS yang sesuai (IVF, HNSW, dll.) dan lakukan training bila perlu.

## Quickstart — contoh langkah singkat
Berikut langkah umum yang harus dilakukan sebelum menjalankan modul (sesuaikan dengan skrip/implementasi Anda):

1. Buat virtualenv dan aktifkan:

```powershell
python -m venv .venv; .\.venv\Scripts\Activate.ps1
pip install -U pip
```

2. Install dependensi dasar (contoh):

```powershell
pip install langchain sentence-transformers faiss-cpu openai numpy pandas
```

3. Siapkan kredensial API (mis. OPENAI_API_KEY) sebagai environment variable jika menggunakan provider eksternal.

4. Jalankan script preprocessing / indexing sesuai folder implementasi (mis. scripts/build_faiss.py atau scripts/build_chatbot_index.py).

5. Jalankan demo atau app server untuk mencoba chatbot dan recommender.

## Struktur (disarankan)
- data/                -> dataset mentah (CSV/JSON)
- notebooks/           -> notebook eksperimen
- scripts/             -> skrip preprocessing, indexing, util
- app/                 -> aplikasi sederhana (API / demo)
- README.md            -> dokumentasi (file ini)

Jika Anda belum menambahkan file kode, gunakan struktur di atas sebagai panduan untuk menaruh implementasi nanti.

## Kontribusi
- Fork repo ini, buat branch untuk fitur, dan ajukan pull request.
- Sertakan deskripsi eksperimen, dependensi di requirements.txt, dan contoh cara menjalankan.

## Lisensi
Proyek ini bisa dilisensikan sesuai kebutuhan (mis. MIT). Jika tidak ditentukan, harap tanyakan pemilik repo.

---

Catatan: README ini adalah template / dokumentasi tingkat tinggi untuk proyek praktikum. Jika Anda mau, saya dapat menambahkan contoh skrip `requirements.txt`, notebook demonstrasi, atau contoh implementasi cepat (minimal runnable) untuk salah satu modul — sebutkan mana yang diinginkan dan saya akan buatkan.
