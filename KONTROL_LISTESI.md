# ✅ Nihai Teslimat Kontrol Listesi (Acceptance Checklist)

AI bir işi "bitti" diye teslim etmeden önce bu listedeki her maddenin işaretli olması zorunludur. Eksik kalan maddeler varsa iş tamamlanmış sayılmaz.

> **Sıfırdan projelerde** klon/hasar maddeleri *"N/A (boş klasörden başlandı)"* olarak işaretlenir; bu eksik sayılmaz.

## 🏁 "Bitti"nin Tam Tanımı
- **Tamamlandı** diyebilmek için: bütün zorunlu kutular işaretli olmalı **veya** kalanlar *"N/A"* / *"kanıtlanmış kısıtlama"* olarak raporlanmış olmalı.
- "Test edilemeyen özellikler açıkça belirtildi" maddesi, kanıtlanmış bir sınır olarak teslimi geçerli kılar; **ancak** raporun başlığı *"Kısmi teslim"* olur, *"Tamamlandı"* değil.
- Yapılamayan bir işi yapılmış gibi anlatmak (EXE hazır, test edildi vb.) **halüsinasyondur ve yasaktır.** Sınırı açıkça söylemek her zaman daha iyidir.

## 🔍 Hasar / Eksik Analizi
- [ ] Depo klonlandı **veya** yerel kopya sağlandı ve dosya yapısı tarandı.
- [ ] Eksik bağımlılıklar (`package.json`, `requirements.txt` vb.) kontrol edildi.
- [ ] Kırık referanslar / import'lar tespit edilip raporlandı.
- [ ] Yarım kalmış `TODO` ve boş fonksiyonlar tarandı.
- [ ] Sürüm çakışmaları kontrol edildi. (Harici bağımlılık yoksa "N/A" işaretlenir, eksik sayılmaz.)
- [ ] Tespit edilen hasarlar onarıldı, proje stabilize edildi.

## 🔐 Güvenlik ve Kalite
- [ ] Gizli veriler (API key, şifre) kod içinde değil, `.env` dosyasında.
- [ ] `.env` ve hassas dosyalar `.gitignore` ile korunuyor.
- [ ] `.env.example` şablonu oluşturuldu (gerçek değer içermez).
- [ ] Kod okunabilir, anlamlı isimlendirme ve yorum satırları mevcut.
- [ ] Tek dosyaya yığılmış spagetti kod yok, modüler yapı var.

## 🧪 Doğrulama Kanıtı
- [ ] Program gerçekten çalıştırıldı ve çıktı logu paylaşıldı.
- [ ] İstenen özellikler tek tek test edildi.
- [ ] Test edilemeyen özellikler açıkça belirtildi.
- [ ] Hata mesajları kontrol edildi, konsol temiz.
- [ ] Kapatıp yeniden açıldığında çalışıyor.

## 📦 Teslimat ve Kayıt
- [ ] Son çalışan sürüm Git ile yerel olarak kaydedildi.
- [ ] Kurulum klavuzu (`INSTALL.md`) hazır.
- [ ] Nasıl çalıştırılacağı sade bir dille anlatıldı.
- [ ] Proje hafıza dosyası (`CLAUDE.md`) güncellendi.
- [ ] Bilinen kısıtlamalar / ertelenen işler listelendi.

---
**Kural:** Bir kutu bile boş kalırsa, AI "bitti" diyemez. Kullanıcıya hangi maddenin neden eksik olduğunu açıklamak zorundadır.
