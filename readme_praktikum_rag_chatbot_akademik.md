# Praktikum AI: Chatbot Panduan Akademik Berbasis RAG (*Retrieval-Augmented Generation*)

Repositori/modul ini berisi materi pengantar dan praktikum berbasis **Google Colab** mengenai penggabungan **Foundation Model (LLM)** dengan **Pengetahuan Eksternal** menggunakan arsitektur ***Retrieval-Augmented Generation* (RAG)**. 

Studi kasus yang diangkat adalah pembangunan **Chatbot Layanan Panduan Akademik Kampus** yang mampu menjawab pertanyaan mahasiswa secara akurat berdasarkan dokumen aturan kampus, lengkap dengan rujukan Bab dan perlindungan anti-halusinasi.

---

## 🎯 Tujuan Pembelajaran

Setelah mengikuti modul praktikum ini, peserta diharapkan mampu:
1. Memahami perbedaan antara penggunaan *Foundation Model* murni dan pendekatan *Retrieval-Augmented Generation* (RAG).
2. Memahami alur kerja dasar RAG: **Chunking $\rightarrow$ Embedding & Vector Database $\rightarrow$ Retrieval $\rightarrow$ Generation**.
3. Membangun *pipeline* pencarian semantik Bahasa Indonesia menggunakan `Sentence-Transformers` dan `FAISS`.
4. Mengintegrasikan hasil pencarian dokumen ke dalam *Foundation Model* dengan teknik *grounding* agar jawaban tetap faktual dan tidak mengarang (*hallucination*).
5. Mendemonstrasikan chatbot secara interaktif menggunakan **Google Colab Form**.

---

## 💡 Konsep Dasar (Ringkasan Materi)

### Apa itu *Foundation Model* (FM)?
***Foundation Model*** adalah model AI berukuran besar yang dilatih dengan miliaran data teks dari internet (contohnya **Gemini, GPT, Qwen, dan LLaMA**). Model ini sangat mahir berbahasa manusia, tetapi memiliki keterbatasan saat dipakai untuk sistem informasi spesifik:
* **Tidak mengetahui data internal/privat** (seperti Buku Pedoman Akademik kampus).
* **Pengetahuan tidak selalu *update*** jika aturan baru saja diubah.
* **Rentan halusinasi**, yaitu mengarang jawaban yang terdengar meyakinkan padahal keliru secara faktual.

### Apa itu RAG (*Retrieval-Augmented Generation*)?
**RAG** menggabungkan kecerdasan bahasa *Foundation Model* dengan dokumen eksternal milik kita sendiri.
* **LLM Murni (Tanpa RAG)** ibarat mahasiswa yang mengikuti **ujian *Closed-Book*** (hanya mengandalkan hafalan).
* **Chatbot RAG** ibarat mahasiswa yang mengikuti **ujian *Open-Book***: mencari pasal yang relevan di buku panduan terlebih dahulu, lalu merangkai jawaban berdasarkan kutipan tersebut.

### Alur Kerja Sistem
```text
[Dokumen Panduan] ---> (1) Dipecah per Aturan (Chunking) ---> (2) Diubah jadi Vektor (FAISS)
                                                                        |
[Pertanyaan Mahasiswa] ----------------------------------> (3) Cari Aturan Mirip (Retrieval)
                                                                        |
                                                           (4) Foundation Model Menjawab (Generation)
```

---

## 📂 Struktur Praktikum (`Praktikum_RAG_Chatbot_Panduan_Akademik-v2.ipynb`)

Notebook dirancang **100% mandiri (*self-contained*)** dan disusun dalam **8 langkah bertahap**:

