<div align="center">

# Groq Article Summarizer

A Python pipeline that **downloads a journal PDF, extracts its text, and summarises it automatically** using the Groq API.

[![Status](https://img.shields.io/badge/status-complete-brightgreen?style=flat-square)](#)
[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB?style=flat-square&logo=python&logoColor=white)](https://python.org)
[![Groq](https://img.shields.io/badge/Groq-llama--3.3--70b-F55036?style=flat-square)](https://groq.com)

**English** · [Bahasa Indonesia](README.id.md)

</div>

---

## About

Built for thesis research: gathering journal literature is slow because each source has to be opened, the PDF downloaded, and the abstract and conclusion read by hand. This pipeline automates the whole path — give it an article URL and it returns a structured summary.

## Pipeline

```
Journal article URL
      │
      ▼
[1] Parse HTML ──► find the PDF link (OJS pattern)
      │
      ▼
[2] Download the PDF
      │
      ▼
[3] Extract text (PyMuPDF)
      │
      ▼
[4] Split body from bibliography (regex)
      │
      ▼
[5] Summarise via Groq (llama-3.3-70b-versatile)
      │
      ▼
Structured summary
```

## Features

- **Automatic PDF discovery and download** — parses the article HTML to find the download link and understands the **Open Journal Systems (OJS)** pattern (rewriting `/article/view/` to `/article/download/`).
- **Clean text extraction** — PyMuPDF pulls the text out, then regex separates the body from the **bibliography**, so citations do not contaminate the summary.
- **LLM summarisation** — `llama-3.3-70b-versatile` via Groq produces a structured summary.
- **Evaluation module** — measures **latency**, **format compliance** (regex parsing rate), and **title/author similarity** with `SequenceMatcher`. This is what backs the thesis evaluation chapter.

## Tech Stack

| Layer | Technology |
| --- | --- |
| Language | Python 3.10+ |
| LLM | Groq API — `llama-3.3-70b-versatile` |
| PDF extraction | PyMuPDF (`fitz`) |
| HTML parsing | BeautifulSoup4 |
| Text splitting | Regular expressions |
| Evaluation | `difflib.SequenceMatcher` |

## Installation

```bash
git clone https://github.com/Ilhamaji/groq-article-summarizer.git
cd groq-article-summarizer
pip install -r requirements.txt
```

## Configuration

Get a free API key from [console.groq.com](https://console.groq.com), then:

```bash
export GROQ_API_KEY="gsk_xxxxx"
```

## Usage

```python
from summarizer import summarize_from_url

result = summarize_from_url("https://journal.example.ac.id/article/view/123")
print(result["result"])
```

The function raises `ValueError` when the PDF cannot be found (paywalled article) or when no text could be extracted.

Run the script directly to execute the evaluation harness:

```bash
python summarizer.py
```

That entrypoint calls `evaluate_modul(...)` against a sample URL and prints the latency and similarity metrics — edit `sample_url` inside the `__main__` block to test a different article.

## Project Layout

`summarizer.py` contains the whole pipeline:

| Function | Role |
| --- | --- |
| `_find_pdf_url` | Locate the PDF link in the article HTML |
| `_extract_text_from_pdf` | Download the PDF and split body from bibliography |
| `summarize` | Send the text to Groq and parse the response |
| `summarize_from_url` | Public entrypoint used by the FastAPI router |
| `evaluate_modul` | Latency and similarity metrics for the thesis evaluation |

## Notes

Originally developed in Google Colab for thesis work. The older copy of this module is archived at [`modul-ringkasan-legacy`](https://github.com/Ilhamaji/modul-ringkasan-legacy).

---

<div align="center">
Built by <a href="https://github.com/Ilhamaji">Ilham Aji</a>
</div>
