# Titanic — EDA ve Hayatta Kalma Tahmini

Titanic yolcu verisi üzerinde keşifsel veri analizi (EDA) ve üç farklı makine öğrenmesi modelinin karşılaştırmalı uygulaması.

---

## Proje Yapısı

```
titanic-ml/
│
├── data/
│   └── train.csv
│
├── notebooks/
│   ├── EDA.ipynb                  # Keşifsel veri analizi ve ön işleme
│   ├── Logistic_Regression.ipynb  # Logistic Regression modeli
│   ├── Decision_Tree.ipynb        # Decision Tree modeli
│   └── Random_Forest.ipynb        # Random Forest modeli
│
└── README.md
```

---

## Kullanılan Teknolojiler

- Python 3.x
- Pandas, NumPy
- Matplotlib, Seaborn
- Scikit-learn

---

## Proje Adımları

### 1. Keşifsel Veri Analizi (EDA)
- 891 yolcu, 12 değişken incelendi
- Eksik değer analizi: Age (%20), Cabin (%77)
- Temel bulgular:
  - Yolcuların yalnızca %38'i hayatta kaldı
  - Kadınların hayatta kalma oranı erkeklerin 3 katı
  - 1. sınıf yolcular 3. sınıfa kıyasla çok daha yüksek hayatta kalma oranına sahip
  - Yüksek bilet fiyatı, hayatta kalmayla güçlü korelasyon gösteriyor

### 2. Veri Ön İşleme
- `Age`: 177 eksik değer medyan ile dolduruldu
- `Embarked`: 2 eksik değer mod ile dolduruldu
- `Cabin`: ikili değişkene dönüştürüldü (1 = kabin var, 0 = yok)
- `Sex`, `Embarked`: sayısal değerlere encode edildi
- `PassengerId`, `Name`, `Ticket`: modele katkısız olduğu için çıkarıldı

### 3. Modeller

| Model | Yaklaşım |
|---|---|
| Logistic Regression | Doğrusal karar sınırı; temel referans model |
| Decision Tree | Kural tabanlı bölünme; görselleştirilebilir yapı |
| Random Forest | Çoklu ağaç topluluğu; daha güçlü genelleme |

Her model aynı özellikler ve aynı `train_test_split` (random_state=42) ile eğitildi.

---

## Sonuçlar

| Model | Test Accuracy | CV Ortalama (5-Fold) |
|---|---|---|
| Logistic Regression | 0.7977 | 0.8100 |
| Decision Tree | 0.7989 | 0.8371 |
| Random Forest | 0.7989 | 0.8261 |

### Yorumlar

**Test accuracy üç modelde neredeyse aynı.** Bu, asıl sınırın model seçiminden değil, feature'lardan kaynaklandığını gösteriyor. Daha anlamlı fark `cross-validation` skorlarında ortaya çıkıyor.

**Decision Tree CV en yüksek (0.8371).** `max_depth=4` kısıtlaması overfitting'i iyi dengelemiş; model hem basit hem genelleyici.

**Random Forest CV beklenenden düşük (0.8261).** Normalde RF > DT olur. `max_depth=6` biraz derin kalıyor olabilir; `max_depth=4` veya `max_depth=5` ile tekrar test edilebilir.

**Logistic Regression CV en düşük (0.8100).** Doğrusal sınır, verinin doğrusal olmayan ilişkilerini tam yakalayamıyor — beklenen bir sonuç.

**Genel çıkarım:** Feature engineering geliştirmeden model değiştirmek anlamlı bir kazanım sağlamıyor. Bir sonraki adım olarak `FamilySize = SibSp + Parch + 1` veya `Title` (isimden unvan çıkarma) gibi yeni özellikler denenebilir.

---

## Nasıl Çalıştırılır

```bash
git clone https://github.com/emir-caliskan/titanic-ml.git
cd titanic-ml
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
jupyter notebook
```

Kaggle'dan `train.csv` indirip `data/` klasörüne koy, ardından notebook'ları sırayla çalıştır.
