# JCBDAAH-006_Beta
# Hotel Reservation Cancellation Analysis

> Final Project — Business & Data Analyst Bootcamp

Project ini dibuat untuk menganalisis pola pembatalan reservasi pada **City Hotel** dan **Resort Hotel**. Fokus utamanya adalah mencari tahu booking seperti apa yang lebih sering dibatalkan, seberapa besar nilai reservasi yang terdampak, dan rekomendasi apa yang bisa diberikan dari hasil analisis.

---

## Daftar Isi

- [Project Overview](#project-overview)
- [Business Problem](#business-problem)
- [Objectives](#objectives)
- [Stakeholder](#stakeholder)
- [Dataset](#dataset)
- [Tools](#tools)
- [Workflow](#workflow)
- [Data Cleaning](#data-cleaning)
- [Feature Engineering](#feature-engineering)
- [Analysis](#analysis)
- [Key Findings](#key-findings)
- [Statistical Testing](#statistical-testing)
- [Business Scenario](#business-scenario)
- [Business Recommendations](#business-recommendations)
- [Business Decision Flow](#business-decision-flow)
- [Dashboard](#dashboard)
- [How to Run](#how-to-run)
- [Limitations](#limitations)
- [Future Work](#future-work)
- [Dataset Source](#dataset-source)

---

## Project Overview

Hotel memiliki jumlah booking yang cukup besar, tetapi tidak semua booking akhirnya menjadi actual stay. Salah satu masalah yang perlu diperhatikan adalah **cancellation**.

Pada project ini, kami menggunakan data reservasi hotel untuk melihat pola cancellation berdasarkan beberapa faktor, terutama:

- Hotel type
- Lead time
- Market segment
- Deposit type
- Potential room value

Selain analisis deskriptif, kami juga melakukan data cleaning, feature engineering, dan statistical testing untuk melihat apakah pola yang ditemukan di data cukup kuat untuk dijadikan bahan pertimbangan bisnis.

### Hasil Singkat

| Metric | Result |
|---|---:|
| Initial bookings | 119,390 |
| Final bookings | 87,395 |
| Cancelled bookings | 24,025 |
| Cancellation rate | **27.49%** |
| Cancelled potential room value | **≈ 11.48 juta** |

---

## Business Problem

Booking yang tercatat belum tentu menghasilkan actual stay karena sebagian reservasi dapat dibatalkan.

Hal ini bisa mempengaruhi:

- occupancy planning
- room availability
- revenue planning
- reservation management
- channel management

Karena itu, kami ingin melihat:

1. Booking seperti apa yang memiliki cancellation rate lebih tinggi?
2. Apakah lead time berhubungan dengan cancellation?
3. Segmen mana yang perlu mendapat perhatian lebih?
4. Berapa potential room value yang berasal dari cancelled booking?
5. Apa strategi yang masuk akal berdasarkan hasil analisis?

---

## Objectives

### 1. Melihat pola cancellation

Membandingkan cancellation berdasarkan:

- City Hotel vs Resort Hotel
- Lead time
- Market segment
- Deposit type

### 2. Mengukur potential room value

Menghitung nilai reservasi menggunakan:

```text
Potential Room Value = ADR × Total Stay Duration
```

Nilai ini digunakan sebagai **proxy nilai reservasi**, bukan sebagai actual lost revenue.

### 3. Melakukan statistical testing

Statistical testing digunakan untuk melihat apakah terdapat association atau perbedaan distribusi antara variabel tertentu dengan cancellation.

### 4. Memberikan rekomendasi bisnis

Hasil analisis digunakan untuk menentukan area yang bisa diprioritaskan dan beberapa ide yang dapat diuji melalui pilot.

---

## Stakeholder

Stakeholder utama yang menurut kami paling relevan dengan hasil analisis ini adalah:

### Revenue / Hotel Management

Membutuhkan informasi mengenai:

- cancellation rate
- kelompok booking dengan cancellation tinggi
- potential room value yang terdampak
- perbedaan pola antar hotel type

### Reservation Team

Dapat menggunakan hasil analisis untuk:

- monitoring booking
- reconfirmation
- reminder
- follow-up booking tertentu

### Marketing / Sales Team

Terutama untuk melihat performa dan pola cancellation berdasarkan market segment atau channel.

---

## Dataset

Dataset yang digunakan adalah **Hotel Booking Demand**.

Data berisi reservasi dari:

- City Hotel
- Resort Hotel

dengan periode data **2015–2017**.

### Dataset Awal

```text
119,390 rows
32 columns
```

### Beberapa Kolom yang Digunakan

| Column | Description |
|---|---|
| `hotel` | Tipe hotel |
| `is_canceled` | Status cancellation |
| `lead_time` | Jarak hari antara booking dan tanggal kedatangan |
| `market_segment` | Segmen pasar |
| `distribution_channel` | Channel distribusi |
| `deposit_type` | Jenis deposit |
| `adr` | Average Daily Rate |
| `stays_in_weekend_nights` | Jumlah malam weekend |
| `stays_in_week_nights` | Jumlah malam weekday |
| `adults` | Jumlah tamu dewasa |
| `children` | Jumlah anak |
| `babies` | Jumlah bayi |
| `reserved_room_type` | Tipe kamar yang dipesan |
| `assigned_room_type` | Tipe kamar yang diberikan |
| `booking_changes` | Jumlah perubahan booking |
| `agent` | ID travel agent |
| `company` | ID perusahaan |
| `reservation_status` | Status akhir reservasi |

---

## Tools

Project dikerjakan menggunakan Python dan Jupyter Notebook.

### Libraries

```python
pandas
numpy
matplotlib
seaborn
scipy
```

| Tool / Library | Usage |
|---|---|
| Python | Analisis data |
| Pandas | Data cleaning dan data manipulation |
| NumPy | Operasi numerik |
| Matplotlib | Visualisasi |
| Seaborn | Visualisasi |
| SciPy | Statistical testing |
| Looker Studio | Dashboard |

---

## Workflow

```text
Business Understanding
        ↓
Data Understanding
        ↓
Data Cleaning
        ↓
Feature Engineering
        ↓
Exploratory Data Analysis
        ↓
Statistical Testing
        ↓
Business Scenario
        ↓
Business Recommendation
        ↓
Dashboard
```

---

# Data Cleaning

Sebelum melakukan analisis, kami melakukan pengecekan terhadap missing value, duplicate, anomaly, outlier, dan datatype.

## 1. Missing Value

| Column | Missing | Treatment |
|---|---:|---|
| `company` | 112,593 | `Unknown` |
| `agent` | 16,340 | `Unknown` |
| `country` | 488 | `Unknown` |
| `children` | 4 | `0` |

Untuk `company`, `agent`, dan `country`, kmi menggunakan `Unknown` karena tidak ada informasi yang cukup untuk menentukan nilai sebenarnya.

Untuk `children`, missing value diisi `0` karena jumlah missing sangat kecil dan kolom tersebut menunjukkan jumlah anak dalam booking.

## 2. Duplicate

Ditemukan:

```text
31,994 duplicate rows
```

Duplicate yang identik kemudian dihapus.

```text
119,390 rows
      ↓
87,396 rows
```

## 3. Negative ADR

Ditemukan satu observasi dengan:

```text
ADR = -6.38
```

Karena ADR negatif tidak masuk akal dalam konteks harga kamar, observasi tersebut dihapus.

Hasil akhir:

```text
87,395 rows
```

## 4. Booking dengan 0 Guest

Ditemukan booking dengan:

```text
adults = 0
children = 0
babies = 0
```

Data tersebut tidak langsung dihapus karena belum ada cukup bukti bahwa seluruh observasi merupakan error.

## 5. Booking dengan 0 Nights

Terdapat booking dengan:

```text
stays_in_weekend_nights = 0
stays_in_week_nights = 0
```

Data tetap dipertahankan karena kondisi tersebut masih mungkin muncul dalam data reservasi.

## 6. Room Type Mismatch

Kami juga mengecek kondisi:

```python
reserved_room_type != assigned_room_type
```

Perbedaan tersebut tidak langsung dianggap sebagai data error karena dapat terjadi akibat upgrade, perubahan booking, atau proses room assignment.

## 7. Outlier

Outlier pada `adr` dan `lead_time` dicek menggunakan metode IQR.

```text
ADR outlier candidates       : ±3,793
Lead time outlier candidates : ±3,005
```

Outlier tersebut tidak otomatis dihapus karena nilai yang tinggi masih bisa merupakan booking yang valid.

## 8. Datatype

Beberapa datatype disesuaikan, termasuk:

```python
reservation_status_date → datetime
children → integer
```

### Final Cleaning Check

```text
Final Shape      : 87,395 × 32
Missing Values   : 0
Duplicate Rows   : 0
Negative ADR     : 0
```

---

# Feature Engineering

## 1. Total Stay Duration

```python
total_stay_duration = (
    stays_in_weekend_nights +
    stays_in_week_nights
)
```

Digunakan untuk mengetahui total lama menginap dan menghitung potential room value.

## 2. Total Guests

```python
total_guests = (
    adults +
    children +
    babies
)
```

Digunakan untuk mendapatkan jumlah total tamu dalam satu booking.

## 3. Potential Room Value

```python
potential_room_value = adr * total_stay_duration
```

Feature ini digunakan sebagai pendekatan untuk melihat nilai reservasi yang berpotensi terdampak ketika booking canceled.

> **Catatan:** Potential Room Value bukan actual lost revenue. Nilai ini belum memperhitungkan kemungkinan kamar terjual kembali, perubahan harga, diskon, pajak, dan faktor lainnya.

---

# Analysis

## Cancellation Rate

Metric utama yang digunakan:

```text
Cancellation Rate =
Cancelled Bookings / Total Bookings × 100%
```

### Lead Time Group

| Group | Range |
|---|---|
| 0–7 | 0–7 hari |
| 8–30 | 8–30 hari |
| 31–90 | 31–90 hari |
| 91–180 | 91–180 hari |
| 181–365 | 181–365 hari |
| 366+ | >365 hari |

---

# Key Findings

## 1. Overall Cancellation

| Status | Bookings | Percentage |
|---|---:|---:|
| Not Canceled | 63,370 | 72.51% |
| Canceled | 24,025 | **27.49%** |

Sekitar **27.49% booking** dalam data final berstatus canceled.

---

## 2. City Hotel Memiliki Cancellation Lebih Tinggi

| Hotel | Total Booking | Canceled | Cancellation Rate |
|---|---:|---:|---:|
| City Hotel | 53,428 | 16,049 | **30.04%** |
| Resort Hotel | 33,967 | 7,976 | **23.48%** |

Cancellation rate City Hotel sekitar **6.56 percentage points** lebih tinggi dibanding Resort Hotel.

---

## 3. Semakin Panjang Lead Time, Cancellation Cenderung Semakin Tinggi

| Lead Time | Total Booking | Canceled | Cancellation Rate |
|---|---:|---:|---:|
| 0–7 | 18,304 | 1,543 | **8.43%** |
| 8–30 | 16,340 | 4,145 | **25.37%** |
| 31–90 | 22,744 | 7,280 | **32.01%** |
| 91–180 | 18,243 | 6,382 | **34.98%** |
| 181–365 | 11,199 | 4,444 | **39.68%** |
| 366+ | 565 | 231 | **40.88%** |

Cancellation rate meningkat ketika lead time semakin panjang.

Median lead time:

| Status | Median Lead Time |
|---|---:|
| Not Canceled | 38 hari |
| Canceled | **80 hari** |

Booking yang canceled memiliki median lead time lebih dari dua kali booking yang tidak canceled.

---

## 4. City Hotel + Long Lead Time

Cancellation rate tertinggi ditemukan pada:

```text
City Hotel + Lead Time 366+
= 51.33%
```

City Hotel juga menunjukkan cancellation rate yang lebih tinggi dibanding Resort Hotel pada hampir seluruh kelompok lead time.

Booking dengan lead time panjang menjadi salah satu kelompok yang menarik untuk dimonitor lebih lanjut.

---

## 5. Online TA Menjadi Segmen yang Perlu Diperhatikan

| Market Segment | Total Booking | Canceled | Cancellation Rate |
|---|---:|---:|---:|
| Online TA | 51,618 | 18,245 | **35.35%** |
| Groups | 4,941 | 1,335 | 27.02% |
| Aviation | 227 | 45 | 19.82% |
| Offline TA/TO | 13,889 | 2,063 | 14.85% |
| Direct | 11,804 | 1,737 | 14.72% |
| Complementary | 702 | 88 | 12.54% |
| Corporate | 4,212 | 510 | 12.11% |

Online TA memiliki:

```text
51,618 booking
= 59.06% dari seluruh booking
```

dan:

```text
18,245 cancellation
= 75.94% dari seluruh cancellation
```

Dengan cancellation rate **35.35%**, Online TA menjadi salah satu area yang perlu dianalisis lebih lanjut.

Namun, hasil ini tidak berarti Online TA harus langsung dikurangi karena segmen ini juga memiliki volume booking yang besar.

---

## 6. Deposit Type Menunjukkan Pola yang Perlu Dicek Lagi

| Deposit Type | Total Booking | Canceled | Cancellation Rate |
|---|---:|---:|---:|
| Non Refund | 1,038 | 983 | **94.70%** |
| No Deposit | 86,250 | 23,016 | 26.69% |
| Refundable | 107 | 26 | 24.30% |

Sekitar **95.80% cancellation** berasal dari booking `No Deposit`.

Di sisi lain, `Non Refund` memiliki cancellation rate **94.70%**, yang cukup tidak biasa.

Karena itu, hasil ini tidak langsung digunakan sebagai dasar untuk mengubah deposit policy. Data perlu dicek lebih lanjut berdasarkan agent, market segment, country, dan lead time.

---

## 7. Potential Room Value

Total potential room value:

```text
≈ 34.46 juta
```

Potential room value dari cancelled booking:

```text
≈ 11.48 juta
```

Proporsinya:

```text
≈ 33.32%
```

Sementara cancelled booking sekitar:

```text
27.49%
```

dari seluruh booking.

Artinya, cancelled booking menyumbang proporsi potential room value yang lebih besar dibanding proporsi jumlah bookingnya.

### Cancelled Potential Room Value berdasarkan Lead Time

| Lead Time | Share of Cancelled Potential Value |
|---|---:|
| 0–7 | 3.35% |
| 8–30 | 15.39% |
| 31–90 | 28.83% |
| 91–180 | **30.15%** |
| 181–365 | 21.82% |
| 366+ | 0.46% |

Kelompok **91–180 hari** memberikan kontribusi terbesar terhadap cancelled potential room value.

Booking dengan lead time **lebih dari 30 hari** secara keseluruhan menyumbang sekitar:

- **76.32%** dari seluruh cancellation
- **81.26%** dari cancelled potential room value

---

# Statistical Testing

Kami menggunakan statistical testing untuk melihat apakah pola dari EDA juga menunjukkan evidence secara statistik.

## Chi-Square Test + Cramér's V

Untuk variabel kategorikal:

- Chi-Square Test of Independence
- Cramér's V

| Variable | Cramér's V | Interpretation |
|---|---:|---|
| `hotel` | 0.0716 | Relatively weak association |
| `market_segment` | **0.2210** | Paling kuat dari variabel kategorikal yang diuji |
| `deposit_type` | 0.1651 | Lebih kuat dibanding `hotel` |

Ketiga variabel memiliki:

```text
p-value < 0.001
```

Jadi terdapat evidence adanya association dengan `is_canceled`.

> **Association tidak sama dengan causation.**

Hasil ini digunakan untuk melihat pola dan menentukan area yang perlu dianalisis lebih lanjut.

## Mann–Whitney U Test

Test ini digunakan untuk membandingkan distribusi:

- `lead_time`
- `adr`

antara booking canceled dan not canceled.

| Variable | Not Canceled | Canceled |
|---|---:|---:|
| Lead Time | 38 | **80** |
| ADR | 94.5 | **109.8** |

Kedua pengujian menghasilkan:

```text
p-value < 0.001
```

Artinya terdapat perbedaan distribusi yang signifikan antara kelompok canceled dan not canceled.

---

# Business Scenario

Baseline:

```text
Cancellation Rate = 27.49%
```

Target scenario:

```text
Cancellation Rate ≤ 25.49%
```

atau turun **2 percentage points**.

| Scenario | Approx. Additional Bookings Not Canceled | Illustrative Potential Value Preserved |
|---|---:|---:|
| Turun 1 percentage point | ±874 | ±417,751 |
| Turun 2 percentage points | **±1,748** | **±835,503** |

Angka ini digunakan untuk memberikan gambaran potential impact berdasarkan historical data.

**Ini bukan prediksi bahwa strategi pasti akan menghasilkan nilai tersebut.**

---

# Business Recommendations

## 1. Proactive Confirmation untuk Long-Lead Booking

**Priority: P1**

Target awal:

```text
Lead Time >30 hari
```

Fokus:

```text
91–365 hari
City Hotel
```

Alasannya:

- long lead time menyumbang sebagian besar cancellation;
- kelompok tersebut juga menyumbang sebagian besar cancelled potential room value;
- masih ada waktu sebelum check-in untuk melakukan follow-up.

### Suggested Tier

| Tier | Lead Time | Cancellation Rate | Initial Action |
|---|---|---:|---|
| A | 31–90 hari | 32.01% | Reminder / reconfirmation |
| B | 91–365 hari | 36.77% | Reconfirmation lebih aktif |
| C | 366+ hari | 40.88% | Watchlist / manual review |

Waktu reminder masih merupakan **hipotesis awal** dan perlu diuji melalui pilot.

### KPI

**Primary**

- cancellation rate booking >30 hari

**Supporting**

- confirmation response rate
- early cancellation notice
- cancelled potential room value
- contact cost per booking

**Guardrail**

- no-show rate
- complaint rate
- opt-out rate

---

## 2. Drill-down pada Online TA

**Priority: P1**

Online TA menjadi salah satu area prioritas karena:

- 59.06% seluruh booking
- 75.94% seluruh cancellation
- cancellation rate 35.35%
- Cramér's V paling tinggi di antara variabel kategorikal yang diuji

Drill-down yang disarankan:

```text
Online TA
   ×
Lead Time
   ×
Hotel Type
   ×
Agent
   ×
Country
```

Tujuannya untuk melihat apakah cancellation terkonsentrasi pada agent, country, hotel type, atau lead time tertentu.

Online TA tidak sebaiknya langsung dikurangi hanya berdasarkan hasil ini karena kontribusinya terhadap booking juga cukup besar.

---

## 3. Risk × Potential Room Value

**Priority: P2**

Cancellation rate dapat digunakan untuk melihat risiko, sedangkan potential room value dapat digunakan untuk menentukan prioritas berdasarkan nilai booking.

| | High Value | Low Value |
|---|---|---|
| High Risk | Personal reconfirmation | Automated reminder |
| Low Risk | Standard monitoring | Existing process |

Dengan pendekatan ini, effort bisa lebih fokus pada booking yang memiliki kombinasi risiko dan nilai yang lebih tinggi.

---

## 4. Audit Deposit Policy

**Priority: P3**

Pola `Non Refund` cukup tidak biasa sehingga sebaiknya dilakukan audit terlebih dahulu.

### Audit

Cek berdasarkan:

- market segment
- agent
- country
- lead time
- payment information

### Pilot

Jika hasil audit mendukung, perubahan policy bisa diuji secara terbatas, misalnya:

- partial deposit
- semi-flexible rate
- treatment tertentu untuk long-lead booking

KPI:

- cancellation rate
- conversion rate
- revenue per realized booking

---

# Business Decision Flow

Framework yang digunakan:

```text
IDENTIFY
   ↓
PRIORITIZE
   ↓
ACT
   ↓
MONITOR
   ↓
EVALUATE
```

### Identify

Cari kelompok dengan:

- cancellation rate tinggi
- booking volume besar
- potential room value tinggi

### Prioritize

Fokus pada:

```text
High Risk + High Value
```

### Act

Contoh action:

- proactive confirmation
- Online TA drill-down
- risk × value tiering
- deposit audit

### Monitor

Primary KPI:

```text
Cancellation Rate
```

Supporting metrics:

- cancelled booking volume
- potential room value exposure
- early cancellation notice

### Evaluate

Gunakan hasil pilot untuk menentukan:

```text
Scale / Adjust / Stop
```

---

# Roadmap Implementasi

| Phase | Timeline | Activity |
|---|---|---|
| Preparation | Month 1 | Baseline, deposit audit, Online TA drill-down, KPI setup |
| Pilot | Month 2–4 | Proactive confirmation dan treatment terbatas |
| Evaluation | Month 5–6 | Membandingkan treatment dan control |
| Scale | >6 months | Memperluas strategi jika hasil pilot sesuai target |

---

# Dashboard

Dashboard dibuat menggunakan **Looker Studio**.

### KPI

- Total Booking
- Cancelled Booking
- Cancellation Rate
- Cancelled Potential Room Value

### Main Charts

- Cancellation Rate by Lead Time Group
- Cancelled Potential Room Value by Lead Time Group
- Cancellation Rate by Market Segment
- Cancelled Bookings by Market Segment
- Cancellation Rate by Hotel Type

### Filters

- Hotel Type
- Lead Time Group
- Market Segment
- Deposit Type

Dashboard dibuat untuk membantu menjawab:

1. Seberapa besar masalah cancellation?
2. Cancellation paling banyak terjadi pada kelompok mana?
3. Booking seperti apa yang perlu dimonitor lebih lanjut?

> Link dashboard dapat ditambahkan di repository GitHub.

---

# How to Run

## 1. Clone Repository

```bash
git clone <URL_REPOSITORY_KAMU>
cd hotel-reservation-cancellation-analysis
```

Atau download repository dalam bentuk ZIP.

## 2. Download Dataset

Download `hotel_bookings.csv` dari sumber dataset pada bagian [Dataset Source](#dataset-source).

Letakkan file CSV di folder project.

## 3. Install Libraries

```bash
pip install pandas numpy matplotlib seaborn scipy jupyter
```

Jika ingin menggunakan virtual environment:

```bash
python -m venv .venv
```

Windows:

```bash
.venv\Scriptsctivate
```

macOS/Linux:

```bash
source .venv/bin/activate
```

Kemudian install dependency:

```bash
pip install pandas numpy matplotlib seaborn scipy jupyter
```

## 4. Sesuaikan Dataset Path

Contoh:

```python
df_hotel = pd.read_csv('hotel_bookings.csv')
```

## 5. Jalankan Notebook

```bash
jupyter notebook
```

Kemudian buka:

```text
BETA_Final_Project_Hotel_Reservation_FINAL.ipynb
```

dan jalankan semua cell.

---

# Limitations

Beberapa keterbatasan project:

1. Dataset tidak memberikan gambaran lengkap mengenai kondisi operasional hotel.
2. Statistical testing menunjukkan **association / difference**, bukan causal relationship.
3. Potential Room Value merupakan proxy dan bukan actual revenue loss.
4. Beberapa kategori memiliki jumlah booking yang kecil sehingga perlu diinterpretasikan dengan hati-hati.
5. Dataset tidak memiliki informasi seperti:
   - alasan cancellation
   - actual revenue setelah cancellation
   - resale rate
   - biaya komunikasi
   - response customer terhadap reminder
6. Analisis statistik masih bersifat univariat dan belum mengontrol confounding antarvariabel.
7. Rekomendasi belum diuji langsung melalui eksperimen operasional.

---

# Future Work

Jika project ini dikembangkan lebih lanjut:

### 1. Multivariate Analysis

Menggunakan:

```text
Logistic Regression
```

untuk melihat beberapa variabel secara bersamaan.

### 2. Cancellation Prediction

Membangun model untuk memprediksi probability cancellation pada level booking.

### 3. Cancellation Reason

Menambahkan alasan cancellation untuk membedakan cancellation yang mungkin dapat dicegah dan yang tidak.

### 4. Actual Revenue Analysis

Menambahkan:

- actual revenue
- resale after cancellation
- final selling price
- discount
- operational cost

### 5. Controlled Experiment

Menguji proactive confirmation menggunakan:

```text
A/B Test / Controlled Pilot
```

### 6. Time Series Analysis

Melihat:

- trend
- seasonality
- perubahan cancellation setelah intervensi

### 7. Dashboard Development

Dashboard dapat dikembangkan menggunakan:

- Looker Studio
- Tableau
- Power BI

---

# Dataset Source

Dataset yang digunakan:

**Hotel Booking Demand**

Kaggle:

https://www.kaggle.com/datasets/jessemostipak/hotel-booking-demand/data

Dataset mencakup reservasi **City Hotel** dan **Resort Hotel** pada periode 2015–2017.

---

# Project Summary

```text
119,390 bookings
        ↓
Data Cleaning
        ↓
87,395 bookings
        ↓
27.49% cancellation rate
        ↓
Long Lead Time + Online TA
menjadi area yang perlu diperhatikan
        ↓
≈ 11.48 juta cancelled potential room value
        ↓
Target scenario:
cancellation ≤ 25.49%
        ↓
Business Recommendations
        ↓
Dashboard
```

## Key Takeaways

- **Cancellation rate berada di 27.49%** setelah data cleaning.
- **City Hotel** memiliki cancellation rate lebih tinggi dibanding Resort Hotel.
- Cancellation rate meningkat seiring semakin panjangnya **lead time**.
- **Online TA** memiliki kombinasi volume booking dan cancellation rate yang cukup tinggi.
- Cancelled booking menyumbang sekitar **33.32% potential room value**, lebih tinggi dari proporsi jumlah cancelled booking.
- `Non Refund` menunjukkan pola yang tidak biasa dan sebaiknya diaudit terlebih dahulu.
- Rekomendasi utama adalah **proactive confirmation untuk long-lead booking**, tetapi efektivitasnya tetap perlu dibuktikan melalui pilot.

---

## Status Project

**Final Project — Completed**

Project mencakup data cleaning, exploratory analysis, statistical testing, business scenario, business recommendation, dan dashboard.

Pengembangan selanjutnya:

```text
Controlled Pilot
      ↓
Multivariate Analysis
      ↓
Predictive Modeling
```
