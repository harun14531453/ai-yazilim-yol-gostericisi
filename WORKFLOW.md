# 🔄 Geliştirme İş Akışı (Workflow)

Bir talebin gelişiyle, teslimatı arasındaki zorunlu döngüdür. Adımlar atlanamaz.

## 🏁 Döngü Şeması
`Talep` $\rightarrow$ `Analiz & Sorgulama` $\rightarrow$ `Taslak Plan` $\rightarrow$ `Onay` $\rightarrow$ `Safe Zone (Branch)` $\rightarrow$ `Kodlama` $\rightarrow$ `Doğrulama` $\rightarrow$ `Commit` $\rightarrow$ `Rapor`

---

## 📝 Detaylı Adımlar

### 1. Talep ve Analiz
- Kullanıcı bir istek iletir.
- AI, isteği analiz eder. Eksik bilgi varsa (örn: "Veritabanı tercihin nedir?", "Renk paleti nasıl olsun?") soruları sorar.
- **ÇIKTI:** Netleştirilmiş gereksinimler listesi.

### 2. Stratejik Planlama
- AI, uygulanacak adımları sıralar.
- Hangi dosyalar etkilenecek? Hangi yeni kütüphaneler kurulacak?
- Potansiyel riskler nelerdir?
- **ÇIKTI:** Onaya sunulan madde madde uygulama planı.

### 3. Uygulama (Build Phase)
- Plan onaylandığında AI işe başlar.
- Değişiklikler küçük parçalar halinde yapılır.
- Her anlamlı blok sonrası "ara kontrol" yapılır.

### 4. Deterministik Doğrulama (Verification)
- AI, "Bitti" demeden önce şu kanıtlardan en az birini sunar:
    - ✅ Terminal üzerinden alınan başarılı çalıştırma logu.
    - ✅ Browser konsolu temiz görüntü raporu.
    - ✅ Fonksiyonelliği kanıtlayan ekran görüntüsü veya çıktı metni.
    - ✅ Yazılmış ve başarıyla geçmiş Unit Test sonucu.
- **KRİTER:** Kullanıcı kanıtı görmeden adım tamamlanmış sayılmaz.

### 5. Kalıcı Kayıt (Checkpoint)
- Çalışan hal onaylandığında Git commit atılır.
- Commit mesajı anlamlı ve sade olmalıdır (Örn: `feat: uçak takip özelliği eklendi`).
- Proje hafıza dosyası (`CLAUDE.md` vb.) güncellenir.

### 6. Nihai Rapor ve Kapanış
- Yapılanlar özetlenir.
- "Plandan sapmalar" varsa açıklanır.
- Bir sonraki adım önerilir.
