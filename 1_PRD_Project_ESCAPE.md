# PRD — Product Requirement Document

**Game:** Project ESCAPE — Hybrid Obby Escape × Escape Room
**Versi:** 1.1 · **Owner:** Bagus · **Status:** Draft · **Platform:** Roblox
*Perubahan v1.1: tanpa shop, monetisasi, PvP, dan trading.*

## Daftar Isi

1. Ringkasan Produk
2. Target Pemain
3. Tujuan & Metrik Keberhasilan
4. Ruang Lingkup Fitur
5. Pengalaman Pemain (Player Journey)
6. Rilis & Roadmap Ringkas
7. Risiko & Mitigasi

## 1. Ringkasan Produk

Project ESCAPE adalah game Roblox bergenre escape yang menggabungkan dua format populer: obby escape (lari, lompat, dan timing melewati rintangan menuju pintu keluar) dan escape room (memecahkan puzzle untuk membuka jalan). Pemain menembus serangkaian map bertema, masing-masing berisi minimal 10 rintangan, dan setiap map selalu ditutup oleh satu rintangan penanda (signature obstacle) bernama Sentinel Gate.

**Versi ini dirancang tanpa monetisasi, tanpa PvP, dan tanpa trading antar pemain** — fokus penuh pada pengalaman escape yang murni berbasis skill dan puzzle.

Dokumen ini mendefinisikan apa yang dibangun dan untuk siapa. Detail desain mekanik ada di GDD, dan detail teknis ada di SRD/TDD.

### 1.1 Visi Produk

Menjadi seri map escape modular di Roblox di mana pemain merasakan tegangnya obby berbasis skill sekaligus kepuasan memecahkan puzzle escape room — dengan identitas yang konsisten lewat satu rintangan penanda di setiap map.

### 1.2 Problem & Peluang

- Banyak obby escape mengandalkan gerak refleks saja sehingga cepat membosankan; escape room murni sering terasa lambat. Menggabungkan keduanya menjaga tempo tetap tinggi sekaligus memberi variasi kognitif.
- Map escape di Roblox biasanya dibuat ad-hoc tanpa standar, sehingga kualitas tidak konsisten dan sulit diproduksi cepat. Master data + template menstandardisasi produksi map.
- Signature obstacle menciptakan brand recognition: pemain langsung mengenali map ini bagian dari seri yang sama.

## 2. Target Pemain

| Segmen | Umur | Motivasi Utama | Kebutuhan Desain |
|---|---|---|---|
| Casual obby player | 8–14 | Fun cepat, tantangan ringan-menengah | Checkpoint sering, feedback jelas, tidak frustasi |
| Skill/speedrun player | 12–18 | Menguasai timing, kompetisi waktu | Rintangan presisi, leaderboard waktu |
| Puzzle solver | 10–20 | Kepuasan memecahkan teka-teki | Puzzle logis, petunjuk adil, tidak brute-force |
| Grup/teman | 8–18 | Main bareng, saling bantu | Dukungan multiplayer, puzzle kooperatif opsional |

*Persona inti: pemain 9–15 tahun yang menyukai obby menegangkan tetapi bosan jika hanya melompat. Mereka menikmati momen 'aha' saat memecahkan puzzle untuk membuka gerbang.*

## 3. Tujuan & Metrik Keberhasilan

| Tujuan | Metrik | Target Awal |
|---|---|---|
| Retensi | D1 retention | ≥ 30% |
| Engagement | Rata-rata sesi | ≥ 8 menit |
| Progresi | % pemain menembus Map 1 | ≥ 60% |
| Kepuasan puzzle | % pemain menyelesaikan Sentinel Gate tanpa keluar | ≥ 45% |
| Sosial | % sesi dengan >1 pemain | ≥ 25% |

Metrik di atas adalah baseline untuk soft launch dan wajib ditinjau ulang setelah data nyata masuk.

## 4. Ruang Lingkup Fitur

### 4.1 Fitur Inti (MVP)

| Fitur | Deskripsi | Prioritas |
|---|---|---|
| Sistem Map | Minimal 3 map bertema saat rilis, tiap map ≥10 obstacle | P0 |
| Signature Obstacle | Sentinel Gate di akhir setiap map (obby + puzzle) | P0 |
| Checkpoint & Respawn | Checkpoint per segmen, respawn instan | P0 |
| Sistem Progres | Menyimpan map/checkpoint terakhir per pemain | P0 |
| Puzzle Engine | Modul puzzle reusable (kode, tuas, pelat tekanan) | P0 |
| Timer & Leaderboard | Waktu tembus map, papan peringkat | P1 |

### 4.2 Di Luar Lingkup (Non-Goals) v1.1

- **Monetisasi apa pun**: tanpa shop/toko in-game, tanpa gamepass, tanpa dev product atau pembelian Robux.
- **PvP / kombat**: tanpa pertarungan antar pemain maupun sistem senjata.
- **Trading antar pemain**: tanpa tukar-menukar item, kosmetik, atau progres.
- Editor map buatan pemain (UGC map).
- Voice chat / fitur sosial kompleks.

*Catatan: kosmetik (jika ada) hanya diperoleh gratis sebagai reward menyelesaikan map, bukan dibeli atau ditukar.*

## 5. Pengalaman Pemain (Player Journey)

| Tahap | Yang Terjadi | Emosi Target |
|---|---|---|
| Masuk (Lobby) | Spawn, lihat pintu map, tutorial singkat | Penasaran |
| Obby Segment | Lewati rintangan gerak & timing | Tegang, fokus |
| Puzzle Room | Pecahkan teka-teki untuk buka jalan | Berpikir, 'aha' |
| Sentinel Gate | Gabungan obby + puzzle sebagai klimaks map | Puncak tegang |
| Selesai Map | Reward gratis, waktu tercatat, buka map berikutnya | Bangga, ingin lanjut |

## 6. Rilis & Roadmap Ringkas

| Fase | Isi | Kriteria Keluar |
|---|---|---|
| Alpha | 1 map lengkap + Sentinel Gate + progres | Loop dasar bisa dimainkan tanpa bug fatal |
| Beta (soft launch) | 3 map, leaderboard | Metrik baseline tercapai di grup uji |
| Rilis 1.0 | Polish, marketing, stabilitas | Stabil, crash rate < 1% |
| Post-launch | Map baru rutin memakai template + Sentinel Gate | Cadence rilis map konsisten |

## 7. Risiko & Mitigasi

| Risiko | Dampak | Mitigasi |
|---|---|---|
| Puzzle terlalu sulit → churn | Tinggi | Sistem petunjuk bertahap, playtest kesulitan |
| Obby terlalu frustrasi | Tinggi | Checkpoint sering, kurva kesulitan bertahap |
| Exploit/cheat (teleport, skip) | Sedang | Validasi server-side, anti-cheat (lihat SRD) |
| Produksi map lambat | Sedang | Template + katalog obstacle modular |
| Data pemain hilang | Tinggi | DataStore aman + ProfileService (lihat SRD) |

*Ketergantungan lintas dokumen: Golden Rules dan katalog obstacle ada di GDD; implementasi teknis, keamanan, dan penyimpanan data ada di SRD/TDD.*
