# SRD / TDD — System & Technical Requirement Document

**Game:** Project ESCAPE — Roblox (Luau)
**Versi:** 1.1 · **Owner:** Bagus · **Referensi:** PRD v1.1, GDD v1.1
*Perubahan v1.1: menghapus MonetizationService, Shop UI, dan semua logika pembelian/gamepass. Tanpa PvP & trading.*

## Daftar Isi

1. Tujuan Dokumen
2. Gambaran Arsitektur
3. Data Flow
4. Spesifikasi Jaringan (RemoteEvent/Function)
5. Penyimpanan Data (Persistence)
6. Keamanan (Anti-Cheat & Server Authority)
7. Performa
8. Struktur Map Standar (Kontrak Teknis)
9. Tooling & Kualitas

## 1. Tujuan Dokumen

Dokumen ini menerjemahkan kebutuhan produk (PRD) dan desain (GDD) menjadi spesifikasi teknis yang dapat diimplementasikan di Roblox Studio menggunakan Luau. Mencakup arsitektur, struktur service, aliran data (data flow), jaringan (RemoteEvent), keamanan, penyimpanan data, dan performa. Versi ini tidak mencakup monetisasi, PvP, maupun trading.

## 2. Gambaran Arsitektur

Arsitektur mengikuti model client-server otoritatif Roblox: server adalah sumber kebenaran (source of truth) untuk progres, timer, dan pembukaan gerbang; client hanya menangani input, kamera, dan UI. Kode disusun modular berbasis service agar tiap map memakai kembali sistem yang sama.

### 2.1 Penempatan Service Roblox

| Container | Isi | Alasan |
|---|---|---|
| ServerScriptService | GameService, ProgressService, PuzzleService, AntiCheatService | Logika otoritatif, tidak bisa diakses client |
| ReplicatedStorage | Remotes (RemoteEvent/Function), Modul bersama (ObstacleConfig, Signals), Assets | Dibagi client & server dengan aman |
| ServerStorage | Template map, data rahasia (kunci puzzle, jawaban) | Hanya server, tidak direplikasi ke client |
| StarterPlayerScripts | InputController, CameraController, PuzzleUIController | Logika sisi client per pemain |
| StarterGui | ScreenGui HUD, Puzzle UI | Antarmuka pemain |
| Workspace | Instansi map aktif (dijaga ringan) | Dunia 3D live |

### 2.2 Modul Inti (Server)

| Modul | Tanggung Jawab |
|---|---|
| GameService | Orkestrasi state map, load/unload map dari ServerStorage, spawn pemain |
| ProgressService | Simpan/muat map & checkpoint terakhir pemain (via DataStore) |
| CheckpointService | Registrasi checkpoint, validasi urutan (anti-skip) |
| PuzzleService | Verifikasi jawaban puzzle server-side, buka pintu/gate |
| SentinelGateService | Logika signature obstacle: fase obby + fase puzzle + rate-limit |
| AntiCheatService | Deteksi teleport, speed, respawn abuse; validasi input |

## 3. Data Flow

Alur data satu siklus map (disederhanakan):

1. Pemain memilih map di lobby → client kirim RequestEnterMap ke server.
2. GameService memvalidasi kelayakan (progres pemain) → load template map dari ServerStorage ke Workspace.
3. Pemain melewati obstacle; menyentuh checkpoint → client kirim TouchCheckpoint; CheckpointService validasi posisi & urutan lalu simpan.
4. Pada puzzle, client kirim input (mis. kode) via SubmitPuzzle → PuzzleService bandingkan dengan jawaban di ServerStorage.
5. Di Sentinel Gate, SentinelGateService memverifikasi fase obby selesai + jawaban benar → buka gate & catat waktu.
6. MapComplete → ProgressService simpan progres, LeaderboardService update waktu.

**Prinsip: setiap keputusan penting (checkpoint sah, puzzle benar, gate terbuka, waktu) dihitung di server, bukan client.**

## 4. Spesifikasi Jaringan (RemoteEvent/Function)

| Nama | Tipe | Arah | Payload | Validasi Server |
|---|---|---|---|---|
| RequestEnterMap | RemoteFunction | C→S | mapId | Cek progres pemain |
| TouchCheckpoint | RemoteEvent | C→S | checkpointId | Jarak posisi, urutan, cooldown |
| SubmitPuzzle | RemoteFunction | C→S | puzzleId, answer | Bandingkan jawaban, rate-limit |
| SentinelSubmit | RemoteFunction | C→S | gateId, answer | Fase obby selesai + jawaban |
| MapCompleted | RemoteEvent | S→C | mapId, time | Server-authoritative |
| UpdateHUD | RemoteEvent | S→C | state | Broadcast state ke client |

