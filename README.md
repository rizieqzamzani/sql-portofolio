# Lending Club Loan Default Analysis

## Overview
Analisis risiko default pada 10.000 pinjaman Lending Club melalui 4 dimensi segmentasi menggunakan SQL. Project ini mengombinasikan analisa data dengan perspektif lapangan dari pengalaman sebagai Branch Collection Head di perusahaan multifinance.

## Background
Sebagai mantan Branch Collection Head di WOM Finance, saya menangani proses penagihan dan analisa kredit after sales — mulai dari grading konsumen, proses banding (appeal), hingga pengelolaan akun bermasalah. Project ini bertujuan memvalidasi pola-pola risiko kredit yang saya temui di lapangan menggunakan data publik Lending Club.

## Dataset
- **Source:** [Lending Club — Kaggle](https://www.kaggle.com/datasets/wordsforthewise/lending-club)
- **Sample:** 10.000 loans (kurasi dari 71.454 records)
- **Columns:** 9 variabel kunci (grade, purpose, home_ownership, loan_amnt, loan_status, term, int_rate, annual_inc, id)
- **Default definition:** Charged Off + Late (31-120 days)

## Business Questions

| BQ | Question | Key Finding |
|---|---|---|
| **BQ1** | Default rate per loan grade (A–G)? | Progresif sempurna: A (2.53%) → G (41.03%), **16x lipat** |
| **BQ2** | Purpose mana yang paling sering default? | Debt consolidation (13.28%) tertinggi — bukan credit card |
| **BQ3** | Apakah home ownership mempengaruhi default risk? | RENT (14.81%) > OWN (11.35%) > MORTGAGE (9.72%) |
| **BQ4** | Apakah loan amount besar = default lebih tinggi? | Medium ($10-20k) justru tertinggi (13.57%), bukan Large |

## Key Insights

**1. Grade kredit = prediktor terkuat.** Default rate naik monoton dari A ke G tanpa anomali — sistem grading Lending Club terbukti valid sebagai basis risk pricing.

**2. Dua temuan counter-intuitive:**
- **BQ2:** Debt consolidation lebih berisiko dari credit card — borrower yang refinancing kemungkinan sudah di titik kesulitan finansial lebih dalam ("gali lobang tutup lobang")
- **BQ4:** Pinjaman menengah ($10-20k) lebih berisiko dari pinjaman besar (>$20k) — kemungkinan karena perbedaan level scrutiny dan kewenangan approval

**3. Home ownership sebagai proxy stabilitas finansial** terkonfirmasi — tapi MORTGAGE lebih aman dari OWN, kemungkinan karena borrower KPR sudah lolos underwriting bank yang lebih ketat (DSR screening).

## Repo Structure

```
sql-portofolio/
├── README.md              ← anda di sini
├── queries/
│   ├── 01_default_rate_by_grade.sql
│   ├── 02_default_rate_by_purpose.sql
│   ├── 03_home_ownership_impact.sql
│   └── 04_loan_amount_vs_default.sql
├── results/
│   ├── 01_default_rate_by_grade.csv
│   ├── 02_default_rate_by_purpose.csv
│   ├── 03_home_ownership_impact.csv
│   └── 04_loan_amount_vs_default.csv
├── data/
│   └── lending_club_10k.csv
└── docs/
    └── findings.md        ← analisis lengkap + perspektif lapangan
```

## Tools
- SQL (MySQL Workbench)
- Dataset: Lending Club (Kaggle)

## About
**Muhamad Rizieq Zamzani** — Economics graduate (GPA 3.73/4.0), ex-Branch Collection Head at WOM Finance. Transitioning to Data Analyst / Credit Risk Analyst role. Domain expertise in credit risk, collections, grading, and default behavior.

- GitHub: [github.com/rizieqzamzani](https://github.com/rizieqzamzani)
- LinkedIn: [linkedin.com/in/rizieqzamzani](https://linkedin.com/in/rizieqzamzani)
