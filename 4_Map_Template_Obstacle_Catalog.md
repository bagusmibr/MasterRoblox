# Map Template + Obstacle Catalog — Cetakan 1 Map Utuh & Katalog Rintangan

**Game:** Project ESCAPE — Hybrid Obby × Escape Room
**Versi:** 1.0 · **Owner:** Bagus · **Referensi:** GDD + Golden Rules v1.1

## Daftar Isi

1. Cara Memakai Template Ini
2. Anatomi Map Standar
3. Contoh Layout Map (Blueprint)
4. Katalog Obstacle
5. Preset Tuning per Tingkat Map
6. Kit Tema (Reskin)
7. Langkah Produksi Map Baru (Ringkas)

## 1. Cara Memakai Template Ini

Dokumen ini adalah cetakan untuk membuat SATU map utuh. Setiap map baru dibuat dengan menyalin struktur di Bagian 2, memilih obstacle dari Katalog di Bagian 4, lalu memvalidasi dengan Checklist di GDD. Ingat: satu template = satu map utuh, dan setiap map selalu diakhiri Sentinel Gate.

*Semua map memakai kerangka yang sama; yang berubah hanyalah tema, pilihan obstacle, dan tuning parameter. Inilah yang menjaga konsistensi seri sekaligus mempercepat produksi.*

## 2. Anatomi Map Standar

Sebuah map dibagi menjadi 5 zona berurutan. Setiap zona memuat sejumlah obstacle dari katalog.

| Zona | Panjang | Isi | Checkpoint |
|---|---|---|---|
| Z1 · Pembuka | ±1–2 mnt | 2 obstacle mudah (ajari mekanik) | CP1 di awal, CP2 di akhir zona |
| Z2 · Aksi Awal | ±2 mnt | 2–3 obstacle obby menengah | CP3 |
| Z3 · Ruang Puzzle | ±2 mnt | 2 puzzle (escape room) untuk buka jalan | CP4 |
| Z4 · Gauntlet | ±2–3 mnt | 2–3 obstacle campuran tekanan tinggi | CP5 |
| Z5 · Sentinel Gate | ±1–2 mnt | Signature obstacle (klimaks) | CP6 tepat sebelum gate |

**Total ≥ 10 obstacle (termasuk Sentinel Gate) sesuai GR-1 & GR-2. Komposisi menjaga minimal 40% obby & 30% puzzle sesuai GR-3.**

## 3. Contoh Layout Map (Blueprint 'Map 01 — Reruntuhan')

Contoh pengisian template dengan 11 obstacle. Kolom 'ID' merujuk ke Katalog di Bagian 4.

| # | Zona | Obstacle (ID) | Tipe | Checkpoint |
|---|---|---|---|---|
| 1 | Z1 | Collapsing Path (OB-01) | Obby | CP1 |
| 2 | Z1 | Swinging Axes (OB-02) | Obby | |
| 3 | Z2 | Rotating Gears (OB-05) | Obby | CP2 |
| 4 | Z2 | Rising Hazard Floor (OB-04) | Obby | |
| 5 | Z2 | Laser Hallway (OB-03) | Obby/Timing | CP3 |
| 6 | Z3 | Pressure-Plate Puzzle (PZ-01) | Puzzle | |
| 7 | Z3 | Color/Symbol Code (PZ-03) | Puzzle | CP4 |
| 8 | Z4 | Moving Wall Crusher (OB-06) | Obby | |
| 9 | Z4 | Lever Sequence (PZ-04) | Puzzle | CP5 |
| 10 | Z4 | Disappearing Platforms (OB-07) | Obby/Timing | |
| 11 | Z5 | THE SENTINEL GATE (SG-00) | Signature | CP6 |

*Perhatikan: tidak ada >3 obstacle bertipe sama berurutan (GR-4), dan checkpoint hadir sebelum tiap lonjakan (GR-6).*

## 4. Katalog Obstacle

Katalog berisi 12 obstacle reusable (8 obby + 4 puzzle) plus Sentinel Gate. Ambil minimal 10 per map. Parameter memakai Attributes di instansi (GR-16).

### 4.1 Obstacle Obby

