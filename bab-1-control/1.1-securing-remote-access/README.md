# 1.1 — Delinea Platform Guard: Securing Remote Access

> **Bab 1: Control** · Course e-learning, 6 lesson (5 video + 1 quiz)

## Daftar Lesson
| # | Lesson | Durasi | Inti |
|---|---|---|---|
| 1 | [Privileged Remote Access: Overview](01-pra-overview/) | 4:27 | Kenapa VPN tidak cukup; Zero Standing Access; 6 fitur PRA |
| 2 | [Secret Server Launchers](02-secret-server-launchers/) | 4:30 | Credential injection; Protocol Handler; built-in vs custom launcher; direct vs proxied |
| 3 | [Secret Server Workflow](03-secret-server-workflow/) | 4:47 | Access Request, Multi-Level Workflow, CheckOut (OTP), integrasi ticketing |
| 4 | [Credential & Connection Manager](04-credential-connection-manager/) | 6:24 | One vault, two front doors: IT (RDP/SSH) vs business user (web app) |
| 5 | [Session Recording & Monitoring](05-session-recording-monitoring/) | 5:42 | Watch / message / terminate; video + keystrokes + metadata |
| 6 | Quiz | – | ⏳ |

## Gambar Besar: Alur Satu Sesi Akses Privileged
```
Vendor/admin login (MFA)                                  ← Lesson 1
  → Request akses ke Secret + nomor tiket (divalidasi)    ← Lesson 3
    → Approval (1 level / multi-level)                    ← Lesson 3
      → CheckOut (eksklusif, default 30 menit)            ← Lesson 3
        → Launcher / Connection Manager (proxied)         ← Lesson 2 & 4
          → Sesi dimonitor live & direkam                 ← Lesson 5
            → Check-in → password dirotasi                ← Lesson 3
```

## Ringkasan Satu Kalimat
> PRA mengganti **"akses permanen ke jaringan"** (VPN/jump host + password dipegang user) dengan **"akses ke satu sesi, sementara, di-approve, password tidak terlihat, dan direkam."**