*Semua RemoteEvent C→S wajib menerapkan rate limiting per pemain (lihat 6.2) untuk mencegah flooding.*

## 5. Penyimpanan Data (Persistence)

### 5.1 Skema Data Pemain

| Field | Tipe | Deskripsi |
|---|---|---|
| highestMap | number | Map tertinggi yang terbuka |
| lastCheckpoint | table {mapId, cpId} | Checkpoint terakhir per map |
| bestTimes | table {mapId = seconds} | Waktu terbaik per map |
| cosmetics | table | Kosmetik yang diperoleh gratis dari reward (opsional) |
| stats | table | Total percobaan, penyelesaian, dsb. |

### 5.2 Aturan Persistence (WAJIB)

- Gunakan ProfileService (atau setara) — JANGAN raw SetAsync untuk data pemain. [SE-1]
- Setiap panggilan DataStore dibungkus pcall; tangani kegagalan dengan retry ber-backoff.
- Session locking untuk mencegah duplikasi/korupsi saat pemain join di beberapa server.
- Simpan otomatis saat checkpoint tercapai & saat PlayerRemoving; BindToClose untuk shutdown.

## 6. Keamanan (Anti-Cheat & Server Authority)

Golden rule keamanan: Never trust the client. Semua nilai divalidasi tipe, rentang, kepemilikan, dan cooldown di server.

### 6.1 Ancaman & Mitigasi

| Ancaman | Mitigasi |
|---|---|
| Teleport ke akhir map / skip obstacle | Validasi jarak checkpoint berurutan; tolak lompatan posisi tak wajar |
| Speed/fly hack | Cek kecepatan rata-rata antar checkpoint di server |
| Submit jawaban puzzle brute-force | Rate-limit percobaan + cooldown + jawaban hanya di ServerStorage |
| Skip Sentinel Gate | Gate hanya terbuka via SentinelGateService; tidak ada jalur client |
| Remote flooding | Rate limiting per pemain per remote [SE-5] |
| Manipulasi progres/leaderboard | Semua progres & waktu dihitung server-side |

### 6.2 Rate Limiting

Terapkan token-bucket per pemain per remote (mis. maksimal N request/detik). Lewat batas → drop + log untuk AntiCheatService.

## 7. Performa

| Area | Target | Teknik |
|---|---|---|
| Frame rate | 60 FPS di device menengah | Batasi part dinamis, gunakan streaming |
| Memory | Stabil, tanpa kebocoran | Disconnect event via Maid/Trove [SE-4] |
| Streaming | Load map bertahap | StreamingEnabled untuk map besar |
| Obstacle bergerak | Hemat, tidak per-frame di server | Gunakan Tween/PrismaticConstraint, bukan loop Heartbeat berat |
| Network | Payload kecil | Kirim event hanya saat perubahan state penting |

## 8. Struktur Map Standar (Kontrak Teknis)

Setiap map di ServerStorage HARUS mengikuti hierarki berikut agar kompatibel dengan sistem:

| Node | Tipe | Keterangan |
|---|---|---|
| Map_&lt;id&gt; | Model/Folder | Root map |
| &nbsp;&nbsp;Spawn | SpawnLocation | Titik awal |
| &nbsp;&nbsp;Checkpoints | Folder | Berisi Checkpoint_1..N (berurutan) |
| &nbsp;&nbsp;Obstacles | Folder | Instansi obstacle (tagged CollectionService) |
| &nbsp;&nbsp;Puzzles | Folder | Node puzzle + Attribute konfigurasi |
| &nbsp;&nbsp;SentinelGate | Model | Signature obstacle (wajib, 1 per map) |
| &nbsp;&nbsp;Exit | Part | Trigger MapComplete |

*Konfigurasi tiap obstacle diletakkan sebagai Attributes pada instansinya (mis. Speed, Interval, Damage) dan dibaca modul ObstacleConfig — sehingga tuning tidak perlu ubah kode.*

## 9. Tooling & Kualitas

- Rojo untuk sinkronisasi kode ↔ Studio dan version control (Git).
- Konvensi penamaan konsisten; pisahkan modul server/client/shared.
- Playtest checklist & unit test untuk modul puzzle sebelum rilis map baru.
- MCP Roblox Studio untuk otomasi build/insert/eksekusi Luau saat produksi map.
