<img width="1410" height="664" alt="image" src="https://github.com/user-attachments/assets/7fac136c-8551-4259-be71-b47b74abafc8" />


# 🔍 LogScan — Web Server Log Analyzer

![Version](https://img.shields.io/badge/version-1.0.0-00f5a0?style=flat-square)
![Format](https://img.shields.io/badge/format-HTML%20%2F%20Vanilla%20JS-00d4ff?style=flat-square)
![OWASP](https://img.shields.io/badge/OWASP-Top%2010%202021-ff4757?style=flat-square)
![License](https://img.shields.io/badge/license-MIT-a29bfe?style=flat-square)

Dashboard analisa log web server berbasis browser — **tidak perlu instalasi, tidak perlu server, tidak perlu koneksi internet**. Cukup buka file HTML dan upload log kamu.

---

## 📋 Deskripsi

LogScan adalah tools analisa log web server yang berjalan sepenuhnya di browser (client-side). Didesain untuk developer dan sysadmin yang ingin dengan cepat menganalisa log Apache/Nginx, memfilter request berdasarkan berbagai kriteria, serta mendeteksi pola serangan berdasarkan **OWASP Top 10 2021** tanpa perlu menginstall tools tambahan atau mengirim data ke server manapun.

---

## ✨ Fitur Utama

### 📂 Multi-Format Support dengan Auto-Detect
Upload satu file, format otomatis terdeteksi. Mendukung 5 format sekaligus:

| Format | Contoh Baris |
|---|---|
| **Apache Access Log** | `127.0.0.1 - - [10/Jun/2025:06:08:16 +0700] "GET /index.php HTTP/1.1" 200 512 "-" "Mozilla/5.0"` |
| **Apache Error Log** | `[Tue Jun 10 06:08:16.944880 2025] [php:error] [pid 1344:tid 404] [client 127.0.0.1:7286] PHP Fatal error: ...` |
| **Nginx Access Log** | `127.0.0.1 - - [10/Jun/2025:06:08:16 +0700] "POST /login HTTP/1.1" 200 128 "-" "curl/7.88"` |
| **Nginx Error Log** | `2025/06/10 06:08:16 [error] 1234#1234: *1 connect() failed...` |
| **Custom Format** | Format log PHP kustom dengan blok `---` separator |

> Format XAMPP (Apache dengan `pid:tid`) juga didukung penuh.

---

### 🔍 OWASP Top 10 Threat Detection

Deteksi dilakukan **hanya pada body request** (URL, GET params, POST params, Body) — bukan pada headers — untuk meminimalisir false positive.

| ID | Nama | Severity | Scope Scan |
|---|---|---|---|
| A03:2021 | SQL Injection | 🔴 Critical | URL + Body |
| A03:2021 | Cross-Site Scripting (XSS) | 🔴 Critical | URL + Body |
| A01:2021 | Path / Directory Traversal | 🔴 Critical | URL + Body |
| A03:2021 | Command Injection | 🔴 Critical | URL + Body |
| A05:2021 | XXE Injection | 🔴 Critical | URL + Body |
| A10:2021 | Server-Side Request Forgery (SSRF) | 🔴 Critical | URL + Body |
| A03:2021 | Server-Side Template Injection (SSTI) | 🔴 Critical | URL + Body |
| A03:2021 | NoSQL Injection | 🔴 Critical | URL + Body |
| A06:2021 | Security Scanner / Recon | 🟡 Medium | User-Agent only |
| A05:2021 | Sensitive File Access | 🟡 Medium | URL only |
| A07:2021 | Brute Force / Auth Endpoint | 🟡 Medium | URL + POST combo |

---

### 🎛️ Filtering & Pencarian

- **Filter Method**: ALL / GET / POST / PUT / DELETE
- **Filter Status**: ALL / 2xx / 3xx / 4xx / 5xx
- **Filter Threat**: ALL / Critical / Medium
- **Search Real-time**: cari berdasarkan URL, IP, User-Agent, atau konten body
- Filter **otomatis berubah** untuk error log (menjadi Error/Crit, Warn/Notice, Info/Debug)

---

### 📊 Dashboard Stats

Menampilkan ringkasan setelah file diupload:
- Total request / entri
- Breakdown per method (GET, POST) atau per level error
- Jumlah response 2xx vs 4xx/5xx
- Total ancaman terdeteksi
- Badge format yang terdeteksi

---

## 🚀 Cara Penggunaan

### 1. Buka File
```
Cukup buka file index.html di browser manapun
(Chrome, Firefox, Edge, Safari)
```

### 2. Upload Log
Klik area upload atau tombol **"Pilih File .log"**, lalu pilih file log kamu. Bisa juga dengan **drag & drop** langsung ke area upload.

### 3. Analisa
- Dashboard langsung menampilkan stats dan semua entri
- Badge format terdeteksi muncul di header
- Klik entri untuk expand detail lengkap
- Gunakan filter dan search untuk menyaring data

### 4. Investigasi Ancaman
Entri yang terdeteksi ancaman ditandai dengan border berwarna:
- 🔴 Border merah = ancaman **Critical**
- 🟡 Border kuning = ancaman **Medium**

Klik entri untuk melihat detail ancaman: nama OWASP, deskripsi, dan string yang di-match.

---

## 📁 Struktur File

```
logscan/
└── log-analyzer.html    ← satu file, semua sudah di dalamnya
```

Tidak ada dependency eksternal selain Google Fonts (opsional, hanya untuk tampilan).

---

## 🖥️ Kompatibilitas

| Browser | Status |
|---|---|
| Chrome 90+ | ✅ Full support |
| Firefox 88+ | ✅ Full support |
| Edge 90+ | ✅ Full support |
| Safari 14+ | ✅ Full support |
| IE 11 | ❌ Tidak didukung |

**OS**: Windows, Linux, macOS — tidak ada perbedaan karena berbasis browser.

---

## ✅ Keunggulan

### 🔒 Privacy — Data Tidak Keluar dari Browser
Semua proses parsing dan analisa terjadi sepenuhnya di browser (client-side JavaScript). File log tidak pernah dikirim ke server manapun. Cocok untuk log yang bersifat sensitif atau internal.

### ⚡ Zero Install
Tidak perlu `npm install`, tidak perlu Python, tidak perlu Docker. Buka file HTML di browser, langsung jalan.

### 🌐 Offline Ready
Setelah file didownload, tidak butuh koneksi internet sama sekali untuk menganalisa log. (Hanya Google Fonts yang membutuhkan internet, itupun hanya untuk tampilan font saja.)

### 🎯 Minim False Positive
Deteksi OWASP dirancang dengan scope yang ketat — headers HTTP tidak discan karena banyak mengandung karakter seperti `;`, `*`, `/` yang bisa memicu false positive. Setiap pattern punya scope spesifik (body only, URL only, UA only, atau kombinasi).

### 📋 Multi-Format dalam Satu Tool
Lima format log berbeda ditangani oleh satu file yang sama, dengan parser dan UI yang menyesuaikan otomatis.

### 🔍 Detail Lengkap Per Entri
Setiap entri bisa di-expand untuk melihat semua field: IP, UA, headers, payload, performance (untuk custom format), raw message error, dan breakdown ancaman yang detail.

---

## ⚠️ Keterbatasan

### 📦 Performa pada File Besar
Karena semua diproses di memori browser dengan vanilla JavaScript, file log yang sangat besar (lebih dari ~50MB atau lebih dari ~500.000 baris) bisa memperlambat browser atau menyebabkan tab tidak responsif. Disarankan memotong log terlebih dahulu untuk file berukuran besar.

### 🔎 Regex-Based Detection (Bukan AI/ML)
Deteksi ancaman menggunakan regex statis, bukan analisa perilaku atau machine learning. Artinya:
- Serangan yang sangat ter-obfuscate mungkin tidak terdeteksi
- Pola baru yang belum ada di database tidak akan terdeteksi
- Bukan pengganti WAF (Web Application Firewall) atau SIEM yang proper

### 📄 Tidak Ada Export
Saat ini tidak ada fitur export hasil analisa ke PDF, CSV, atau format lainnya. Hasil hanya bisa dilihat di browser.

### 🔄 Tidak Ada Parsing Real-time
Log harus diupload secara manual — tidak ada fitur tail/watch untuk memantau log secara live seperti `tail -f`.

### 🌐 Format Non-Standard
Jika web server menggunakan format log kustom yang sangat berbeda dari Combined Log Format standar, parser mungkin tidak dapat membacanya dengan benar dan perlu modifikasi regex manual di source code.

### 🔐 Deteksi Terbatas untuk Error Log
Untuk Apache/Nginx Error Log, deteksi OWASP kurang efektif karena error log umumnya tidak mengandung payload request secara langsung. Lebih berguna untuk analisa error dan debugging daripada deteksi serangan.

---

## 🛠️ Pengembangan Lanjutan (Roadmap Ideas)

Beberapa fitur yang bisa ditambahkan di versi mendatang:

- [ ] Export hasil ke CSV / PDF
- [ ] Pagination untuk file log besar
- [ ] Chart / visualisasi distribusi request per jam
- [ ] Web Worker untuk parsing background (tidak membekukan UI)
- [ ] Dukungan file `.gz` (log terkompresi)
- [ ] Custom regex pattern oleh user
- [ ] Simpan hasil analisa ke localStorage

---

## 📝 Catatan Teknis

### Mengapa PHP tidak digunakan?
Dashboard ini diputuskan menggunakan pure HTML + JavaScript karena:
- Parsing log dilakukan di client-side sehingga tidak perlu server
- File log tidak perlu dikirim ke manapun (privacy)
- Lebih portable — cukup satu file `.html`

PHP akan relevan jika ada kebutuhan seperti parsing di server, penyimpanan hasil ke database, atau notifikasi email otomatis saat ancaman terdeteksi.

### Arsitektur Deteksi OWASP
```
File Log Diupload
       ↓
  detectFormat()        ← identifikasi format dari 2000 karakter pertama
       ↓
  parser spesifik()     ← parseAccess / parseApacheError / parseNginxError / parseCustom
       ↓
  finalizeEntry()
       ↓
  detectThreats()       ← scan HANYA: URL + GET + POST + Body (bukan headers)
       ↓
  renderLogs()          ← tampilkan di UI dengan filter aktif
```

---

## 📄 Lisensi

MIT License — bebas digunakan, dimodifikasi, dan didistribusikan.

---

> Dibuat untuk kebutuhan analisa log lokal yang cepat, aman, dan tanpa ketergantungan eksternal.
