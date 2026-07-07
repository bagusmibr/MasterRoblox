# GDD + Golden Rules — Game Design Document & Aturan Emas Map Escape

**Game:** Project ESCAPE — Hybrid Obby × Escape Room
**Versi:** 1.1 · **Owner:** Bagus · **Referensi:** PRD v1.1, SRD/TDD v1.1
*Perubahan v1.1: tanpa monetisasi/shop, PvP, dan trading. GR-21 disesuaikan.*

## Daftar Isi

1. Pilar Desain
2. Core Gameplay Loop
3. Kurva Kesulitan & Pacing
4. GOLDEN RULES — Aturan Emas Membangun Map Escape
5. Signature Obstacle — The Sentinel Gate
6. Reward & Progresi
7. Checklist Kepatuhan Map (Sebelum Rilis)

## 1. Pilar Desain

Empat pilar yang memandu setiap keputusan desain. Jika sebuah ide melanggar pilar, ide itu ditolak.

| Pilar | Arti | Implikasi Desain |
|---|---|---|
| Tegang tapi Adil | Sulit namun selalu bisa dikuasai | Checkpoint sering, kegagalan mengajari, bukan menghukum acak |
| Gerak + Pikir | Obby (skill) berpadu puzzle (logika) | Setiap map mencampur segmen aksi & segmen berpikir |
| Identitas Konsisten | Semua map terasa satu seri | Signature obstacle wajib di tiap map |
| Momentum | Pemain jarang menganggur | Puzzle punya solusi jelas; tidak ada dead-end membingungkan |

## 2. Core Gameplay Loop

Loop utama yang diulang pemain di setiap map:

1. Masuk map → orientasi cepat (lihat tujuan/pintu keluar).
2. Segmen OBBY: lewati rintangan gerak & timing hingga checkpoint.
3. Segmen PUZZLE: pecahkan teka-teki untuk membuka jalan berikutnya.
4. Ulangi selang-seling obby ↔ puzzle (minimal 10 obstacle total).
5. SENTINEL GATE: klimaks yang menggabungkan obby + puzzle.
6. Keluar map → reward, waktu tercatat, map berikutnya terbuka.

*Ritme ideal: pola A-P-A-P (Aksi–Puzzle) agar pemain berganti mode berpikir dan tidak jenuh.*

## 3. Kurva Kesulitan & Pacing

| Zona Map | Kesulitan | Fokus | Checkpoint |
|---|---|---|---|
| Pembuka (0–25%) | Ringan | Ajari mekanik dasar | Rapat |
| Tengah (25–70%) | Menengah | Kombinasikan mekanik | Sedang |
| Menuju klimaks (70–90%) | Menengah-tinggi | Tekanan waktu & presisi | Sedang |
| Sentinel Gate (90–100%) | Puncak | Gabungan skill + logika | Sebelum gate |

**Aturan pacing: naikkan kesulitan bertahap; jangan pernah menaruh puncak kesulitan sebelum pemain menguasai dasarnya. Selalu beri checkpoint tepat sebelum lonjakan sulit.**

## 4. GOLDEN RULES — Aturan Emas Membangun Map Escape

**Ini adalah kontrak desain. Setiap map yang dibangun dari template WAJIB mematuhi seluruh aturan berikut. Aturan dikelompokkan menjadi lima kategori.**

### 4.1 Struktur & Konten

- **GR-1** — Setiap map memuat MINIMAL 10 obstacle yang dapat dibedakan.
- **GR-2** — Setiap map WAJIB memuat tepat satu Sentinel Gate (signature obstacle) sebagai rintangan penutup.
- **GR-3** — Komposisi seimbang: minimal 40% obstacle bertipe obby dan minimal 30% bertipe puzzle. Sisanya campuran.
- **GR-4** — Selang-seling tipe: jangan menaruh lebih dari 3 obstacle bertipe sama secara berurutan.
- **GR-5** — Panjang map ditargetkan 6–12 menit untuk pemain rata-rata pada percobaan pertama.

### 4.2 Keadilan (Fairness)

- **GR-6** — Checkpoint diletakkan sebelum setiap lonjakan kesulitan; jarak antar-checkpoint tidak melebihi ~45 detik permainan sukses.
- **GR-7** — Tidak ada 'kematian tak terlihat': semua bahaya harus punya isyarat visual/audio sebelum aktif.
- **GR-8** — Setiap puzzle punya SATU solusi logis yang jelas dan petunjuk yang cukup di dalam ruangan; dilarang tebak-tebakan buta.
- **GR-9** — Puzzle menyediakan petunjuk bertahap (hint) setelah pemain gagal/menganggur beberapa kali.
- **GR-10** — Tidak ada dead-end permanen: pemain tidak boleh terjebak tanpa jalan maju atau reset.

### 4.3 Keterbacaan (Readability)

- **GR-11** — Jalur maju selalu terbaca: gunakan pencahayaan, warna, atau panah lingkungan untuk mengarahkan.
- **GR-12** — Elemen interaktif (tuas, pelat, tombol) punya bahasa visual konsisten di semua map.
- **GR-13** — Bahaya memakai kode warna konsisten (mis. merah = mematikan, kuning = hati-hati/timing).
- **GR-14** — Feedback instan untuk setiap aksi: benar/salah puzzle, checkpoint tercapai, gate terbuka.

### 4.4 Teknis & Keamanan (ringkas; detail di SRD)

