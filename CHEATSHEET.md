# Delinea Platform Guard — Summary, Glosarium & Cheat Sheet

> Ringkasan seluruh learning plan **Delinea Platform Guard** (3 bab, 8 sub-bab, 20 video).
> Detail per lesson + screenshot ada di folder masing-masing bab.

**Isi:** [1. Brief Summary](#1-brief-summary) · [2. Glosarium](#2-glosarium) · [3. Cheat Sheet](#3-cheat-sheet)

---

## 1. Brief Summary

### Gambaran besar
**Delinea** = vendor **PAM / identity security** (hasil merger **Thycotic** → Secret Server dan **Centrify** → server PAM / AD bridging).
**Delinea Platform** = platform **cloud-native**, *powered by Iris AI*, yang mengubah "kekacauan identitas" (terlalu banyak akun, izin, & blind spot) menjadi **kontrol berbasis policy**.

Tiga outcome platform = tiga bab learning plan:

| Outcome | Tagline | Pertanyaan yang dijawab | Bab |
|---|---|---|---|
| **Visibility** | *Illuminate YOUR identities* | Identitas & akun apa saja yang ada? Siapa punya akses ke apa? | 3 |
| **Control** | *Accelerate YOUR Business* | Bagaimana akses diberikan, dibatasi, dan direkam? | 1 |
| **Posture** | *Quantify YOUR Risk* | Seberapa berisiko kondisi sekarang? Ada ancaman? | 2 |

> Urutan implementasi yang realistis: **Visibility → Control → Posture**. Tidak bisa melindungi yang tidak terlihat.

### Bab 1 — Control
| Sub-bab | Inti satu kalimat |
|---|---|
| **1.1 Securing Remote Access** | PRA mengganti "akses permanen ke jaringan" (VPN/jump host + password dipegang user) dengan **akses ke satu sesi, sementara, di-approve, password tidak terlihat, dan direkam**. |
| **1.2 Collections** | Collection = **saved inventory query** untuk menargetkan policy, dashboard, report, & izin; mengatur **visibilitas di platform**, **bukan** akses di level mesin. |
| **1.3 PCS** | **Penegak aturan di dalam server** (agent): AD bridging, JIT privilege elevation, MFA at depth, host-based recording. |

### Bab 2 — Posture
| Sub-bab | Inti satu kalimat |
|---|---|
| **2.1 Auditing, Analytics & Alerting** | **Audit** semua aksi → **Analytics** (baseline per user, risk score, cases) → **Reporting** (50+ report) → **Subscriptions** (kasih tahu) & **Pipelines** (bertindak otomatis). |
| **2.2 PCCE / ITP** | **PCCE = CIEM** (rapikan hak akses di cloud), **ITP = ITDR** (deteksi & respons ancaman identitas). |

### Bab 3 — Visibility
| Sub-bab | Inti satu kalimat |
|---|---|
| **3.1 Platform Overview** | Peta platform: 3 outcome, komponen, arsitektur koneksi (**HTTPS 443 outbound only**), business value. |
| **3.2 Continuous Identity Discovery** | Menemukan **semua identitas privileged** (manusia & mesin) terus-menerus: **Sources → Inventory → Checks → Posture Score**. |
| **3.3 Vaulting with Secret Server** | Vault berlapis: **Secrets, Templates, Folders, Policies, Roles** + **Discovery** (temukan) → **RPC** (rotasi) → **Heartbeat** (verifikasi). |

### Satu sesi akses privileged dari awal sampai akhir
```
Discovery menemukan akun ──► akun di-vault (Secret + Template + Folder + Policy)
                                   │
User login Platform (MFA) ──► Request akses + nomor tiket ──► Approval (1 / multi-level)
                                   │
                         CheckOut (eksklusif, default 30 menit)
                                   │
               Launcher / Connection Manager / PRA (proxied, credential injection)
                                   │
          Sesi dimonitor live + direkam  (+ PCS: elevasi JIT & host recording di server)
                                   │
                   Check-in ──► RPC rotasi password ──► Heartbeat verifikasi
                                   │
         Audit setiap aksi ──► Analytics / Risk score ──► Subscription / Pipeline ──► SIEM
```

---

## 2. Glosarium

### Akses & sesi (Bab 1)
| Istilah | Arti singkat |
|---|---|
| **PAM** | Privileged Access Management — mengelola & mengawasi akun berhak tinggi |
| **PRA** | Privileged Remote Access — akses remote **browser-based, VPN-less** ke satu target |
| **Zero Standing Access (ZSA)** | Tidak ada akses permanen; akses **JIT** (just-in-time) & **JEA** (just-enough), dicabut otomatis |
| **Credential injection** | Password dari vault disuntik ke sesi; **user tidak pernah melihatnya** |
| **Launcher** | Tombol "Launch" di Secret → aplikasi terbuka dalam kondisi sudah login |
| **Protocol Handler** | Software kecil di laptop user; jembatan browser → aplikasi lokal (mstsc, PuTTY) |
| **Direct vs Proxied** | Direct: client → target. Proxied: client → **proxy Secret Server** → target (lebih aman, recording lebih kuat) |
| **Custom Launcher** | Tipe: **Process**, **Proxied SSH Process**, **Batch File** |
| **Access Request** | Approval **satu level** sebelum akses; bisa **di-deny setelah approve** → sesi diputus |
| **Multi-Level Workflow** | Approval berjenjang, **hingga 15 level** |
| **CheckOut (OTP)** | Akses **eksklusif untuk 1 orang** dalam waktu tertentu (default **30 menit**) → akuntabilitas |
| **Ticketing integration** | Wajib nomor tiket (Jira, BMC Remedy, ManageEngine, ServiceNow) |
| **Connection Manager** | **Aplikasi desktop** multi-sesi RDP/SSH untuk **IT** |
| **Credential Manager** | **Browser extension + mobile app** untuk login web app **user bisnis** |
| **Session Monitoring** | Live: **watch, send message, terminate** |
| **Session Recording** | Rekaman: **video, keystrokes (opsional, searchable), metadata** (advanced: proses & window) |
| **Platform Collection** | Saved inventory query atas **Computers, Remote Apps (PRA), Web Apps (PRA)** |
| **PCS** | Privilege Control for Servers — kontrol login & elevasi **di dalam server** |
| **AD Bridging** | Linux/Unix join ke AD → **satu identitas AD** lintas OS |
| **Managed computer** | Mesin yang sudah terpasang **PCS Agent** |
| **Platform Engine** | Komponen on-prem wajib; menjalankan **Audit Collector** & **Command Relay** |
| **Audit Collector** | Kirim data audit level mesin ke Platform |
| **Command Relay** | Kontrol perintah sesuai policy lewat **SSH** |
| **Emergency Access** | Template PCS: login + elevasi **saat server tidak bisa konek ke platform** (break-glass) |

### Posture (Bab 2)
| Istilah | Arti singkat |
|---|---|
| **Baseline** | Pola normal per user: **lokasi, IP, browser** (dari audit log Platform & Secret Server Cloud) |
| **Alert** | Deviasi dari baseline / konfigurasi |
| **Analytics Case** | Kumpulan alert terkait yang **diagregasi & dikorelasikan** → kurangi alert fatigue |
| **User Risk Score** | Skor near-real-time, **diagregasi** dari **alert yang belum di-resolve** + akses/permission |
| **Audit log** | Catatan **mentah** setiap aksi: **Who / When / What** |
| **Report** | **Rangkuman** data audit; **50+ built-in**, custom via **SQL**, on-demand/scheduled |
| **Dual Controls** | Orang kedua wajib memasukkan credential untuk melihat report/rekaman (lindungi **PII**) |
| **User Audit Report** | Semua yang diakses **satu orang** dalam rentang waktu + bisa picu ganti password |
| **Security Hardening Report** | Cek konfigurasi IIS, SQL Server, Windows, AD |
| **Event Subscription** | "Tell me when this happens" — **alert saja**, read-only |
| **Event Pipeline** | **Trigger + Filter + Task** — bisa **bertindak** otomatis |
| **Pipeline Policy** | Pengelompokan pipeline yang menargetkan **folder, secret policy, atau user group** |
| **UVAM** | Unlimited Vault Access Mode — admin bisa melihat **semua** secret (break-glass) |
| **PCCE** | Privilege Control for Cloud Entitlements = **CIEM** |
| **ITP** | Identity Threat Protection = **ITDR** |
| **CIEM** | Cloud Infrastructure Entitlement Management — siapa punya hak apa di cloud |
| **ITDR** | Identity Threat Detection & Response — deteksi & respons ancaman identitas |
| **ITP/PCCE Collection** | Pengelompokan **identitas & hak** (bukan mesin); punya **System Collections** tenant-wide |

### Visibility & Vaulting (Bab 3)
| Istilah | Arti singkat |
|---|---|
| **CID** | Continuous Identity Discovery — discovery identitas privileged **real-time & terus-menerus** |
| **Sources** | Integrasi CID ke sistem (AD, Entra ID, AWS, GCP, Workday, Snowflake, Ping) via connector **Marketplace** |
| **Inventory (CID)** | 8 objek: Identities, Accounts, Groups, Assets, Memberships, Access Policies, Privileges, Activities |
| **Identity vs Account** | Identity = **orangnya/entitasnya**; Account = akunnya **di satu aplikasi** |
| **Checks** | 6 kategori: AI Security, Authentication, Privileged Access, Privilege Escalation, Stale Access, Vaulting |
| **Posture Score** | Persentase check yang lulus |
| **Orphaned account** | Akun yang pemiliknya sudah tidak ada |
| **Shadow admin** | Tidak terlihat admin, tapi punya hak setara admin (delegasi / nested group) |
| **Entitlement drift** | Hak akses menumpuk melebihi kebutuhan |
| **Secret** | Apa pun di vault: password, **SSH key, file, lisensi, catatan, PIN** |
| **Secret Template** | **Struktur & field** sebuah Secret; Secret **dibuat dari** template |
| **Folder** | **Mengelompokkan** secret + **kontrol akses** (permission ke banyak secret sekaligus) |
| **Secret Policy** | **Setting keamanan** untuk banyak secret sekaligus (rotasi, workflow, **session recording**); dipasang ke secret atau folder |
| **Role / Role Permission** | **Apa yang boleh dilakukan** user di platform (built-in & custom) |
| **Discovery (Secret Server)** | Scan jaringan untuk menemukan **akun privileged yang tidak dikenal** |
| **Extensible Discovery** | Discovery kustom via script: non-domain, network device, DB, Azure tenant, config file berisi password |
| **Discovery Rules** | Otomatis: import (pakai template), rotasi, alert, attach dependency |
| **RPC** | Remote Password Changing — **mengganti** password di target (on-demand, bulk, scheduled) |
| **Service account** | Akun yang dipakai **aplikasi/proses**, bukan manusia |
| **Dependency** | Tempat password service account tertanam: **IIS App Pool, Scheduled Task, Windows Service, SSH key** |
| **Custom password changer** | Script **PowerShell, SQL, atau via SSH** |
| **Heartbeat** | **Mengecek** (login uji): Engine heartbeat & Credential heartbeat |
| **Distributed Engine** | Engine Secret Server di site lain: rotasi, discovery, heartbeat, proxy |
| **Connector** | Jembatan platform ke AD & sistem internal |

---

## 3. Cheat Sheet

### 3.1 Pasangan yang sering ditukar di soal
| A | vs | B | Kunci pembeda |
|---|---|---|---|
| **Access Request** | | **CheckOut** | Request = **minta izin** dulu · CheckOut = **kunci eksklusif** 1 orang, waktu terbatas |
| **Session Monitoring** | | **Session Recording** | Monitoring = **menghentikan** (live) · Recording = **membuktikan** (sesudahnya) |
| **Direct launcher** | | **Proxied launcher** | Proxied = credential di proxy, target tak perlu terbuka dari client → **wajib untuk vendor** |
| **Launcher** | | **Connection Manager** | Launcher = **mekanisme** buka 1 app · CM = **aplikasi desktop** banyak sesi |
| **Connection Manager** | | **Credential Manager** | IT + RDP/SSH + desktop · Business user + web app + extension |
| **Platform Collections** | | **ITP/PCCE Collections** | Mesin & aplikasi · Identitas & hak akses |
| **Role-level permission** | | **Collection-level permission** | **Role selalu menang** (Launch with PRA di role = restriksi collection tidak berlaku) |
| **Collections** | | **PCS** | Visibility di platform · **Enforcement di mesin** |
| **Proxy-based recording** | | **Host-based recording** | Proxy buta terhadap bypass · Host (PCS agent) merekam semua login ke server |
| **Alert** | | **Analytics Case** | Satu deviasi · **Kumpulan** alert terkait |
| **Audit log** | | **Report** | Data **mentah** setiap event · **Rangkuman** |
| **Audit log satu secret** | | **User Audit Report** | Fokus ke **objek** · Fokus ke **orang** |
| **Event Subscription** | | **Event Pipeline** | **Kasih tahu** (detektif) · **Bertindak** (otomatis) |
| **Pipeline Filter** | | Menghapus event | Filter = **menentukan event mana yang ditindaklanjuti**; event tetap ada di audit |
| **PCCE (CIEM)** | | **ITP (ITDR)** | Postur hak akses cloud (preventif) · Deteksi & respons ancaman identitas |
| **SS Discovery** | | **CID** | Akun & credential → vault · Identitas & hak akses → posture |
| **Secret Template** | | **Secret Policy** | **Bentuk** data · **Perilaku** keamanan |
| **Folder** | | **Role** | Akses ke **secret mana** · **Apa yang boleh dilakukan** di platform |
| **RPC** | | **Heartbeat** | **Mengganti** password · **Mengecek** password |
| **Heartbeat Failed** | | **Unable to Connect** | Password **ditolak** (berubah di luar vault) · Target **tak terjangkau** (jaringan) |
| **Engine Heartbeat** | | **Credential Heartbeat** | Engine → Platform (online & config) · Secret Server → target (password valid) |

### 3.2 "Fitur mana yang punya kemampuan ini?"
| Kemampuan | Pemilik |
|---|---|
| Akses remote tanpa VPN via browser | **PRA** |
| Transfer file ke/dari target | **PRA** |
| Launch web app privat | **PRA** (Web Applications) |
| Password tidak pernah terlihat user | **Credential injection** (Launcher/PRA) |
| Minta persetujuan sebelum akses | **Workflow** (Access Request / Multi-Level) |
| Akses eksklusif + akuntabilitas | **CheckOut** |
| Memutus sesi yang sedang berjalan | **Session Monitoring** (terminate) / Access Request di-deny |
| Membuktikan apa yang dilakukan user | **Session Recording** |
| Memblokir perintah tertentu | **Command filtering** (PRA) / **Command Relay** (PCS) |
| Elevasi privilege di server | **PCS** (JIT Privilege Elevation) |
| Satu identitas AD di Linux | **PCS** (AD Bridging) |
| Login server saat platform tak terjangkau | **PCS Emergency Access** |
| Menargetkan policy ke sekelompok aset | **Collections** |
| Baseline perilaku & risk score | **Platform Analytics** |
| Menggabungkan alert terkait | **Analytics Cases** |
| Siapa/kapan/apa untuk setiap aksi | **Audit log** |
| Laporan untuk auditor | **Reports** (built-in / custom SQL / scheduled) |
| Melindungi report/rekaman berisi PII | **Dual Controls** |
| Notifikasi email/Slack/inbox | **Event Subscription** |
| Aksi otomatis (edit secret, script, webhook) | **Event Pipeline** |
| Right-sizing izin cloud | **PCCE** |
| Deteksi credential stuffing / ancaman identitas | **ITP** |
| Menemukan identitas & shadow admin lintas IdP/cloud/SaaS | **CID** |
| Deteksi PAM bypass | **CID** |
| Menemukan akun privileged yang tidak dikenal di jaringan | **Secret Server Discovery** |
| Rotasi password terjadwal / massal | **RPC** (+ Secret Policy) |
| Update password service account + restart service | **RPC + Dependencies** |
| Memastikan password di vault masih valid | **Credential Heartbeat** |
| Struktur field secret | **Secret Template** |
| Mengelompokkan secret + permission massal | **Folder** |
| Setting keamanan (rotasi, workflow, **recording**) massal | **Secret Policy** |

### 3.3 Angka & default yang wajib hafal
| Item | Nilai |
|---|---|
| Durasi CheckOut default | **30 menit** |
| Level Multi-Level Workflow | **hingga 15** |
| Built-in report Secret Server | **50+** |
| Siklus rotasi yang umum disyaratkan | **15 / 30 / 45 / 60 hari** |
| Engine heartbeat | **tiap 5–10 menit** |
| Koneksi on-prem → cloud | **HTTPS 443, outbound only** |
| Account proliferation | akun **> identitas user 30%+** |
| Objek Inventory CID | **8** |
| Kategori Checks CID | **6** |
| Built-in discovery types | **6** (AD, AWS, GCP, Unix/Linux, VMware ESX/ESXi, SSH Keys) |
| Status Credential Heartbeat | **4** (Success, Processing, Failed, Unable to Connect) |
| Tipe Platform Collection | **3** (Computers, Remote Apps, Web Apps) |
| Komponen recording | **3** (video, keystrokes, metadata) |
| Aksi Session Monitoring | **3** (watch, message, terminate) |
| Blok Event Pipeline | **3** (trigger, filter, task) |
| Tingkat use case vaulting | **4** (personal-kerja, shared, shared privileged, elevated privileged) |

### 3.4 Daftar wajib per fitur
- **4 pilar PCS:** Identity Consolidation/AD Bridging · Privilege Elevation · MFA at Depth · Session Audit/Recording
- **5 core capabilities PCS:** Comprehensive Discovery · Unify Linux/Unix w/ AD · JIT Privilege Elevation · Policy-based Controls · Host-based Auditing
- **6 istilah PCS:** PCS Agent · PCS Policies · Inventory · Engine Mgmt. · Audit Collector · Command Relay
- **3 template policy PCS:** Privilege Elevation · Endpoint Login · Emergency Access (**deny menang atas allow**)
- **Built-in launcher:** RDP · PuTTY · Web Password Filler · Web Launcher
- **Analytics Case types:** Brute Force · MFA Bombing · Suspicious Behavior
- **Event yang diaudit Platform:** Engine Mgmt · Policy · Remote Access · Inventory
- **Event yang diaudit Secret Server:** view/launch/copy secret · users · templates · policies · **perubahan konfigurasi**
- **5 kapabilitas CID:** Comprehensive Discovery · Uncover Privileged Users · Automatic Vaulting · Detect PAM Bypass · Uncover Misconfigurations
- **3 core use case CID:** Visibility & Discovery · Reduce Identity Sprawl · Robust Identity Governance
- **Discovery AD menemukan:** Domain accounts · IIS App Pools · Local Windows accounts · Windows Services · Scheduled Tasks
- **Discovery Rules:** import (template) · rotasi · alert · attach dependency
- **5 Platform Services:** Continuous Discovery · Audit & Analytics · AI Driven · MFA · Ecosystem
- **4 business value:** Reduce risk · Operational velocity · Lower cost & complexity · Future-proof
- **Komponen on-prem:** Platform Engine · Distributed Engine · Connector · (Agent di server PCS)

### 3.5 Pola jebakan soal
1. **Tukar nama fitur** — deskripsi benar, ditempel ke fitur lain. Tanya: *"ini kerjaan siapa?"* (pakai tabel 3.2).
2. **Kata mutlak** ("only", "always", "every", "never") — curigai kecuali materi menyebutnya eksplisit.
3. **Sebelum vs sesudah** — preventif (workflow, policy, PCS deny) vs detektif (subscription, monitoring, recording).
4. **Objek vs orang** — audit log secret vs User Audit Report.
5. **"By default"** — notifikasi butuh subscription yang dikonfigurasi; status ditandai tanpa konfigurasi.
6. **Detail spesifik di soal** ("shared", "used regularly", "already found") menunjuk ke fitur yang paling relevan, walau semua opsi benar secara umum.
7. **Aksi destruktif** (hapus, blokir) jarang jadi perilaku default fitur keamanan vault — expiration & heartbeat failure **menandai**, bukan menghapus.

### 3.6 Catatan praktis (dunia nyata, bukan materi exam)
- **Bypass PAM** adalah celah terbesar: buat rule SIEM sendiri → logon ke server kritikal dari IP **bukan** Engine/proxy Delinea = alert tinggi.
- **Heartbeat Failed** = password diganti di luar vault → **investigasi**, jangan cuma reset.
- **Service account:** discover → petakan dependency → uji non-prod → baru rotasi otomatis. Pertimbangkan **gMSA** dulu.
- **Engine, Connector, Distributed Engine, AD** = **tier-0**.
- **Recording & report** = data sensitif (bisa berisi data nasabah) → Dual Controls, batasi playback, pertimbangkan **UU PDP**.
- **SaaS/cloud** → cek ketentuan **OJK** soal penempatan data sebelum memilih deployment.
- Teruskan **audit, cases, subscription** ke **SIEM** agar terkorelasi & audit trail tidak hanya di satu sistem.
