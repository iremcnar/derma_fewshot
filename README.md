# 🏥 Sağlıkta Yapay Zeka: Few-Shot Learning ile Deri Hastalıkları Sınıflandırma

Bu proje, **Burdur Mehmet Akif Ersoy Üniversitesi** bünyesindeki **Sağlıkta Yapay Zeka** dersi uygulama çalışmaları kapsamında geliştirilmiştir. Projenin amacı, kısıtlı veriyle öğrenme (**Few-Shot Learning**) tekniklerini kullanarak deri hastalıklarını (Dermatoloji) sınıflandıran bir derin öğrenme modeli geliştirmektir.

## 📌 Proje Özeti
Tıbbi görüntüleme setlerinde, özellikle nadir görülen hastalıklar için yeterli miktarda veri bulmak zordur. Bu projede, klasik derin öğrenme yöntemlerinin aksine, her sınıftan sadece birkaç örnekle (5-way 5-shot) öğrenme gerçekleştirebilen **Prototypical Networks** mimarisi kullanılmıştır.

**Veri Seti:** [MedMNIST - DermaMNIST](https://medmnist.com/) (Deri lezyonlarını içeren tıbbi görüntü seti)

## 🛠️ Kullanılan Teknolojiler
- **Python** & **PyTorch** (Model mimarisi ve eğitim)
- **ResNet18** (Özellik çıkarıcı/Feature Extractor olarak)
- **MedMNIST** (Medikal veri yükleme kütüphanesi)
- **Scikit-Learn** (AUC-ROC ve performans metrikleri)
- **NumPy & Torchvision** (Veri manipülasyonu ve transformlar)

## 🚀 Proje Uygulama Adımları

### 1. Veri Hazırlığı ve MedMNIST Entegrasyonu
- **Veri Seti:** DermaMNIST veri seti yüklendi ve görüntüler ResNet mimarisine uygun şekilde normalize edildi.
- **N-Way K-Shot Hazırlığı:** Veri seti, Few-Shot öğrenme mantığına uygun olarak destek setleri (support sets) ve sorgu setleri (query sets) şeklinde yapılandırıldı.



### 2. Modelleme Yaklaşımı: Prototypical Networks
Projede, sınıfları temsil eden birer "prototip" vektör oluşturma mantığına dayanan mimari kullanılmıştır:
- **Feature Extractor:** Önceden eğitilmiş (Pre-trained) **ResNet18** mimarisi, deri görüntülerinden yüksek seviyeli öznitelik vektörleri çıkarmak için kullanıldı.
- **Prototip Oluşturma:** Destek setindeki her sınıfa ait görüntülerin öznitelik ortalamaları alınarak o sınıfın "Prototipi" (merkezi) belirlendi.
- **Mesafe Ölçümü (Euclidean Distance):** Sorgu görüntüsünün hangi sınıfa ait olduğu, prototiplere olan Öklid mesafesi hesaplanarak (Softmax ile olasılığa dönüştürülerek) tahmin edildi.



### 3. Eğitim ve Few-Shot Evaluation
- **Task-Based Training:** Model, her adımda farklı "görevler" (tasks) üzerinden eğitilerek yeni gördüğü sınıflara hızlı uyum sağlama yeteneği kazandırıldı.
- **Performans Ölçümü:** Modelin başarısı sadece doğruluk (Accuracy) ile değil, tıbbi teşhislerde kritik olan **AUC-ROC** skoru üzerinden değerlendirildi.

## 📊 Öne Çıkan Bulgular
- **Düşük Veri Verimliliği:** Modelin, her sınıftan sadece 5 örnekle (5-shot) bile deri lezyonlarını ayırt etmede başarılı sonuçlar verdiği gözlemlendi.
- **Transfer Learning:** ResNet18 ağırlıklarının kullanımı, tıbbi görüntülerdeki doku ve kenar özelliklerinin daha hızlı yakalanmasını sağladı.
- **Novelty Detection:** Prototip merkezlerine uzaklık üzerinden, modelin eğitilmediği sınıfları tespit etme potansiyeli analiz edildi.

## 📂 Dosya Yapısı
- `derma_fewshot.ipynb`: MedMNIST veri yükleme, ResNet tabanlı öznitelik çıkarımı ve Prototypical Network eğitim süreçlerini içeren ana notebook.
- `requirements.txt`: Projenin çalışması için gerekli kütüphaneler (`medmnist`, `torch`, `torchvision`, `sklearn`).

---
**Not:** Bu çalışma, kısıtlı veriyle medikal tanı koyma üzerine bir akademik araştırma uygulamasıdır. Klinik ortamlarda kullanılması için daha geniş kapsamlı testler ve uzman onayı gerekmektedir.