| Langkah | Topik | Penjelasan Singkat |
| :--- | :--- | :--- |
| **Langkah 1** | Instalasi Library | Memasang `sentence-transformers`, `faiss-cpu`, `transformers`, `accelerate`, dan `google-genai`. |
| **Langkah 2** | Data & *Chunking* | Membuat simulasi dokumen **Panduan Akademik (Bab I – Bab V)** dan memecahnya per paragraf aturan. |
| **Langkah 3** | *Embedding* & FAISS | Mengonversi teks aturan Bahasa Indonesia menjadi vektor angka dan menyimpannya ke database pencarian **FAISS**. |
| **Langkah 4** | Modul *Retriever* | Membuat fungsi `cari_aturan_relevan()` dan menguji pencarian makna (parafrase sehari-hari). |
| **Langkah 5** | *Foundation Model* | Memuat otak pembuat jawaban (otomatis memakai **GPU T4 gratis** di Colab, *fallback* ke CPU, atau opsi **Gemini API**). |
| **Langkah 6** | *Pipeline* Chatbot RAG | Menggabungkan pencari referensi dan *Foundation Model* ke dalam fungsi `tanya_chatbot()` dengan aturan anti-halusinasi. |
| **Langkah 7** | Uji Coba Otomatis | Menguji chatbot pada 3 skenario: pertanyaan faktual, bahasa santai (parafrase), dan pertanyaan di luar dokumen (*out-of-scope*). |
| **Langkah 8** | **Live Demo Form** | Antarmuka formulir interaktif di Google Colab untuk sesi tanya-jawab langsung tanpa perlu mengedit kode. |

---

## 🚀 Cara Menjalankan & Memaparkan di Google Colab

1. **Buka Google Colab:** Kunjungi [colab.research.google.com](https://colab.research.google.com/), lalu pilih **File $\rightarrow$ Upload notebook** dan unggah file `Praktikum_RAG_Chatbot_Panduan_Akademik-v2.ipynb`.
2. **Aktifkan GPU Gratis (Sangat Disarankan):**
   * Klik menu **Runtime $\rightarrow$ Change runtime type $\rightarrow$ T4 GPU $\rightarrow$ Save**.
   * *Catatan:* Dengan GPU T4, notebook otomatis menggunakan model **`Qwen/Qwen2.5-1.5B-Instruct`** yang fasih berbahasa Indonesia. Jika menggunakan CPU biasa, sistem otomatis beralih ke model ringan cadangan.
3. **Persiapan Sebelum Presentasi:**
   * Jalankan **Langkah 1 sampai Langkah 6** terlebih dahulu hingga muncul pesan `Chatbot RAG Akademik siap dipakai!`.
4. **Saat Sesi Pemaparan / Live Demo:**
   * Tampilkan hasil pengujian pada **Langkah 7**.
   * Gunakan formulir interaktif pada **Langkah 8** untuk mengetikkan pertanyaan langsung dari audiens dan klik tombol **Play (▶)**.

---

## 💬 Contoh Pertanyaan untuk Uji Coba Live Demo

Anda dapat mencoba tiga kategori pertanyaan berikut saat mendemonstrasikan chatbot:

1. **Pertanyaan Faktual Langsung:**
   * *"Jika IPS semester lalu saya 2,80, maksimal berapa SKS yang bisa saya ambil?"*
   * *"Apa saja syarat SKS dan IPK supaya saya bisa ambil Skripsi?"*
   * *"Berapa persen batas maksimal kemiripan plagiarisme untuk tugas akhir?"*
2. **Pertanyaan Bahasa Santai / Parafrase (Uji Pencarian Makna):**
   * *"Boleh nggak saya libur kuliah (cuti) selama 2 semester berturut-turut?"*
   * *"Kalau mau ganti atau batalin mata kuliah setelah kuliah mulai, maksimal berapa minggu?"*
3. **Pertanyaan di Luar Dokumen / *Out-of-Scope* (Uji Anti-Halusinasi):**
   * *"Berapa tarif parkir motor per hari di kampus?"*
   * *"Jam operasional perpustakaan sampai pukul berapa?"*

---

## 🛠️ Teknologi & Pustaka yang Digunakan

* **Python 3** & **Google Colaboratory**
* **[Sentence-Transformers](https://www.sbert.net/):** Model `paraphrase-multilingual-MiniLM-L12-v2` untuk representasi vektor teks multibahasa (mendukung Bahasa Indonesia).
* **[FAISS](https://github.com/facebookresearch/faiss):** Basis data vektor berkecepatan tinggi untuk pencarian kemiripan semantik.
* **[Hugging Face Transformers](https://huggingface.co/docs/transformers):** Memuat *Foundation Model* *open-source* (`Qwen/Qwen2.5-1.5B-Instruct` / `google/flan-t5-base`).
* **[Google Gen AI SDK](https://ai.google.dev/):** Integrasi opsional dengan model `gemini-2.5-flash`.