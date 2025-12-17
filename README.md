# 🎣 Phishing Website Detection (Oltalama Web Sitesi Tespiti)

![Python](https://img.shields.io/badge/Python-3.8%2B-blue)
![Machine Learning](https://img.shields.io/badge/Machine%20Learning-Scikit%20Learn-orange)
![Status](https://img.shields.io/badge/Status-Completed-green)

## 📝 Proje Hakkında
Bu proje, web sitelerinin URL özelliklerini analizerek "Oltalama (Phishing)" veya "Yasal (Legitimate)" olup olmadıklarını tespit etmeyi amaçlayan uçtan uca bir makine öğrenmesi çalışmasıdır. Proje kapsamında veri temizliğinden model optimizasyonuna kadar 14 adımlı detaylı bir iş akışı izlenmiş ve 5 farklı algoritma kıyaslanmıştır.

## 📂 Veri Seti (Dataset)
Bu çalışmada kullanılan veri seti **Kaggle** platformundan temin edilmiştir.
* **Veri Seti Kaynağı:** [Phishing Website Dataset - Kaggle](https://www.kaggle.com/datasets/akashkr/phishing-website-dataset)
* **Özellikler:** URL yapısı, alan adı yaşı, HTTPS durumu, trafik verileri gibi 30 farklı öznitelik.
* **Hedef Değişken:** `Result` (1: Legitimate, 0: Phishing).
    * *Not: Veri setindeki orijinal -1 değerleri, sigmoid tabanlı modellerle uyumluluk için 0'a dönüştürülmüştür.*

## 🛠️ Kullanılan Teknolojiler ve Kütüphaneler
Projede aşağıdaki Python kütüphaneleri kullanılmıştır:
* **Veri Analizi:** `pandas`, `numpy`
* **Görselleştirme:** `matplotlib`, `seaborn`
* **İstatistik:** `scipy` (Kolmogorov-Smirnov testi)
* **Makine Öğrenmesi:** `scikit-learn`
    * *Modeller:* Random Forest, SVM, KNN, Decision Tree, MLP (YSA)
    * *Önişleme:* StandardScaler, SelectKBest

## ⚙️ Uygulanan Yöntemler ve İş Akışı
Proje, ham verinin modellemeye hazır hale getirilmesi için aşağıdaki titiz süreçlerden geçmiştir:

### 1. Veri Temizliği (Data Cleaning)
* Gereksiz `index` sütunu çıkarıldı.
* **Duplicate Kontrolü:** Veri setinde tespit edilen **5.206 adet tekrarlayan satır** silinerek veri bütünlüğü sağlandı (Satır sayısı 11.055 -> 5.849'a düştü).
* Eksik veri (Null) kontrolü yapıldı (Veri seti eksiksizdir).

### 2. Veri Dengeleme (Undersampling)
Veri setindeki sınıf dengesizliğini gidermek ve modelin taraflı (biased) öğrenmesini engellemek için **Undersampling** tekniği uygulandı.
* **Sonuç:** Her iki sınıf (Phishing ve Legitimate) eşitlenerek toplam **5.660** satırlık dengeli bir veri seti oluşturuldu.

### 3. Özellik Seçimi (Feature Selection)
Model performansını artırmak ve "Curse of Dimensionality" (Boyut Laneti) riskini azaltmak için **ANOVA F-value (SelectKBest)** yöntemi kullanıldı.
* 30 özellik analiz edildi ve en yüksek varyansı açıklayan **29 özellik** modelleme için seçildi.

### 4. İstatistiksel Analiz ve Ölçekleme
* **Normallik Testi:** Kolmogorov-Smirnov testi ile verilerin dağılımı incelendi (Veriler normal dağılım göstermemektedir).
* **Scaling:** KNN ve SVM gibi mesafeye duyarlı algoritmalar için veriler `StandardScaler` ile ölçeklenerek (Ortalama=0, Std.Sapma=1) standardize edildi.

## 📊 Model Sonuçları ve Performans
Beş farklı makine öğrenmesi algoritması eğitilmiş ve test seti üzerindeki doğruluk (Accuracy) oranları karşılaştırılmıştır. En başarılı sonuç **Random Forest** algoritması ile elde edilmiştir.

| Model | Doğruluk (Accuracy) | Açıklama |
| :--- | :--- | :--- |
| **Random Forest** | **%94.96** 🏆 | En yüksek başarı, düşük varyans. |
| **SVM (Destek Vektör)** | %93.90 | Doğrusal olmayan verilerde başarılı. |
| **ANN (Yapay Sinir Ağı)**| %93.55 | Karmaşık ilişkileri öğrenmede iyi. |
| **Decision Tree** | %93.29 | Yorumlanabilir, ancak RF kadar güçlü değil. |
| **KNN** | %91.87 | En düşük performans ve overfitting riski. |

### Overfitting Analizi
Modellerin Eğitim (Train) ve Test başarıları kıyaslanarak aşırı öğrenme durumu kontrol edilmiştir:
* **Random Forest:** Eğitim (%96.97) ve Test (%95.05) skorları birbirine çok yakındır. Model genelleştirme yeteneğine sahiptir. ✅
* **KNN:** Eğitim (%98.98) ve Test (%92.31) arasındaki fark yüksektir, overfitting riski taşımaktadır. ⚠️

## 🚀 Sonuç
Yapılan kapsamlı analizler sonucunda, **Random Forest** algoritmasının oltalama saldırılarını tespit etmede en güvenilir ve yüksek performanslı yöntem olduğu belirlenmiştir. Tekrarlayan verilerin temizlenmesi ve özellik seçimi adımları, modelin başarısını doğrudan etkileyen kritik faktörler olmuştur.

---
