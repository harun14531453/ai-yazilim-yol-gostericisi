# 🛡️ Güvenlik ve Kalite Standartları (Security & Quality Gates)

Bu belge, üretilen kodun sadece "çalışmasını" değil, aynı zamanda güvenli, kaliteli ve profesyonel standartlarda olmasını sağlar.

## 🔐 1. Güvenlik ve Gizlilik Politikası (Secret Management)
Yazılım dünyasının en büyük risklerinden biri şifrelerin ve anahtarların kod içerisinde paylaşılmasıdır.
- **Hardcoding Yasaktır:** API anahtarları, veritabanı şifreleri, gizli tokenlar ASLA kodun içine yazılmaz.
- **Environment Variables (.env):** Tüm hassas veriler `.env` dosyasında tutulmalıdır. 
- **GitIgnore Filtresi:** `.env`, `/node_modules`, `/dist`, `.DS_Store` gibi dosyalar mutlaka `.gitignore` dosyasına eklenmeli ve asla commit edilmemelidir.
- **Sızıntı Kontrolü:** Kod teslim edilmeden önce, içinde kazara bırakılmış gizli bir bilgi olup olmadığı taranmalıdır.

## 💎 2. Kalite Kapısı (Code Quality Gate)
"Çalışan kod" yeterli değildir; "bakımı yapılabilir kod" esastır.
- **Okunabilirlik:** Değişken ve fonksiyon isimleri İngilizce, anlamlı ve betimleyici olmalıdır (Örn: `calc()` yerine `calculateTotalInvoice()`).
- **Modüler Yapı:** Tek bir dev dosya yerine, sorumluluklarına göre bölünmüş modüler dosyalar tercih edilmelidir.
- **Dokümantasyon:** Karmaşık algoritmaların üzerine "neden böyle yapıldığına" dair kısa, açıklayıcı yorum satırları eklenmelidir.
- **Spagetti Kod Reddi:** Bir fonksiyon 50 satırdan fazlaysa, parçalanması ve alt fonksiyonlara bölünmesi değerlendirilmelidir.

## 📦 3. Teslimat Standartları (Delivery Package)
Bir iş "bitti" denildiğinde, teslimat şu paketi içermek zorundadır:
1. **Güncel Kaynak Kod:** Çalışan ve test edilmiş son sürüm.
2. **Kurulum Klavuzu (`INSTALL.md`):** Bağımlılıkların nasıl kurulacağı (Step-by-step).
3. **Çalıştırma Talimatı:** Programa nasıl giriş yapılacağı ve başlatılacağı.
4. **Doğrulama Raporu:** Hangi özelliklerin test edildiği ve nasıl doğrulandığına dair kısa liste.
5. **Bilinen Kısıtlamalar:** Henüz çözülmemiş veya bilinçli olarak ertelenmiş eksiklikler.

---

## 🔍 Hasar Analizi ve Kurtarma Protokolü (Recovery Protocol)
Kullanıcı bir GitHub linki verdiğinde, AI kod yazmaya başlamadan önce şu **"Sağlık Kontrolü"**nü yapmak zorundadır:

### A. Statik Analiz (Hasar Tespiti)
AI, depoyu klonladığı an şu kontrol listesini işletir:
- [ ] **Eksik Bağımlılıklar:** `package.json` veya `requirements.txt` dosyaları eksik mi veya güncelliğini yitirmiş mi?
- [ ] **Kırık Referanslar:** Import edilen dosyalar fiziksel olarak orada mı? (Broken Imports).
- [ ] **Mantıksal Boşluklar:** Yarım bırakılmış fonksiyonlar veya içi boş `TODO` yorumları var mı?
- [ ] **Sürüm Çakışmaları:** Kullanılan kütüphanelerin sürümleri birbirleriyle uyumlu mu?

### B. Onarım Süreci
Eksik veya hasarlı parçalar tespit edildiğinde AI şu adımları izler:
1. **Teşhis Raporu:** "Depoda şu dosyalar eksik ve şu importlar kırık. Bu haliyle çalışmaz." şeklinde rapor sunar.
2. **Restorasyon Planı:** "Eksik olan X fonksiyonunu mevcut mantığa göre yeniden yazacağım ve dependency'leri güncelleyeceğim." der.
3. **Stabilizasyon:** Önce projeyi "stabil ve çalışır" hale getirir, ardından istenen yeni özellikleri eklemeye başlar.

---
*Bu protokol, çöp koddan altın değerinde çalışan sistemler üretmek için tasarlanmıştır.*