- **GR-15** — Semua keputusan penting (checkpoint sah, puzzle benar, gate terbuka, waktu) dihitung SERVER-SIDE.
- **GR-16** — Konfigurasi obstacle memakai Attributes pada instansi (bukan hardcode) agar mudah dituning.
- **GR-17** — Obstacle memakai CollectionService tag agar sistem mengenalinya otomatis.
- **GR-18** — Map mengikuti hierarki standar (Spawn, Checkpoints, Obstacles, Puzzles, SentinelGate, Exit).

### 4.5 Identitas & Tema

- **GR-19** — Signature obstacle (Sentinel Gate) boleh di-reskin sesuai tema map, tetapi MEKANIKNYA harus identik di semua map.
- **GR-20** — Setiap map punya satu tema visual jelas (mis. reruntuhan, laboratorium, kuil) yang diterapkan konsisten.
- **GR-21** — Sentinel Gate TIDAK boleh bisa di-skip dengan cara apa pun — ia wajib dilalui setiap pemain. (Game ini tanpa monetisasi, jadi tidak ada mekanisme skip berbayar.)

## 5. Signature Obstacle — The Sentinel Gate

Sentinel Gate (Gerbang Penjaga) adalah rintangan penanda yang WAJIB hadir di setiap map. Ia dirancang khusus untuk menyatukan dua jiwa game: keterampilan obby dan pemecahan puzzle escape room. Karena hadir di mana-mana dengan mekanik identik, ia menjadi ciri khas seri Project ESCAPE.

### 5.1 Konsep

Sebuah gerbang besar terkunci di ujung map, dijaga oleh 'Sentinel' (patung/mata penjaga bertema). Untuk membukanya, pemain harus melakukan dua hal sekaligus: (a) melewati gauntlet obby singkat untuk mengaktifkan 3 rune/panel yang tersebar, lalu (b) memecahkan urutan puzzle di panel utama sebelum waktu 'penjagaan' habis.

### 5.2 Anatomi (3 Fase)

| Fase | Jenis | Yang Dilakukan Pemain | Kegagalan |
|---|---|---|---|
| 1. Aktivasi Rune | Obby | Lewati 3 jalur pendek untuk menyalakan 3 rune di sekitar gerbang | Jatuh → kembali ke checkpoint gate |
| 2. Baca Petunjuk | Puzzle | Rune yang menyala menampilkan simbol/urutan sebagai clue | — |
| 3. Buka Gerbang | Puzzle + Timing | Masukkan urutan benar di panel utama sebelum timer 'Sentinel' habis | Salah/telat → reset panel, rune tetap menyala |

### 5.3 Parameter yang Dapat Dituning

| Parameter | Deskripsi | Rentang Saran |
|---|---|---|
| RuneCount | Jumlah rune obby yang diaktifkan | 3 (tetap untuk identitas) |
| SequenceLength | Panjang urutan puzzle | 3–5 simbol |
| SentinelTimer | Waktu memecahkan panel | 20–40 detik |
| ObbyDifficulty | Kesulitan gauntlet rune | Menengah–tinggi |
| ResetBehavior | Apa yang ter-reset saat gagal | Panel reset, rune tetap |

### 5.4 Aturan Wajib Sentinel Gate

- Mekanik 3-fase (Aktivasi → Petunjuk → Buka) identik di SEMUA map — hanya visual/tema yang berubah (GR-19).
- Selalu ada checkpoint tepat sebelum gate (GR-6).
- Petunjuk urutan selalu tersedia di dalam arena gate — tidak boleh mengandalkan hafalan dari luar (GR-8).
- Verifikasi urutan dilakukan server-side melalui SentinelGateService (GR-15).
- Tidak bisa di-skip dengan cara apa pun (GR-21).

### 5.5 Contoh Reskin per Tema

| Tema Map | Wujud 'Sentinel' | Rune | Panel Urutan |
|---|---|---|---|
| Reruntuhan Kuno | Patung penjaga batu | Obor api | Ukiran simbol batu |
| Laboratorium | Mata kamera keamanan | Terminal daya | Keypad hologram |
| Kuil Es | Golem kristal | Pilar es bercahaya | Kepingan salju bersimbol |
| Pabrik | Bos mesin | Sakelar industri | Panel tombol warna |

## 6. Reward & Progresi

- Menyelesaikan map: buka map berikutnya + catat waktu terbaik.
- Menaklukkan Sentinel Gate memberi micro-reward (efek/badge) untuk memperkuat rasa 'penanda'.
- Semua reward diperoleh GRATIS lewat gameplay (tanpa pembelian, shop, atau trading) dan bersifat kosmetik/progres — bukan kekuatan, sehingga fair-play terjaga.

## 7. Checklist Kepatuhan Map (Sebelum Rilis)

Sebuah map baru hanya boleh dirilis jika SEMUA butir tercentang:

- [ ] ≥10 obstacle & tepat 1 Sentinel Gate (GR-1, GR-2)
- [ ] Komposisi obby/puzzle seimbang & selang-seling (GR-3, GR-4)
- [ ] Checkpoint sebelum tiap lonjakan sulit (GR-6)
- [ ] Semua bahaya punya isyarat; tidak ada kematian tak terlihat (GR-7)
- [ ] Tiap puzzle punya solusi jelas + hint bertahap (GR-8, GR-9)
- [ ] Jalur & bahaya terbaca via warna/cahaya konsisten (GR-11–GR-13)
- [ ] Validasi server-side aktif; config via Attributes (GR-15, GR-16)
- [ ] Hierarki map standar terpenuhi (GR-18)
- [ ] Sentinel Gate mekanik identik, hanya reskin (GR-19); tidak bisa di-skip (GR-21)
- [ ] Sudah di-playtest kesulitan & waktu tembus 6–12 menit (GR-5)