| ID | Nama | Mekanik Singkat | Parameter Kunci |
|---|---|---|---|
| OB-01 | Collapsing Path | Pijakan runtuh beberapa saat setelah diinjak | CollapseDelay, RespawnTime |
| OB-02 | Swinging Axes | Kapak/bandul mengayun; lewati saat aman | SwingSpeed, Gap, Damage |
| OB-03 | Laser Hallway | Sinar laser hidup-mati berpola; timing | OnTime, OffTime, Pattern |
| OB-04 | Rising Hazard Floor | Lava/air naik; pemain harus terus naik | RiseSpeed, StartDelay |
| OB-05 | Rotating Gears | Silinder/gir berputar; melompat presisi | RotationSpeed, Radius |
| OB-06 | Moving Wall Crusher | Dinding menekan berkala; lewati celahnya | Interval, CloseTime, Damage |
| OB-07 | Disappearing Platforms | Pijakan muncul-hilang berpola | VisibleTime, HiddenTime |
| OB-08 | Wind/Conveyor Push | Dorongan angin/ban berjalan mengubah gerak | PushForce, Direction |

### 4.2 Obstacle Puzzle (Escape Room)

| ID | Nama | Mekanik Singkat | Parameter Kunci |
|---|---|---|---|
| PZ-01 | Pressure-Plate Puzzle | Injak pelat sesuai pola untuk buka pintu | PlateCount, Pattern, ResetOnError |
| PZ-02 | Hidden Key & Lock | Temukan kunci tersembunyi untuk membuka gerbang | KeyCount, HideRadius |
| PZ-03 | Color/Symbol Code | Baca petunjuk di ruangan, masukkan kode di panel | CodeLength, ClueSpots, Attempts |
| PZ-04 | Lever Sequence | Tarik tuas dalam urutan benar | LeverCount, SequenceLen, HintDelay |

### 4.3 Signature Obstacle

| ID | Nama | Ringkas | Detail |
|---|---|---|---|
| SG-00 | THE SENTINEL GATE | 3 fase: Aktivasi Rune (obby) → Petunjuk → Buka Gerbang (puzzle+timing) | Wajib 1 per map. Mekanik identik, hanya reskin. Lihat GDD Bagian 5. |

## 5. Preset Tuning per Tingkat Map

Gunakan preset ini agar kesulitan konsisten saat map baru dibuat. Nilai relatif, disetel di Attributes.

| Parameter | Map Mudah | Map Menengah | Map Sulit |
|---|---|---|---|
| Jarak antar checkpoint | Pendek | Sedang | Panjang |
| Kecepatan obstacle (rata2) | Lambat | Sedang | Cepat |
| Panjang urutan puzzle | 3 | 4 | 5 |
| SentinelTimer (detik) | 40 | 30 | 20 |
| Toleransi timing | Longgar | Sedang | Ketat |
| Jumlah percobaan puzzle | Tak terbatas | Terbatas + hint | Terbatas ketat |

## 6. Kit Tema (Reskin)

Satu map = satu tema (GR-20). Ganti aset visual tanpa mengubah mekanik. Contoh kit:

| Tema | Palet Warna | Material/Aset | Ambience |
|---|---|---|---|
| Reruntuhan Kuno | Cokelat, emas, hijau lumut | Batu, obor, tanaman rambat | Gema, angin |
| Laboratorium | Putih, biru neon | Metal, kaca, hologram | Dengung mesin |
| Kuil Es | Biru, putih, sian | Es, kristal, salju | Angin dingin |
| Pabrik | Abu, oranye, kuning | Baja, pipa, konveyor | Mesin berat |

## 7. Langkah Produksi Map Baru (Ringkas)

1. Salin hierarki map standar (Spawn, Checkpoints, Obstacles, Puzzles, SentinelGate, Exit).
2. Pilih tema dari Kit Tema (Bagian 6).
3. Isi 5 zona (Bagian 2) dengan ≥10 obstacle dari Katalog, sisipkan Sentinel Gate di Z5.
4. Terapkan preset tuning sesuai tingkat kesulitan (Bagian 5).
5. Reskin Sentinel Gate sesuai tema — mekanik tetap identik (GR-19).
6. Pasang checkpoint, tag obstacle, isi Attributes.
7. Playtest: cek kesulitan, waktu tembus 6–12 menit, keterbacaan jalur.
8. Jalankan Checklist Kepatuhan (GDD Bagian 7) sebelum rilis.

**Dengan mengikuti template ini, setiap map baru otomatis konsisten, adil, dan mengandung signature obstacle yang sama — persis tujuan master data ini.**
