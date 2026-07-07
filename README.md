# Master Data — Project ESCAPE

Hybrid **Obby Escape × Escape Room** untuk Roblox. Versi **1.1** (tanpa monetisasi, tanpa PvP, tanpa trading).

Folder ini berisi metadata master data. Dokumen naratif lengkapnya ada di Google Docs (tautan di bawah), dan seluruh datanya juga tersedia terstruktur di `metadata.json`.

## Dokumen

| # | Dokumen | Versi | Tautan |
|---|---------|-------|--------|
| 1 | PRD — Product Requirement Document | 1.1 | https://docs.google.com/document/d/1oHf-7cF-lBWX90n2EqiwMYKtuNxHo7uK-OCvVz9FEGg/edit |
| 2 | SRD/TDD — System & Technical | 1.1 | https://docs.google.com/document/d/1N5yZ5rnHK-5Oqmpa_OcPelYNqqimoElvsuAsWvCVnyo/edit |
| 3 | GDD + Golden Rules | 1.1 | https://docs.google.com/document/d/1_bxmlEJNxo3G8wUZLyXvwKkjQmOSndstHjyri3v90l8/edit |
| 4 | Map Template + Obstacle Catalog | 1.0 | https://docs.google.com/document/d/1arB6CLPR7xFpCTvYRHVLLGzPq-T9F8Q3e-bXRGxQbJo/edit |

## Ringkasan Cepat

- **Signature obstacle:** *The Sentinel Gate* (SG-00) — wajib 1 per map, 3 fase (Aktivasi Rune → Baca Petunjuk → Buka Gerbang), mekanik identik di semua map, hanya di-reskin.
- **Aturan inti:** minimal 10 obstacle per map, komposisi ≥40% obby & ≥30% puzzle, checkpoint sebelum tiap lonjakan sulit, semua keputusan penting dihitung server-side.
- **21 Golden Rules** (GR-1…GR-21) dikelompokkan: Struktur & Konten, Keadilan, Keterbacaan, Teknis & Keamanan, Identitas & Tema.
- **Katalog obstacle:** 8 obby (OB-01…OB-08) + 4 puzzle (PZ-01…PZ-04) + 1 signature (SG-00).
- **Template map:** 5 zona (Z1 Pembuka → Z2 Aksi Awal → Z3 Ruang Puzzle → Z4 Gauntlet → Z5 Sentinel Gate).

## Isi Folder

- `metadata.json` — master data terstruktur (dokumen, scope, golden rules, katalog obstacle, template, preset kesulitan, tema).
- `README.md` — indeks ini.

## Di Luar Lingkup (v1.1)

Tanpa monetisasi (shop/gamepass/dev product), tanpa PvP/kombat, tanpa trading antar pemain, tanpa UGC map editor, tanpa voice chat.
