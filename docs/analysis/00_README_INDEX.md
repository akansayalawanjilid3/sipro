# SIPRO REBUILD — FRAMEWORK PENGEMBANGAN (baca ini dulu)

> Kumpulan dokumen ini **bukan laporan terpisah** — ini **satu framework** yang saling mengunci: Analisis (WHY) → Blueprint (WHAT) → Plan+Guardrail (HOW). Tiap dokumen menautkan ke yang lain. Perintah owner: *analisis mendalam & saling bersinergi; plan detail (cara implementasi + gambaran produk); analisis sebagai acuan root-cause.*

## 1. TIGA LAPISAN FRAMEWORK & ALIRANNYA
```
        WHY (root-cause / kebutuhan)              WHAT (produk & arsitektur)            HOW (eksekusi & mutu)
  ┌──────────────────────────────────┐     ┌──────────────────────────────┐    ┌──────────────────────────────┐
  │ 01 Repo & Feasibility            │     │ 03 IA + Work Hub + Domain     │    │ 04 Roadmap: Fondasi/Foundations│
  │ 02 Business Process + Benchmark  │ ──▶ │ 07 Omnichannel Lead Engine    │──▶ │    EPIC + Guardrail + DoD       │
  │ 05 Pain Points (outcome-driven)  │     │ 08 §Gambaran Produk           │    │ 08 §Implementasi (detail)      │
  │ 06 Personas/JTBD + Prod Benchmark│     │ (domain model kanonik)        │    │ plan.md (fase berjalan)        │
  └──────────────────────────────────┘     └──────────────────────────────┘    └──────────────────────────────┘
        ▲ dipakai sebagai acuan bila butuh detail root-cause  ◀───────────────── referensi silang
```
- **Butuh alasan/akar masalah?** → 01/02/05/06/07.
- **Butuh gambaran produk / arsitektur?** → 03 + 08 (§Gambaran Produk).
- **Butuh cara eksekusi / urutan / mutu?** → 04 + 08 (§Implementasi) + plan.md.

## 2. DAFTAR DOKUMEN (urutan baca)
| # | Dokumen | Peran dalam framework |
|---|---|---|
| 01 | REPO_ANALYSIS_AND_FEASIBILITY | Fakta SIPROnext (aset & 9 kegagalan), pola `kn`, clone vs rebuild, kelayakan |
| 02 | BUSINESS_PROCESS_AND_BENCHMARK | Bisnis-proses properti ID end-to-end + benchmark sistem matang + peta gap |
| 05 | PAIN_POINTS_AND_BUSINESS_NEEDS | **Outcome-driven**: Pain→Akar→Dampak→Solusi→KPI (Sales/Konstruksi/Finance/Trust) |
| 06 | USER_PERSONAS_AND_PRODUCT_BENCHMARK | 6 persona + JTBD (termasuk Pembeli) + benchmark produk PMS/PM + KPI |
| 07 | OMNICHANNEL_LEAD_ENGINE | WA in-chat inbox + Ads lead capture + conversational triggers → lifecycle |
| 03 | IA_WORKHUB_UX_BLUEPRINT | IA flow-based, Role-Home, **Work Hub "Slack tapi ERP"**, Process Timeline, domain model |
| 04 | MASTER_ROADMAP | Fase 0 Fondasi, Foundations F1–F10, EPIC + dependency graph, "lebih baik dari kn", DoD, governance |
| 08 | MASTER_PLAN_AND_PRODUCT_VISION | **Plan detail**: per-EPIC → root-cause ref + gambaran produk + implementasi + guardrail + DoD |

## 3. EMPAT OUTCOME BISNIS (north-star penilai semua fitur)
1. **Tingkatkan konversi** · 2. **Cegah kebocoran** · 3. **Jaga deadline** · 4. **Amankan arus kas & kepercayaan**.

## 4. TRACEABILITY MATRIX (pengikat utama — dari kebutuhan sampai gate)
> Baca baris kiri→kanan: kebutuhan bisnis mengalir jadi kode & gate. Inilah "sinergi" antar-dokumen.

