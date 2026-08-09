# LENDING CLUB PORTFOLIO ANALYSIS — FINDINGS

## Ringkasan

Analisis ini menguji risiko default pada 10.000 pinjaman Lending Club (sampel kurasi dari 71.454 data) melalui 4 dimensi segmentasi: grade kredit, tujuan pinjaman, status kepemilikan rumah, dan besaran pinjaman. Default didefinisikan sebagai status **Charged Off** atau **Late (31-120 days)** — mencerminkan pinjaman yang sudah masuk fase kolektibilitas bermasalah secara fungsional, bukan hanya write off (WO) final.

**Temuan utama:** Grade kredit adalah prediktor terkuat dan paling konsisten terhadap risiko default (progresif sempurna A→G). Namun dua dimensi lain — tujuan pinjaman dan besaran pinjaman — menunjukkan pola yang tidak sepenuhnya sesuai ekspektasi awal, membuka ruang analisa lebih lanjut.

---

## BQ1: Default Rate by Loan Grade (A–G)

### Hasil
| Grade | Total Loans | Defaulted | Default Rate |
|---|---|---|---|
| A | 2,098 | 53 | 2.53% |
| B | 2,775 | 219 | 7.89% |
| C | 2,820 | 372 | 13.19% |
| D | 1,562 | 305 | 19.53% |
| E | 548 | 168 | 30.66% |
| F | 158 | 59 | 37.34% |
| G | 39 | 16 | 41.03% |

### Interpretasi
Default rate naik **progresif dan monoton** dari grade A ke G — dari 2.53% ke 41.03%, hampir 16x lipat. Ini mengonfirmasi bahwa sistem grading Lending Club bekerja sesuai desain: semakin tinggi grade risiko yang di-assign di awal, semakin tinggi pula realisasi risiko default aktualnya.

### Perspektif Lapangan (komparasi pengalaman selama di WOM Finance)
Grading merupakan langkah mitigasi awal untuk meminimalisir risiko dan mempercepat proses analisa konsumen. Selama menganalisa konsumen after sales, penerapan grading terbukti mampu menekan angka bad debt — sesuai dengan hasil BQ1 di atas.

Salah satu tantangan di lapangan adalah **konsumen banding** — konsumen yang sudah di-reject oleh credit analyst berdasarkan grading, namun kemudian diajukan ulang (banding) oleh tim marketing karena pertimbangan target sales. Konsumen jenis ini sering menjadi problem, baik dari sisi kapasitas bayar aktual maupun karakter kredit.

Grading lebih mudah diterapkan pada konsumen existing (Repeat Order) karena histori pembayaran sudah ada. Untuk konsumen baru, grading tetap bisa diterapkan menggunakan data eksternal seperti SLIK dan Biro Kredit sebagai dasar penilaian riwayat kredit sebelumnya.

### Catatan Data
- Grade G hanya 39 loans (sample kecil) — pattern tetap konsisten tapi perlu disebutkan sebagai limitasi.

---

## BQ2: Default Rate by Loan Purpose

### Hasil
| Purpose Group | Total Loans | Defaulted | Default Rate |
|---|---|---|---|
| Debt Consolidation | 5,759 | 765 | 13.28% |
| Others | 1,912 | 201 | 10.51% |
| Credit Card | 2,329 | 226 | 9.70% |

### Interpretasi
Berbeda dari ekspektasi awal (credit card dianggap segmen berisiko tinggi karena mencerminkan borrower yang terjebak siklus utang kartu kredit), hasil justru menunjukkan **debt consolidation** sebagai segmen berisiko tertinggi, sementara **credit card** justru terendah.

Kemungkinan penjelasan: borrower yang mengajukan pinjaman untuk konsolidasi utang mungkin sudah berada di titik kesulitan finansial yang lebih dalam — proses refinancing ini bisa jadi merupakan usaha terakhir sebelum gagal bayar, bukan solusi yang berhasil menyelesaikan masalah utang mereka.

### Perspektif Lapangan (komparasi pengalaman selama di WOM Finance)
Hal seperti ini banyak terjadi. Umumnya nasabah dalam keadaan mendesak mendapatkan tawaran RO (Repeat Order) dengan kondisi adanya tunggakan di bulan tersebut, sehingga operasional usaha harus mandek karena adanya tagihan usaha yang tidak bisa terealisasi di bulan yang sama. Satu sisi ini strategi marketing yang baik karena melihat peluang kebutuhan konsumen, namun realitanya banyak konsumen yang proses RO dikarenakan kondisi usaha yang sudah memburuk atau menutup hutang di tempat lain (gali lobang tutup lobang) sehingga kemampuan konsumen untuk bayar ke depannya jauh lebih sulit.

### Catatan Data
- Grouping "others" menggabungkan 11 kategori purpose minor (medical, car, house, dll) untuk menghindari sample size terlalu kecil per kategori individual.

---

## BQ3: Home Ownership Impact on Default Risk

### Hasil
| Home Ownership | Total Loans | Defaulted | Default Rate |
|---|---|---|---|
| Rent | 3,929 | 582 | 14.81% |
| Own | 1,207 | 137 | 11.35% |
| Mortgage | 4,864 | 473 | 9.72% |

