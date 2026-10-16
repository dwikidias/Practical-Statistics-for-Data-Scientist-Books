# Practical Statistics for Data Scientists (O'Reilly) - Code Reproduction & Theory Deep-Dive

* **Buku Referensi:** *Practical Statistics for Data Scientists: 50+ Essential Concepts Using R and Python* - Peter Bruce, Andrew Bruce, & Peter Gedeck (O'Reilly)

---

## 📚 Tabel Navigasi Notebook (Chapter 1 – 6)

| Bab | Judul Materi | Tautan Notebook | Status Pengerjaan |
| :---: | :--- | :---: | :---: |
| **Bab 1** | **Exploratory Data Analysis (EDA)** | [Buka Notebook Bab 1](./Bab1_Exploratory_Data_Analysis.ipynb) | Selesai (Milestone 1) |
| **Bab 2** | **Data and Sampling Distributions** | [Buka Notebook Bab 2](./Bab2_Data_and_Sampling_Distributions.ipynb) | Selesai (Milestone 1) |
| **Bab 3** | **Statistical Experiments and Significance Testing** | [Buka Notebook Bab 3](./Bab3_Statistical_Experiments_and_Significance_Testing.ipynb) | Selesai (Milestone 1) |
| **Bab 4** | **Regression and Prediction** | [Buka Notebook Bab 4](./Bab4_Regression_and_Prediction.ipynb) | Selesai (Milestone 1) |
| **Bab 5** | **Classification** | [Buka Notebook Bab 5](./Bab5_Classification.ipynb) | Selesai (Milestone 2) |
| **Bab 6** | **Statistical Machine Learning** | [Buka Notebook Bab 6](./Bab6_Statistical_Machine_Learning.ipynb) | Selesai (Milestone 2) |

---

## 📝 Dokumentasi Rinci Pembahasan per Bab

---

### BAB 1: EXPLORATORY DATA ANALYSIS (EDA)

Fokus utama bab ini adalah memahami struktur dasar data sebelum pemodelan, memilih ukuran statistik yang tahan terhadap pencilan (*outliers*), dan memvisualisasikan hubungan multivariat:

1. **Elemen Data Terstruktur:**
   - Membedakan data numerik kontinu (misal: tingkat kejahatan) dan diskrit (cacah), serta data kategorik nominal, ordinal, dan biner.
2. **Estimates of Location (Ukuran Pemusatan):**
   - Menguji kelemahan *Mean* ($\bar{x} = \sum x_i / n$) yang sangat rentan terhadap pencilan pada data populasi negara bagian AS (`state.csv`).
   - Menerapkan *Trimmed Mean* (membuang 10% ekor) dan *Median* yang bersifat *robust*.
   - Menerapkan *Weighted Mean* dan *Weighted Median* dengan pembobotan populasi negara bagian pada variabel `Murder.Rate`.
3. **Estimates of Variability (Ukuran Penyebaran):**
   - Menghitung Variansi ($s^2$), Standar Deviasi ($s$), Rentang Antarkuartil ($\text{IQR} = Q_3 - Q_1$), dan *Median Absolute Deviation from the Median* ($\text{MAD} = \text{Median}(\vert{}x_i - m\vert{})$) menggunakan `statsmodels.robust`.
   - Membuktikan bahwa kuadrat deviasi pada standar deviasi memperbesar penalti pencilan, sedangkan MAD dan IQR mempertahankan gambaran dispersi riil kelompok mayoritas.
4. **Eksplorasi Distribusi Data Tunggal:**
   - Menyusun *Five-Number Summary* dan memvisualisasikan *Boxplot* dengan batas kumis $1.5 \times \text{IQR}$ untuk mendeteksi pencilan positif (seperti California dan Texas).
   - Membagi data ke dalam 10 interval (*bins*) menggunakan tabel frekuensi dan *Histogram*.
   - Menerapkan *Kernel Density Estimation* (KDE) yang ditumpangkan di atas histogram.
5. **Eksplorasi Data Kategorik:**
   - Menganalisis penyebab penundaan penerbangan di Bandara Dallas/Fort Worth (`dfw_airline.csv`).
   - Menghitung Modus, Proporsi Persentase, dan memvisualisasikan dengan *Bar Chart* (menunjukkan faktor *Carrier* dan *ATC* mendominasi hampir 60% delay, jauh melampaui cuaca).
6. **Hubungan Multivariat (Korelasi & Eksplorasi Lanjutan):**
   - Menghitung matriks korelasi Pearson ($r$) pada imbal hasil harian saham S&P 500 (`sp500_data.csv.gz`), mendokumentasikan korelasi positif kuat antara AT&T (`T`) dan Verizon (`VZ`) ($r \approx 0.68$).
   - Menyajikan *Correlation Heatmap* untuk portofolio ETF.
   - Mengatasi masalah penumpukan titik (*overplotting*) pada ratusan ribu data perumahan King County (`kc_tax.csv.gz`) menggunakan **Hexagonal Binning** (`hexbin`) dan **Contour Plot 2D (KDE)**.
   - Menganalisis relasi data numerik terhadap kategori menggunakan **Grouped Boxplot** dan **Violin Plot** pada persentase keterlambatan maskapai (`airline_stats.csv`).

---

### BAB 2: DATA AND SAMPLING DISTRIBUTIONS

Fokus bab ini adalah membuktikan bagaimana proses pengambilan sampel acak memengaruhi kestabilan estimasi parameter populasi:

1. **Random Sampling & Central Limit Theorem (CLT):**
   - Menganalisis data pendapatan peminjam pinjaman (`loans_income.csv`). Meskipun distribusi data individu aslinya menceng ke kanan (*right-skewed*), distribusi rata-rata dari 1.000 sampel acak ($n=5$ dan $n=20$) berkonvergensi membentuk kurva normal simetris sempurna (*bell-shaped curve*).
   - Memverifikasi rumus teoretis Standar Error ($SE = s / \sqrt{n}$) terhadap simulasi empiris.
2. **The Bootstrap (Metode Resampling Modern):**
   - Mengambil sampel ulang berukuran $n$ dengan pengembalian (*sampling with replacement*) sebanyak $R = 1.000$ iterasi menggunakan `sklearn.utils.resample`.
   - Berhasil mengestimasi Standar Error dan bias untuk nilai **Median** pendapatan tanpa membutuhkan rumus analitis tertutup.
3. **Confidence Intervals (Selang Kepercayaan):**
   - Menyusun selang kepercayaan $90\%$ (persentil ke-5 dan ke-95) dan $95\%$ (persentil ke-2.5 dan ke-97.5) berbasis persentil bootstrap.
   - Memvalidasi bahwa metode non-parametrik bootstrap selaras dengan pendekatan teoretis parametrik normal ($z^* = 1.96$).
4. **Distribusi Normal & Fenomena Ekor Tebal (*Fat Tails*):**
   - Menggunakan visualisasi diagnostik **Quantile-Quantile Plot (QQ-Plot)** via `scipy.stats.probplot`.
   - Menguji data imbal hasil harian saham Netflix (`NFLX`): titik-titik pada QQ-plot membentuk kurva lengkung huruf "S" (jatuh di bawah garis pada kuantil negatif dan melambung di atas garis pada kuantil positif) dengan nilai *Excess Kurtosis* tinggi ($\approx 20$), membuktikan adanya *fat tails* (kejadian ekstrem terjadi jauh lebih sering daripada prediksi distribusi normal).
5. **Distribusi Probabilitas Kunci Lainnya:**
   - Membandingkan kurva **Student's t-Distribution** ($df=2, 10$) terhadap normal baku.
   - Menghitung fungsi probabilitas massa (PMF) dan kumulatif (CDF) pada **Distribusi Binomial** untuk rasio klik iklan.
   - Mensimulasikan cacah kejadian diskrit pada **Distribusi Poisson** ($\lambda = 2$) serta waktu tunggu kontinu antar-kejadian pada **Distribusi Eksponensial**.

---

### BAB 3: STATISTICAL EXPERIMENTS AND SIGNIFICANCE TESTING

Membahas perancangan eksperimen terkontrol dan pengujian signifikansi untuk menarik kesimpulan kausal:

1. **A/B Testing & Uji Permutasi (Permutation Test):**
   - Menguji perbedaan rata-rata durasi sesi pengguna antara Halaman Web A dan B (`web_page_data.csv`).
   - Menerapkan Uji Permutasi (resampling 1.000 kali dengan mengacak label perlakuan tanpa memedulikan kelompok) untuk membangun distribusi nol (*null distribution*).
   - Menghasilkan $p$-value dua arah sebesar $\approx 27\%$ ($> \alpha = 0.05$), menyimpulkan bahwa keunggulan Page B sebesar $0.36$ detik semata-mata merupakan fluktuasi acak wajar (gagal menolak $H_0$).
2. **Signifikansi Statistik & Two-Sample t-Test:**
   - Menerapkan pengujian parametrik klasik **Welch's t-test** (`scipy.stats.ttest_ind(equal_var=False)`).
   - Menghasilkan nilai $t = -1.123$ dan $p$-value $0.269$, memberikan hasil keputusan yang konsisten persis dengan uji permutasi.
3. **ANOVA (Analysis of Variance) & Rasio F:**
   - Menghindari masalah kelipatan pengujian (*multiple testing problem*) saat membandingkan 4 variasi tata letak halaman web (`four_sessions.csv`).
   - Menghitung tabel ANOVA via OLS regression: menghasilkan nilai $F = 2.7398$ dan $p$-value $0.0776$, membuktikan belum terdapat perbedaan yang signifikan antar varian pada tingkat kepercayaan $95\%$.
4. **Uji Chi-Kuadrat ($\chi^2$) & Uji Eksak Fisher:**
   - Menguji tabel kontinjensi rasio klik (*Click-Through Rate*) dari 3 variasi headline berita.
   - Menghitung frekuensi harapan ($E_{ij}$) dan statistik uji $\chi^2 = 1.6659$ ($p = 0.6586$), serta mengonfirmasi komparasi $2 \times 2$ via **Fisher's Exact Test** ($p = 0.3013$).
5. **Multi-Arm Bandit & Power Analysis:**
   - Mensimulasikan algoritma $\epsilon$-Greedy (10% eksplorasi, 90% eksploitasi) yang secara adaptif mengalihkan lalu lintas pengunjung ke banner dengan konversi tertinggi.
   - Menyusun kurva daya statistik (*Power Curve*) menggunakan `TTestIndPower` untuk menentukan batas ukuran sampel minimum eksperimen ($n$).

---

### BAB 4: REGRESSION AND PREDICTION

Membahas pemodelan prediktif kuantitatif menggunakan dataset penjualan rumah King County (`house_sales.csv.gz`):

1. **Simple Linear Regression (Metode Kuadrat Terkecil - OLS):**
   - Memodelkan harga jual (`AdjSalePrice`) berdasarkan luas bangunan (`SqFtTotLiving`): $b_1 \approx \$269.95/\text{sq ft}$ dan $b_0 \approx -\$43.580$.
   - Memvisualisasikan garis *best-fit* OLS terhadap titik sebaran sampel rumah ($R^2 \approx 32.5\%$).
2. **Multiple Linear Regression:**
   - Menambahkan fitur prediktor: luas tanah, jumlah kamar tidur, kamar mandi, dan mutu konstruksi (*BldgGrade*).
   - Nilai $R^2$ meningkat menjadi $\approx 54.1\%$ dan RMSE turun menjadi $\approx \$214.000$.
   - Menginterpretasikan fenomena koefisien negatif pada jumlah kamar tidur (prinsip *ceteris paribus*: menambah kamar tidur pada luas total yang sama mempersempit ukuran ruangan).
   - Menganalisis *Residual Plot* (Fitted Values vs Residuals) untuk memeriksa asumsi homoskedastisitas.
3. **Cross-Validation & Seleksi Model (AIC/BIC):**
   - Menerapkan **5-Fold Cross-Validation** via `cross_val_score`: rata-rata $\text{RMSE}_{cv} \approx \$214.500$ membuktikan kestabilan model di luar sampel data latih (*out-of-sample*).
   - Membuktikan penurunan tajam nilai AIC dan BIC dari model sederhana ke model berganda sebagai bukti keunggulan model yang lebih kaya fitur.
4. **Variabel Faktor / Dummy Variables:**
   - Mengonversi fitur kategorik tipe properti (`PropertyType`) menjadi variabel biner via `pd.get_dummies(drop_first=True)`.
   - Menginterpretasikan koefisien dummy secara relatif terhadap kategori referensi (*baseline*: `Multiplex`).
5. **Diagnostik Regresi & Regresi Polinomial:**
   - Mengidentifikasi titik-titik berpengaruh menggunakan **Leverage (Hat-Values)**, **Standardized Residuals**, dan **Cook's Distance**.
   - Mengimplementasikan **Regresi Polinomial Kuadratik** (`np.power(SqFtTotLiving, 2)`) yang berhasil menaikkan skor $R^2$ menjadi $34.1\%$ dan memodelkan lengkungan kenaikan harga pada properti mewah berukuran masif.

---

### BAB 5: CLASSIFICATION

Membahas klasifikasi terarah dengan fokus pada mitigasi risiko pinjaman perbankan (`loan_data.csv.gz`):

1. **Naive Bayes:**
   - Menerapkan `GaussianNB` pada kombinasi fitur numerik kontinu (`borrower_score`) dan fitur kategorik (`purpose_`, `home_`, `emp_len_`).
   - Memanfaatkan aturan Teorema Bayes dan probabilitas awal (*prior*) untuk memprediksi probabilitas posterior gagal bayar (*default*).
2. **Discriminant Analysis (LDA):**
   - Memodelkan probabilitas bersyarat multivariat menggunakan dua prediktor utama: `borrower_score` dan rasio cicilan terhadap pendapatan (`payment_inc_ratio`).
   - Menghitung koefisien diskriminan dan membentuk garis batas keputusan linier analitis tanpa iterasi berulang.
3. **Logistic Regression & Rasio Odds:**
   - Memetakan kombinasi linier ke dalam fungsi sigmoid/logit ($p \in [0, 1]$).
   - Mengonversi bobot koefisien menjadi **Odds Ratio ($e^{\beta}$)**: membuktikan kenaikan `borrower_score` menurunkan odds gagal bayar secara drastis ($\text{OR} < 0.01$), sedangkan kenaikan `payment_inc_ratio` menaikkan odds gagal bayar ($\text{OR} \approx 1.085$).
4. **Evaluasi Model Komprehensif (ROC/AUC):**
   - Memetakan kurva **ROC (Receiver Operating Characteristic)**: menghasilkan nilai $\text{AUC} \approx 0.69$, jauh melampaui tebakan acak.
   - Menyajikan kurva **Precision-Recall** untuk menganalisis pertukaran (*trade-off*) sensitivitas risiko.
5. **Strategi Penanganan Data Tidak Seimbang (*Imbalanced Data*):**
   - Mensimulasikan kondisi riil perbankan dengan rasio kelas $90:10$ (lancar vs macet).
   - Membuktikan kegagalan model standar yang meraih akurasi $90\%$ namun memiliki Recall $0\%$ untuk mendeteksi debitur macet.
   - Menerapkan **Cost-Sensitive Learning (`class_weight='balanced'`)** yang berhasil melipatgandakan Recall kelas minoritas hingga $\approx 65\%–70\%$.

---

### BAB 6: STATISTICAL MACHINE LEARNING

Membahas algoritma machine learning tingkat lanjut dan metode ansambel:

1. **K-Nearest Neighbors (KNN):**
   - Memanfaatkan KNN sebagai mesin referensi profil lokal untuk mengklasifikasikan pemohon pinjaman baru berdasarkan $K=20$ tetangga terdekat.
   - Menyajikan visualisasi **Scatter Plot 2D Spasial** yang memperlihatkan posisi pemohon baru bertanda bintang emas di tengah-tengah lingkaran tetangga terdekatnya.
2. **Tree Models (Decision Trees):**
   - Menerapkan partisi rekursif biner berbasis kriteria ketidakmurnian Gini (*Gini Impurity*).
   - Menghasilkan dan memvisualisasikan diagram hierarki **Decision Tree** terpotong (`max_depth=3`), menyajikan alur keputusan IF-THEN yang transparan bagi auditor kredit.
3. **Bagging & Random Forest:**
   - Menggabungkan 100 pohon bootstrap independen dengan pemilihan subset fitur acak ($\sqrt{p}$) di setiap simpul untuk memecah korelasi antar-pohon.
   - Memvisualisasikan diagram batang horizontal **Feature Importance (Mean Decrease Gini)**, mendokumentasikan dominasi `borrower_score` ($\approx 38\%$) dan `payment_inc_ratio` dalam penentuan risiko.
4. **Boosting (Gradient Boosting):**
   - Menerapkan pembelajaran sekuensial yang mengoreksi nilai sisaan (*residuals*) dari model sebelumnya secara bertahap.
   - Memvisualisasikan kurva **Penurunan Eror Log-Loss (*Staged Deviance*)** data latih vs data uji sepanjang 100 iterasi pohon untuk mendeteksi titik konvergensi optimal.

---

## 🛠️ Lingkungan Komputasi & Pustaka
* **Platform:** Google Colab / Python 3.12+
* **Pustaka Utama:** `pandas`, `numpy`, `scipy`, `statsmodels`, `scikit-learn`, `wquantiles`, `matplotlib`, dan `seaborn`.