| Outcome | Pain (05/07) | Kapabilitas produk (03/07/08) | EPIC (08/04) | Entity utama (03§6 / 07§E) | Guardrail (04§1.2) |
|---|---|---|---|---|---|
| Konversi ↑ | SL-1 respons lambat | Ads capture <30dtk + auto-assign + task 5-mnt | 1.7 | `lead_capture_events`, `leads`, `tasks` | signature webhook, verify_rbac |
| Konversi ↑ | SL-2 lead bocor | WA inbox in-app + conversational trigger→stage | 1.7 | `conversations`,`messages`,`automation_rules` | verify_tenant_scope, ux_audit |
| Konversi ↑ | SL-3 double booking | Unit atomic hold + status real-time | 1.3, 2.0 | `units`,`reservations`,`deals` | verify_data_integrity |
| Konversi ↑ | SL-4 KPR ditolak | Pra-skrining SLIK/DP + financing multi-bank | 1.5 | `financing_apps` | verify_referential_integrity |
| Konversi ↑ | SL-5 sistem pasif | Work Hub Task Inbox + NBA + guided flow | 1.0 | `tasks`,`activities`,`events` | ux_audit, check_nav_map |
| Konversi ↑ | SL-6 dispute komisi | Komisi engine transparan | 1.6 | `commissions` | verify_data_integrity |
| Cegah kebocoran | Material dicuri/over-order | Material ledger + opname + MRP + 3-way match | 2.6 | `materials`,`material_txns`,`stock_opname` | audit trail, verify_data_integrity |
| Cegah kebocoran | Fraud keuangan | Approval berjenjang + 3-way match + audit + RBAC | 3.6 | `ap_bills`,`audit_logs` | verify_rbac |
| Jaga deadline | Perizinan/dokumen lambat | Permit/Doc tracker + reminder + eskalasi | 2.7 | `permits`,`documents`,`tasks` | health_check |
| Jaga deadline | Deviasi jadwal telat ketahuan | Kurva-S plan vs aktual + task korektif | 2.3 | `construction_units`,`progress_claims` | verify_data_integrity |
| Jaga deadline | Cacat mutu/rework | QC/punch list + foto ber-lokasi | 2.4, 2.8 | `qc_inspections`,`attachments` | ux_audit |
| Arus kas & trust | Penagihan macet | AR aging + collection worklist + cash-flow projection | 3.0, 3.5 | `ar_schedules`,`receipts` | verify_data_integrity |
| Arus kas & trust | Subcon telat / retensi | AP + retensi + jadwal bayar | 3.1 | `ap_bills`,`retentions`,`payments_out` | verify_data_integrity |
| Arus kas & trust | RevRec salah (PSAK 72) | Contract-liability + RevRec @ BAST | 3.2 | `contract_liabilities`,`revenue_recognitions` | verify_data_integrity |
| Arus kas & trust | Pembeli tak percaya | Customer Portal (progres/bayar/dok/komplain SLA) | M1 | `documents`,`ar_schedules`,`construction_units` | verify_rbac |

## 5. FONDASI LINTAS-SEKTOR (build once → dipakai semua EPIC) — detail Dok 04 §2
`F1 RBAC` · `F2 Multi-tenant scope` · `F3 Event Bus` · `F4 Guided Work Engine` · `F5 Activity/Collaboration` · `F6 Document engine` · `F7 Object storage` · `F8 Finance primitives` · `F9 Process Timeline` · `F10 Notification`.

## 6. VERDIKT & STATUS
Rebuild **layak & bernilai**. Kegagalan SIPRO lama = **fondasi & eksekusi**, bukan konsep. Rencana: Fondasi ala `kn` (ditingkatkan: RBAC + multi-tenant + event bus + Guided Work Engine + Activity + Omnichannel + design modern-SaaS) → Pilar Sales/CRM + Work Hub + Omnichannel → Konstruksi/Subcon → Finance → Pematangan (Customer Portal, WA/CAPI penuh, real-time, GL, multi-tenant UI).

- [x] 01–02 Analisis repo/kelayakan + bisnis-proses/benchmark
- [x] 05–06 Pain point outcome-driven + persona/JTBD + benchmark produk
- [x] 07 Omnichannel (WA in-chat + Ads capture + trigger lifecycle)
- [x] 03 Blueprint IA/Work Hub + domain model
- [x] 04 Roadmap + Foundations + guardrail plan
- [x] 08 Plan detail (implementasi + gambaran produk) + traceability
- [ ] **ACC owner** → mulai Fase 0 (Fondasi)
