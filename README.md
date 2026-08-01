# RFM Segmentation

Bu layihə satış datası üzərində **RFM (Recency, Frequency, Monetary) analizi** vasitəsilə müştəri seqmentasiyasını həyata keçirir. Məqsəd müştəriləri davranışlarına görə qruplaşdırıb hər seqment üçün fərdi marketinq strategiyaları təklif etməkdir.

## Dataset

- **Mənbə:** [Kaggle — Sample Sales Data](https://www.kaggle.com/datasets/kyanyoga/sample-sales-data)
- 2823 sətir, 25 sütun; sifariş, məhsul, müştəri və ünvan məlumatlarını əhatə edir
- Tarix aralığı: 01/10/2003 – 09/09/2004 (2003–2005-ci illər)

## Layihənin Strukturu

```
rfm-segmentation/
│
├── data/
│   ├── raw/
│   │   └── sales_data_sample.csv          # Kaggle-dan yüklənən xam dataset
│   └── processed/
│       └── sales_data_cleaned.csv         # Təmizlənmiş dataset
│
├── notebooks/
│   ├── 01_data_cleaning.ipynb             # Data təmizləmə prosesi
│   ├── 02_eda.ipynb                       # RFM analizi və seqmentasiya
│   └── rfm_3d_scatter.html                # İnteraktiv 3D RFM vizuallaşdırması (Plotly)
│
├── main.py
├── pyproject.toml
├── uv.lock
└── README.md
```

## İstifadə Olunan Texnologiyalar

- Python 3.13+ (mühit `uv` ilə idarə olunur)
- pandas, numpy — data emalı
- matplotlib, seaborn, plotly — vizuallaşdırma
- Jupyter Notebook

## 1. Data Təmizləmə (`01_data_cleaning.ipynb`)

- Dataset-in strukturu (`shape`, `info`, `describe`) araşdırılıb, boş və təkrarlanan sətirlər yoxlanılıb
- `ORDERDATE` sütunu `datetime` formatına çevrilib
- Boş dəyərlər məntiqli qaydada doldurulub:
  - `ADDRESSLINE2` → "Not Provided"
  - `STATE` → USA-ya aid olmayan ölkələr üçün "Not Applicable"
  - `POSTALCODE` → ünvana əsaslanaraq (California, 94217) doldurulub
  - `TERRITORY` → Şimali Amerika ölkələri üçün "NA" ilə doldurulub
- Nəticə `data/processed/sales_data_cleaned.csv` olaraq yadda saxlanılıb

## 2. RFM Analizi və Seqmentasiya (`02_eda.ipynb`)

- Hər müştəri üzrə **Recency** (son sifarişdən neçə gün keçib), **Frequency** (unikal sifariş sayı) və **Monetary** (ümumi xərclənmiş məbləğ) hesablanıb
- `pd.qcut` ilə hər metrik 4 çərəyə (quartile) bölünərək 1–4 aralığında bal verilib (R_score, F_score, M_score)
- Bu balların birləşməsindən `RFM_Segment` kodu formalaşdırılıb
- Qayda-əsaslı funksiya ilə hər müştəri 5 seqmentdən birinə mənsub edilib
- Seqmentlər üzrə müştəri sayı və ortalama R/F/M dəyərləri hesablanıb
- Plotly ilə 3D interaktiv scatter plot (`rfm_3d_scatter.html`) hazırlanıb — Recency, Frequency, Monetary oxları üzrə seqmentlərin vizual müqayisəsi

### Müştəri Seqmentləri

| Seqment | Təsvir |
|---|---|
| **Champions** | Ən aktiv, ən tez-tez və ən çox xərcləyən VIP müştərilər |
| **Loyal** | Mütəmadi alış-veriş edən sadiq müştərilər |
| **At Risk** | Əvvəllər dəyərli olan, lakin son zamanlar uzaqlaşan müştərilər |
| **Needs Attention** | Aşağı dəyərli, lakin hazırda aktiv olan müştərilər |
| **Lost** | Uzun müddətdir alış-veriş etməyən, az dəyər gətirmiş müştərilər |

## Marketinq Strategiyaları (Seqment üzrə)

- **Champions:** VIP proqram, həftəlik fərdi endirim kuponları
- **Loyal:** Loyallıq (bonus/xal) proqramı, cross-sell və up-sell kampaniyaları
- **At Risk:** Keçmiş alışlara uyğun xüsusi endirimlər, şəxsi zəng/email ilə əlaqə
- **Needs Attention:** Marağı artıran kontent, kiçik endirimlər və bundle təklifləri
- **Lost:** Aşağı büdcəli reaktivasiya kampaniyaları (SMS/Email), "geri dön" tipli güclü təkliflər

## Necə İşə Salmaq

Layihə `uv` ilə idarə olunur:

```bash
uv sync
uv run jupyter notebook notebooks/01_data_cleaning.ipynb
```
