# RCMS Intelligence — Refinery Contract AI Chatbot

Chatbot AI berbasis query untuk sistem manajemen kontrak kilang minyak.
Terhubung ke PostgreSQL Railway, didukung Claude AI (Anthropic).

---

## 🏗️ Struktur Project

```
chatbot/
├── backend/
│   ├── main.py              ← FastAPI app utama
│   ├── requirements.txt     ← dependencies Python
│   └── .env.example         ← template environment variables
└── frontend/
    └── index.html           ← UI chatbot (single file, tidak perlu build)
```

---

## ⚙️ Setup Backend

### 1. Install Dependencies

```bash
cd backend
pip install -r requirements.txt
```

### 2. Konfigurasi Environment

```bash
cp .env.example .env
```

Edit `.env`:

```env
DATABASE_URL=postgresql://USER:PASSWORD@HOST:PORT/DATABASE
ANTHROPIC_API_KEY=sk-ant-xxxxx
```

- `DATABASE_URL` → connection string PostgreSQL Railway kamu
- `ANTHROPIC_API_KEY` → dari https://console.anthropic.com

### 3. Jalankan Backend

```bash
# Development
python main.py

# Atau dengan uvicorn langsung
uvicorn main:app --reload --host 0.0.0.0 --port 8000
```

Backend akan berjalan di: `http://localhost:8000`
API docs tersedia di: `http://localhost:8000/docs`

---

## 🎨 Setup Frontend

Frontend adalah **single HTML file** — tidak perlu build/install apapun.

### Opsi 1: Buka langsung di browser
```bash
open frontend/index.html
# atau double-click file index.html
```

### Opsi 2: Serve dengan Python (recommended)
```bash
cd frontend
python -m http.server 3000
# Buka http://localhost:3000
```

### Konfigurasi API URL

Klik tombol **"Config"** di kanan atas → masukkan URL backend:
```
http://localhost:8000
```

Atau edit langsung di HTML:
```javascript
let API_URL = 'http://localhost:8000';
```

---

## 🚀 Deploy

### Backend (Railway / Render / VPS)

```bash
# Procfile untuk Railway/Render:
web: uvicorn main:app --host 0.0.0.0 --port $PORT
```

Set environment variables:
- `DATABASE_URL`
- `ANTHROPIC_API_KEY`

### Frontend

Upload `index.html` ke static hosting manapun (Netlify, Vercel, nginx, dll).
Update API_URL di file HTML sesuai URL backend yang sudah deploy.

---

## 🔐 Keamanan

- ✅ Hanya query SELECT yang diizinkan
- ✅ SELECT * diblokir — harus specify kolom
- ✅ UPDATE, DELETE, INSERT, DROP diblokir
- ✅ Semua query otomatis dibatasi LIMIT 1000
- ✅ Input divalidasi sebelum dieksekusi

---

## 💡 Fitur

| Fitur | Keterangan |
|-------|-----------|
| Natural Language | Tanya dalam bahasa Indonesia sehari-hari |
| Auto Table | Hasil >3 baris otomatis jadi tabel interaktif |
| Auto Chart | Deteksi otomatis bar/line/pie/doughnut chart |
| Narasi | Hasil sederhana dijawab dalam bentuk kalimat |
| Download Excel | Dataset >20 baris dapat diunduh |
| SQL Viewer | Bisa lihat query SQL yang dihasilkan |
| Klarifikasi | AI minta konfirmasi jika pertanyaan ambigu |
| Riwayat Chat | Konteks percakapan disimpan per sesi |
| Quick Questions | 10 template pertanyaan siap pakai |

---

## 🗄️ Database

Project ini terhubung ke PostgreSQL dengan tabel:
- `profiles`, `vendor`, `kontrak`, `amandemen_kontrak`
- `tagihan`, `progress_lumpsum`, `progress_unit_price`
- `monitoring_ltsa`, `padi`, `dokumen_approval`, `konfigurasi_sistem`

---

## ❓ Contoh Pertanyaan

```
"Berapa total nilai kontrak yang sedang aktif?"
"Vendor mana yang memiliki skor tertinggi?"
"Tampilkan 10 tagihan terbesar yang belum dibayar"
"Distribusi tipe kontrak dalam bentuk chart pie"
"Kontrak apa saja yang KOM-nya terlambat bulan ini?"
"Tren progress actual vs plan per vendor — tampilkan chart"
"Daftar pembelian PADI yang nilai-nya di atas 100 juta"
```
"# chatbotkontrak" 
