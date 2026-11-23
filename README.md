# Learn Delinea — Catatan Belajar Delinea Platform Guard

Catatan belajar mandiri **Delinea PAM** dari [Delinea University](https://university.delinea.com/), learning plan **Delinea Platform Guard**.
Setiap lesson berisi ringkasan slide, screenshot, penjelasan konsep dengan analogi, catatan dari sudut pandang senior engineer, dan soal cek pemahaman.

> Video di Delinea University tidak memiliki subtitle, jadi catatan disusun dari isi slide + deskripsi lesson.

## Roadmap

| Bab | Sub-bab | Status |
|---|---|---|
| **1. Control** | [1.1 Securing Remote Access](bab-1-control/1.1-securing-remote-access/) | ✅ Selesai (5 video) · ⏳ Quiz |
| | 1.2 Collections | ⏳ Belum |
| | 1.3 Privilege Control for Servers (PCS) | ⏳ Belum |
| **2. Posture** | 2.1 Auditing, Analytics, and Alerting | ⏳ Belum |
| | 2.2 Entitlement and Identity Threat Protection (PCCE/ITP) | ⏳ Belum |
| **3. Visibility** | 3.1 Platform Overview | ⏳ Belum |
| | 3.2 Continuous Identity Discovery | ⏳ Belum |
| | 3.3 Vaulting with Secret Server | ⏳ Belum |

## Struktur Repo
```
bab-1-control/
└── 1.1-securing-remote-access/
    ├── README.md                      ← ringkasan sub-bab
    ├── 01-pra-overview/
    │   ├── README.md                  ← catatan lesson
    │   └── img/                       ← screenshot slide
    ├── 02-secret-server-launchers/
    ├── 03-secret-server-workflow/
    ├── 04-credential-connection-manager/
    └── 05-session-recording-monitoring/
```

## Format Tiap Lesson
1. **Learning Objectives** — dari Delinea University
2. **Isi slide** + screenshot
3. **Konsep kunci** dengan analogi
4. **Catatan Senior Engineer** — hal yang tidak dibahas di video (risiko nyata, jebakan implementasi, konteks perbankan & SOC)
5. **Cek Pemahaman**