### Interpretasi
Sesuai ekspektasi: borrower dengan status **RENT** memiliki default rate tertinggi (14.81%), sementara **MORTGAGE** terendah (9.72%) — mengonfirmasi bahwa kepemilikan aset/collateral berkorelasi dengan stabilitas finansial yang lebih baik.

Menariknya, **MORTGAGE** justru lebih rendah dibanding **OWN** (rumah lunas tanpa cicilan) — meski secara intuitif OWN seharusnya paling stabil karena tidak ada beban cicilan rumah. Kemungkinan penjelasan: borrower dengan mortgage sudah melalui proses underwriting bank yang lebih ketat (qualified buyer), sementara status OWN bisa mencakup berbagai skenario (warisan, pembelian tunai tanpa screening kredit ketat, dll) yang tidak seragam kualitas kreditnya.

### Perspektif Lapangan (komparasi pengalaman selama di WOM Finance)
Status kepemilikan rumah menjadi variabel penting dalam analisa kredit. Nasabah dengan KPR (Mortgage) umumnya melalui proses penilaian yang lebih ketat dari sisi perbankan — termasuk perhitungan **DSR (Debt Service Ratio)**. Selama DSR masih dalam batas wajar meski tanggungan angsuran besar, ini justru jadi indikator bahwa nasabah tersebut layak dibiayai (sudah lolos underwriting ketat).

Di sisi lain, kepemilikan rumah pribadi (OWN) bisa berasal dari berbagai sumber — salah satunya warisan. Ini yang membuat status OWN tidak otomatis mencerminkan histori kredit yang baik, meskipun secara kapasitas finansial bisa jadi nilai plus.

---

## BQ4: Loan Amount vs Default Risk

### Hasil
| Loan Amount Bin | Total Loans | Defaulted | Default Rate |
|---|---|---|---|
| Medium ($10k–$20k) | 3,757 | 510 | 13.57% |
| Large (>$20k) | 2,579 | 329 | 12.76% |
| Small (≤$10k) | 3,664 | 353 | 9.63% |

### Interpretasi
Hipotesis awal (pinjaman lebih besar = beban pembayaran lebih berat = risiko default lebih tinggi) tidak sepenuhnya terbukti. Justru segmen **Medium** yang paling berisiko, bukan **Large**.

Kemungkinan penjelasan: pinjaman berjumlah besar (>$20k) kemungkinan melalui proses persetujuan dengan scrutiny lebih tinggi, sehingga borrower yang lolos di kategori ini cenderung memiliki profil kredit lebih kuat. Sementara pinjaman menengah berada di "zona nyaman" persetujuan — cukup besar untuk jadi beban signifikan, tapi tidak cukup besar untuk mendapat pemeriksaan ekstra.

### Perspektif Lapangan (komparasi pengalaman selama di WOM Finance)
Pinjaman skala menengah cenderung lebih rentan karena batas kewenangan approval — jika ditolak oleh credit analyst, keputusan akhir (banding/appeal) di level ini biasanya cukup sampai ke Kepala Cabang saja. Ini membuat proses mitigasi risiko di segmen menengah jadi lebih rentan terhadap kepentingan sales, dibanding pinjaman besar yang biasanya butuh approval di level lebih tinggi dengan scrutiny lebih ketat.

Akibatnya, banyak keputusan kredit di segmen menengah yang lebih mengutamakan pencapaian sales dibanding mitigasi risiko — ini yang kemungkinan menjelaskan kenapa default rate di segmen Medium justru lebih tinggi dari Large.

---

## Kesimpulan & Rekomendasi Bisnis

1. **Grade kredit tetap jadi indikator paling reliable** untuk risk pricing — sistem grading Lending Club valid dan sebaiknya jadi basis utama keputusan underwriting.
2. **Tujuan pinjaman dan besaran pinjaman memerlukan analisis lebih dalam** — pola yang tidak straightforward menunjukkan interaksi dengan variabel lain (kemungkinan perlu cross-analysis: purpose × grade, atau loan amount × grade) untuk memahami root cause.
3. **Survey after sales bisa menjadi indikator tambahan dalam sistem grading** — data lapangan menunjukkan bahwa kondisi aktual nasabah pasca-pencairan sering berbeda dari profil saat pengajuan. Memasukkan data after sales ke dalam model grading bisa membantu mencegah pola yang sama terulang.

---

## Data & Metodologi

- **Dataset:** Lending Club (Kaggle), dikurasi dari 71.454 → 10.000 records
- **Kolom kunci:** grade, purpose, home_ownership, loan_amnt, loan_status
- **Definisi default:** Charged Off + Late (31-120 days)
- **Tools:** SQL (MySQL Workbench)
- **Query lengkap:** lihat folder `queries/`

---

## Next Steps

- Cross-analysis: grade × purpose, grade × loan amount (identifikasi interaksi antar variabel)
- Investigasi lebih dalam segmen counter-intuitive (BQ2, BQ4)
- Window functions: ranking default rate per segmen, running total, dll (skill development)
