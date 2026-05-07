# Modul 5 — Visualisasi Prediksi dengan Power BI

Modul terakhir ini membangun **Power BI report** dari tabel prediksi yang Anda hasilkan di Modul 4 (`df_test_with_predictions_v1`) menggunakan **DirectLake**.

📖 Referensi: <https://learn.microsoft.com/en-us/fabric/data-science/tutorial-data-science-create-report>

---

## 🎯 Tujuan

- Membuat **Semantic Model** dari tabel prediksi
- Menambahkan **DAX measures** (Churn Rate, Customers, per-Geography churn)
- Membangun report dengan **Card**, **Line and stacked column chart**, **Stacked column chart**, dan **Clustered column chart**

---

## 🗺️ Alur Modul 5

```mermaid
flowchart LR
    A[("Tables/<br/>df_test_with_predictions_v1")] -->|DirectLake| B[Semantic Model<br/>'bank churn predictions']
    B --> M1[Measure: Churn Rate]
    B --> M2[Measure: Customers]
    B --> M3[Measure: Germany Churn]
    B --> M4[Measure: Spain Churn]
    B --> M5[Measure: France Churn]
    M1 & M2 & M3 & M4 & M5 --> R[📊 Power BI Report<br/>Bank Customer Churn]

    style B fill:#e6ffe6
    style R fill:#fff4e6
```

---

## 1️⃣ Buat Semantic Model

1. Panel kiri → pilih **workspace**
2. Filter di kanan atas: **Lakehouse**

   ![Screenshot filter Lakehouse di workspace](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/filter-by-lakehouse.png)

3. Buka Lakehouse yang sama dengan Modul 4

   ![Screenshot pemilihan Lakehouse spesifik](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/select-lakehouse.png)

4. Pada ribbon Lakehouse, klik **New semantic model**

   ![Screenshot tombol New semantic model di ribbon Lakehouse](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/new-power-bi-dataset.png)

5. Beri nama: `bank churn predictions`
6. Centang tabel **`df_test_with_predictions_v1`** (alias `customer_churn_test_predictions`)

   ![Screenshot dialog New semantic model dengan dataset prediksi terpilih](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/select-predictions-data.png)

7. Klik **Confirm**

---

## 2️⃣ Tambahkan Measures (DAX)

> Klik **New measure** di ribbon, lalu ketik formula. Klik ✓ untuk apply.

![Screenshot pembuatan New measure pada semantic model](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/new-measure.png)

### Churn Rate

```DAX
Churn Rate = AVERAGE(customer_churn_test_predictions[predictions])
```

![Screenshot tombol checkmark di formula bar untuk apply measure](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/select-checkmark.png)

![Screenshot measure baru muncul di data table](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/new-datatable-measure.png)

Properties:
- Format: **Percentage**
- Decimal places: **1**

![Screenshot measure Churn Rate dengan properties Percentage 1 desimal](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/churn-rate.png)

### Total Customers

> Setiap baris prediksi merepresentasikan satu nasabah — sehingga `COUNT(predictions)` = jumlah nasabah.

```DAX
Customers = COUNT(customer_churn_test_predictions[predictions])
```

### Churn per Negara

```DAX
Germany Churn = CALCULATE(
    AVERAGE(customer_churn_test_predictions[predictions]),
    FILTER(customer_churn_test_predictions, customer_churn_test_predictions[Geography_Germany] = TRUE())
)
```

```DAX
Spain Churn = CALCULATE(
    AVERAGE(customer_churn_test_predictions[predictions]),
    FILTER(customer_churn_test_predictions, customer_churn_test_predictions[Geography_Spain] = TRUE())
)
```

```DAX
France Churn = CALCULATE(
    AVERAGE(customer_churn_test_predictions[predictions]),
    FILTER(customer_churn_test_predictions, customer_churn_test_predictions[Geography_France] = TRUE())
)
```

---

## 3️⃣ Bangun Report

Klik **Create new report** di ribbon → buka tab Power BI authoring baru.

![Screenshot tombol Create a report di ribbon semantic model](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/visualize-this-data.png)

### Layout yang disarankan

```mermaid
flowchart TB
    subgraph Header
        T[Text: 'Bank Customer Churn']
        C1[Card: Churn Rate]
    end
    subgraph Top["Top Row"]
        V1[Line + Stacked Column<br/>X: Age<br/>Line: Customers<br/>Column: Churn Rate]
        V2[Line + Stacked Column<br/>X: NumOfProducts<br/>Line: Customers<br/>Column: Churn Rate]
    end
    subgraph Bottom["Bottom Row"]
        V3[Stacked Column<br/>X: NewCreditsScore<br/>Y: Churn Rate]
        V4[Clustered Column<br/>Y: Germany / Spain / France Churn]
    end
    Header --> Top --> Bottom
```

