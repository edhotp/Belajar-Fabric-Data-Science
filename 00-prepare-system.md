# Modul 0 — Prepare System

Sebelum memulai tutorial, siapkan **workspace**, **lakehouse**, dan **notebook** di Microsoft Fabric.

---

## 🎯 Tujuan

- Mengaktifkan workload Fabric Data Science
- Membuat Lakehouse
- Mengimpor / membuat Notebook untuk seluruh tutorial
- Meng-attach Lakehouse ke Notebook

---

## 🗺️ Alur Persiapan

```mermaid
flowchart LR
    A[Login ke<br/>Fabric] --> B{Sudah punya<br/>Lakehouse?}
    B -- Belum --> C[Create<br/>Lakehouse]
    B -- Sudah --> D[Pilih Workspace]
    C --> D
    D --> E[Import / Create<br/>Notebook .ipynb]
    E --> F[Attach Lakehouse<br/>ke Notebook]
    F --> G[Siap mengikuti<br/>Modul 1]

    style C fill:#fff4e6
    style F fill:#e6ffe6
```

---

## 1. Prasyarat

- Memiliki [Microsoft Fabric subscription](https://learn.microsoft.com/en-us/fabric/enterprise/licenses) atau [free trial](https://learn.microsoft.com/en-us/fabric/fundamentals/fabric-trial)
- Login ke <https://fabric.microsoft.com/>
- Pindah ke pengalaman **Fabric** lewat experience switcher (kiri-bawah)
- Jika belum ada Lakehouse, buat dengan mengikuti [Create a lakehouse in Microsoft Fabric](https://learn.microsoft.com/en-us/fabric/data-engineering/create-lakehouse)

![Screenshot pemilihan Fabric pada experience switcher](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-prepare-system/switch-to-data-science.png)

---

## 2. Membuat / Mengimpor Notebook

Tutorial ini tersedia sebagai file Jupyter Notebook di GitHub. Pilih salah satu cara:

### Opsi A — Buat Notebook Baru
1. Buka workspace Anda
2. Klik **+ New** → **Notebook**
3. Salin-tempel kode dari setiap modul tutorial

### Opsi B — Import Notebook dari GitHub
Notebook asli sudah disediakan di folder lokal [`notebooks/`](./notebooks/) (mirror dari [`data-science-tutorial`](https://github.com/microsoft/fabric-samples/tree/main/docs-samples/data-science/data-science-tutorial) di GitHub):

| Notebook | Modul Tutorial |
|----------|----------------|
| `1-ingest-data.ipynb` | Modul 1 |
| `2-explore-cleanse-data.ipynb` | Modul 2 |
| `3-train-evaluate.ipynb` | Modul 3 |
| `4-predict.ipynb` | Modul 4 |

> ℹ️ Tersedia juga versi **long-name** (`01-…`, `02-…`, dst.) — sama isinya, tinggal pilih salah satu.

> ⚠️ Jika men-download manual dari GitHub, pakai link **"Raw"** agar file `.ipynb` tidak rusak.

Lalu di Fabric:
1. Buka workspace
2. Klik **Import** di command bar → **Notebook** → **From this computer** → pilih file `.ipynb`
3. Setelah ter-import, buka notebook → menu **Edit** → **Clear all outputs**

> ✅ Notebook di folder lokal [`notebooks/`](./notebooks/) **sudah dipatch** (`pd.get_dummies(..., dtype=int)`) untuk mencegah error `Cannot convert numpy type object to spark type` di Modul 4.
>
> ⚠️ Jika Anda men-download **manual dari GitHub**, terapkan fix `dtype=int` pada notebook `2-explore-cleanse-data.ipynb` — lihat detail di [Modul 2](./02-explore-cleanse-data.md).

![Screenshot menu Edit untuk Clear all outputs pada notebook](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-prepare-system/clear-outputs.png)

### Opsi C — Sample dari Workload Data Science
1. Panel kiri → **Workloads** → **Data Science**
2. Pada kartu **Explore a sample**, klik **Select**
3. Pilih sample dari tab **End-to-end workflows (Python)**

---

## 3. Attach Lakehouse ke Notebook

> ⚠️ **Wajib** dilakukan untuk **setiap notebook** sebelum eksekusi sel.

1. Buka notebook di workspace
2. Pada panel kiri, klik **Add lakehouse**
3. Pilih:
   - **New** → buat Lakehouse baru, beri nama, lalu **Create**, atau
   - **Existing lakehouse** → pilih dari Data hub, lalu **Add**
4. Setelah ter-attach, Lakehouse muncul di panel kiri dan Anda dapat melihat folder **Files** & **Tables**.

![Diagram langkah attach lakehouse ke notebook](https://learn.microsoft.com/en-us/fabric/data-science/media/tutorial-data-science-prepare-system/attach-lakehouse-process.png)

```mermaid
flowchart LR
    N[Notebook] -->|Add lakehouse| L[(Lakehouse)]
    L --> F[Files/]
    L --> T[Tables/]
    F -.menampung.-> R[raw csv]
    T -.menampung.-> D[Delta tables]
```

---

## ✅ Checklist Sebelum Lanjut

- [ ] Workspace Fabric aktif
- [ ] 1 Lakehouse sudah dibuat
- [ ] Notebook untuk Modul 1–4 sudah tersedia di workspace
- [ ] Lakehouse ter-attach ke notebook Modul 1

➡️ Lanjut ke **[Modul 1 — Ingest Data](./01-ingest-data.md)**
