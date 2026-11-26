# 🔁 ECC'den Alınan İş Akışları (Sadeleştirilmiş)

Bunlar `affaan-m/ECC` projesindeki akışların, bizim "sade Türkçe + otonom" sistemimize uyarlanmış hâlidir. **Hepsinde kullanıcıya soru sorulmaz**; `ORCHESTRATOR.md` kuralları üstünlüğünü korur.

---

## 1. 🔍 Araştırma-Önce (Research-First)
**Tetikleyici:** Yeni bir özellik, kütüphane veya platform işi başlayacakken.

**Adımlar:**
1. Kod yazmadan önce **resmi dokümanları / benzer çözümleri** ara.
2. Bulunan kaynakların adreslerini rapora yaz.
3. Kaynak bulunamadıysa *"resmi kaynak bulamadım, şu varsayımı kullandım"* de.
4. Sonra planı yap.

**Kanıt:** Rapor "__Araştırılan kaynaklar__" listesi. Kayıtsız varsayım = halüsinasyon riski.

## 2. 👁️ Taze Gözle İnceleme (Fresh-Context Review)
**Tetikleyici:** Bir özellik yazılıp test edildikten sonra, "bitti" denmeden önce.

**Adımlar:**
1. Kodu yazan ile inceleyen **ayrı bağlamda** olur (yeni sohbet / yeni ajan).
2. İnceleyen, yazanın raporuna **bakmadan** kodu okur.
3. `KONTROL_LISTESI.md` maddelerini tek tek işaretler.
4. Çelişki varsa düzeltme ister; yazan kendi işini onaylayamaz.

**Kanıt:** İnceleme raporu + bulunan eksikler listesi (boş olabilir ama rapor olmalı).

## 3. ✅ Doğrulama Döngüsü (Verification Loop)
**Tetikleyici:** Her kod değişikliğinde.

**Adımlar:**
1. Çalıştır → log al → hata varsa nedenini yaz → düzelt → **tekrar çalıştır**.
2. En az **2 tur** doğrulama yapılır (ilk tur hata bulsa da bulmasa da).
3. Her turun çıktısı kanıt klasörüne kaydedilir.
4. Son tur **yeşil** (EXIT 0) olmadan "bitti" denmez.

**Kanıt:** Tur logları + son tur çıktısı.

## 4. 🧠 Oturum Özeti + Eğilim (Session Memory & Instinct)
**Tetikleyici:** Her önemli aşamanın sonunda ve uzun sohbet bitmeden önce.

**Adımlar:**
1. `CLAUDE.md` / proje hafıza dosyasına **kısa** bir durum notu yaz.
2. Tekrarlanan başarıları "kesin kural" değil, **"eğilim"** olarak kaydet: *"Bu projede X yaklaşımı iki kez işe yaradı."*
3. Yeni sohbette bu notlar okunur; aynı hatalar tekrar yapılmaz.

**Kanıt:** Güncellenmiş hafıza dosyası + son commit hash'i.

---

## ⚖️ Uyum Kuralı
Bu akışlar `WORKFLOW.md`'deki ana üretim bandını **uzatır, değiştirmez**. Otonom modda hiçbiri kullanıcıya soru sormaz; sadece rapora kanıt ekler.

> Not: ECC'nin 72 ajan / 328 skill / hook altyapısı buraya **alınmadı**. Sadece davranış fikirleri alındı; sistem küçük ve okunabilir kaldı.
