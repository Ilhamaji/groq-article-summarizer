<div align="center">

# Groq Article Summarizer

Pipeline Python untuk **mengunduh PDF jurnal, mengekstrak teksnya, dan meringkasnya otomatis** dalam Bahasa Indonesia menggunakan Groq API.

[![Status](https://img.shields.io/badge/status-complete-brightgreen?style=flat-square)](#)
[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=flat-square&logo=python&logoColor=white)](https://python.org)
[![Groq](https://img.shields.io/badge/Groq-llama--3.3--70b-F55036?style=flat-square)](https://groq.com)

</div>

---

## Tentang

Modul ini dibuat untuk kebutuhan riset skripsi: mengumpulkan literatur jurnal jadi memakan waktu karena harus membuka satu per satu, mengunduh PDF, lalu membaca abstrak dan kesimpulannya. Pipeline ini mengotomatiskan seluruh alur tersebut — cukup beri URL artikel, dan hasilnya berupa ringkasan terstruktur.

## Alur Kerja

```
URL artikel jurnal
      │
      ▼
[1] Parsing HTML ──► deteksi tautan PDF (pola OJS)
      │
      ▼
[2] Unduh PDF
      │
      ▼
[3] Ekstraksi teks (PyMuPDF)
      │
      ▼
[4] Pemisahan isi vs daftar pustaka (Regex)
      │
      ▼
[5] Ringkasan via Groq (llama-3.3-70b-versatile)
      │
      ▼
Ringkasan Bahasa Indonesia
```

## Fitur

- **Deteksi & unduh PDF otomatis** — mem-parsing HTML untuk menemukan tautan unduhan, mendukung pola **Open Journal Systems (OJS)** (mengubah `/article/view/` menjadi `/article/download/`).
- **Ekstraksi teks cerdas** — PyMuPDF mengekstrak teks lalu memisahkan naskah utama dari bagian **daftar pustaka** menggunakan regex, sehingga ringkasan tidak tercemar sitasi.
- **Ringkasan mendalam** — memakai `llama-3.3-70b-versatile` lewat Groq untuk hasil Bahasa Indonesia baku.
- **Modul evaluasi** — mengukur **latensi**, **kepatuhan format** (regex parsing rate), dan **kemiripan judul/penulis** menggunakan `SequenceMatcher` (berguna untuk bab pengujian skripsi).

## Tech Stack

| Bagian | Teknologi |
| --- | --- |
| Bahasa | Python 3.10+ |
| LLM | Groq API — `llama-3.3-70b-versatile` |
| Ekstraksi PDF | PyMuPDF (`fitz`) |
| Parsing HTML | BeautifulSoup4 |
| Pemisahan teks | Regular Expression |
| Evaluasi | `difflib.SequenceMatcher` |

## Instalasi

```bash
git clone https://github.com/Ilhamaji/groq-article-summarizer.git
cd groq-article-summarizer
pip install -r requirements.txt
```

## Konfigurasi

Dapatkan API key gratis di [console.groq.com](https://console.groq.com), lalu:

```bash
export GROQ_API_KEY="gsk_xxxxx"
```

## Cara Pakai

```python
from summarizer import ringkas_dari_url

hasil = ringkas_dari_url("https://jurnal.contoh.ac.id/article/view/123")
print(hasil)
```

Atau jalankan langsung sebagai script:

```bash
python summarizer.py
```

## Struktur

```
summarizer.py       # seluruh pipeline (deteksi PDF -> ekstraksi -> ringkasan -> evaluasi)
requirements.txt    # dependensi
```

## Catatan

Awalnya dikembangkan di Google Colab untuk keperluan skripsi. Versi lama dari modul ini diarsipkan di [`modul-ringkasan-legacy`](https://github.com/Ilhamaji/modul-ringkasan-legacy).

---

<div align="center">
Dibuat oleh <a href="https://github.com/Ilhamaji">Ilham Aji</a>
</div>