### Langkah Detail

1. **Title** — tambahkan **Text box** dari ribbon, ketik: `Bank Customer Churn`. Atur **font size** dan **background color** lewat panel **Format** (atau format bar yang muncul saat teks dipilih).

   ![Screenshot opsi Text box di ribbon](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/select-textbox.png)

   ![Screenshot text box berisi judul report](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/build-textbox-value.png)

2. **Card — Churn Rate** — di panel Visualizations pilih ikon **Card** → di **Data pane** centang **Churn Rate** → atur font/background lewat panel **Format** → drag ke pojok kanan atas.

   ![Screenshot pemilihan ikon Card pada panel Visualizations](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/select-card-icon.png)

   ![Screenshot pemilihan Churn Rate di Data pane](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/select-churn-rate.png)

   ![Screenshot pengaturan format pada panel Format](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/format-report.png)

   ![Screenshot Card Churn Rate di pojok kanan atas report](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/card-churn.png)

3. **Line + Stacked Column (Age)** — X-axis: `Age`, **Line y-axis**: `Customers`, **Column y-axis**: `Churn Rate`.

   ![Screenshot pemilihan Line and stacked column chart](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/select-line-and-stacked-column-chart.png)

   ![Screenshot pemilihan Age, Churn Rate, Customers di Data pane](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/select-data-pane-options.png)

   ![Screenshot konfigurasi field axis chart](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/configure-chart.png)

4. **Line + Stacked Column (NumOfProducts)** — X-axis: `NumOfProducts`, **Line y-axis**: `Customers`, **Column y-axis**: `Churn Rate`.
   > ⚠️ Pastikan field **Column y-axis** hanya berisi **satu instance** `Churn Rate`. Hapus field lain bila ada.

   ![Screenshot stacked column chart NumOfProducts](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/number-of-products.png)

5. **Stacked Column (Credit Score)** — X-axis: `NewCreditsScore`, Y-axis: `Churn Rate`. Di panel **Format** ubah judul `NewCreditsScore` menjadi `Credit Score`.
   > 💡 Anda mungkin perlu memperlebar X-axis chart ini agar label terbaca.

   ![Screenshot stacked column chart NewCreditScore](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/new-credit-score.png)

   ![Screenshot mengubah judul chart](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/change-title.png)

6. **Clustered Column (per Country)** — Y-axis (urut): `Germany Churn`, `Spain Churn`, `France Churn`. Atur ukuran chart sesuai kebutuhan.

   ![Screenshot clustered column chart per negara](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-create-report/germany-spain-france.png)

> 📝 **Catatan:** Tutorial ini mendemokan analisis dasar di Power BI. Untuk skenario churn nyata, Anda dapat memperluas dengan visualisasi tambahan sesuai kebutuhan tim bisnis dan KPI yang sudah distandardisasi.

---

## 🔍 Insight Akhir

- 🔴 Nasabah dengan **>2 produk** memiliki churn rate jauh lebih tinggi → bank perlu investigasi alasannya.
- 🌍 Nasabah di **Germany** punya churn rate tertinggi vs France & Spain.
- 👥 Rentang usia **45–60** paling rawan churn.
- 📉 Nasabah dengan **credit score rendah** lebih mungkin pindah → tawarkan program retention.

---

## 🎓 Selamat!

Anda telah menyelesaikan tutorial **end-to-end Microsoft Fabric Data Science**:

```mermaid
flowchart LR
    M0[✅ Prep<br/>System] --> M1[✅ Ingest]
    M1 --> M2[✅ Explore<br/>+ Cleanse]
    M2 --> M3[✅ Train<br/>+ MLflow]
    M3 --> M4[✅ Batch<br/>Scoring]
    M4 --> M5[✅ Power BI<br/>Report]

    style M5 fill:#e6ffe6,stroke:#107c10,stroke-width:2px
```

### 📚 Bacaan Lanjutan

- [Microsoft Fabric Data Science overview](https://learn.microsoft.com/en-us/fabric/data-science/data-science-overview)
- [Machine learning model scoring with PREDICT](https://learn.microsoft.com/en-us/fabric/data-science/model-scoring-predict)
- [MLflow autologging in Fabric](https://aka.ms/fabric-autologging)
- [Data Wrangler](https://learn.microsoft.com/en-us/fabric/data-science/data-wrangler)
- [SemPy library](https://learn.microsoft.com/en-us/fabric/data-science/semantic-link-overview)

⬅️ Kembali ke **[README](./README.md)**
