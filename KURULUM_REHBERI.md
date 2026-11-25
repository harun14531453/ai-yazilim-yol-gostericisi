# ⚙️ Kurulum ve Kaynak Sağlama Rehberi (Provisioning)

Bir araç/uygulama yapılırken gereken programların **nereden alınacağını, hangi sürümün kurulacağını ve nasıl doğrulanacağını** bu belge belirler. AI, gerekli kurulumu kendi yapar; kullanıcıya sadece gerçekten gerektiğinde sorar.

---

## 🎯 Temel Kural
> **Ücretsiz + resmi kaynak + kullanıcı haklarıyla yapılabilen kurulum = Otonom (soru yok).**
> **Ücretli / yönetici hakkı / bilinmeyen kaynak = Dur ve sor.**

Bu kural, `ORCHESTRATOR.md`'deki 4 güvenlik frenini genişletmez; sadece "kurulum" konusuna uygular.

---

## 1. Adım: İhtiyaç Analizi
Kod yazmaya başlamadan önce AI şunu çıkarır:
- Hangi çalışma zamanı gerekli? (Node.js, Python, Java/JDK, .NET)
- Hangi SDK / araç gerekli? (Android SDK, Xcode, Gradle, npm, pip)
- Hangi paketler gerekli? (kütüphaneler)
- Bunlardan hangileri zaten kurulu? (`node --version`, `python --version`, `java -version` vb. ile kontrol)

**ÇIKTI:** "Kurulacaklar listesi" (araç + sürüm + neden).

## 2. Adım: Kaynak Seçimi (Güven Zinciri)

| Kaynak | Kullanılabilir mi? | Not |
|---|---|---|
| Resmi paket yöneticileri (`winget`, `npm`, `pip`, `dotnet`, `scoop`, `choco`, `brew`, `apt`) | ✅ Evet | Tercih edilen yol |
| Resmi proje siteleri / GitHub releases | ✅ Evet | İmzalı sürüm, checksum kontrolü |
| Kullanıcı haklarıyla (`--scope user`, `venv`, proje içi `node_modules`) | ✅ Evet | Sistem geneline dokunmaz |
| Yönetici hakları gerektiren kurulum | ⛔ Dur, sor | 4. güvenlik freni |
| Ücretli lisans / ücretli servis | ⛔ Dur, sor | 4. güvenlik freni |
| Bilinmeyen site, `.exe` indirme, "crack", lisans bypass | ❌ Asla | Güvenlik ihlali |
| `curl ... \| sh` tarzı kaynaksız script | ❌ Asla | İçerik okunmadan çalıştırılmaz |

**İmza/checksum:** Mümkün olan her yerde sürüm sabitlenir (`package-lock.json`, `requirements.txt`, `poetry.lock`). Rastgele "latest" kullanılmaz.

## 3. Adım: Kurulum (Otonom)
- Mümkün olduğunca **proje içi / kullanıcı seviyesinde** kurulum yapılır; sistem genelini değiştirmez.
- Global kurulum yalnızca gerçekten gerekliyse yapılır ve rapora yazılır.
- Kurulumdan sonra **sürüm kontrolü** yapılır: `node --version` vb. Çıktı kanıt olarak saklanır.
- Kurulum başarısız olursa: hata mesajı okunur, nedeni açıklanır, alternatif resmi kaynak denenir. Yine olmazsa durup kullanıcıya net hata ile rapor verilir.

## 4. Adım: Kayıt ve Geri Alma
Her kurulum şunları rapora / `INSTALL.md` dosyasına yazılır:
- Araç adı + sürüm
- Kaynak (hangi paket yöneticisi/site)
- Neden gerekli
- Kaldırma (uninstall) komutu — kullanıcı isterse geri alınabilsin

Proje hafıza dosyasına (`CLAUDE.md`) **"Kurulu araçlar ve sürümleri"** bölümü eklenir. Yeni sohbette AI bunu okur, tekrar kurulum yapmaz.

## 5. Adım: Tekrar Kurulumu Önleme
Yeni sohbette veya aynı projede:
1. Önce `CLAUDE.md` okunur.
2. Sonra sürüm kontrolleri çalıştırılır.
3. Zaten kurulu olan araç **tekrar kurulmaz**; sadece doğrulanır.
4. Eksik olanlar eklenir.

---

## 🧩 Platforma Özel Notlar

**Windows:** `winget` (kullanıcı scope) tercih edilir. Yönetici isteyen kurulumlarda durulur.
**Android:** Android SDK / command-line tools resmi kaynaktan alınır; Gradle sürümü proje dosyasına sabitlenir. Emulator büyük kaynak tükettiği için kurulmadan önce gerekirliliği değerlendirilir; kurulmazsa "cihaz testi yapılamadı" raporlanır.
**iOS:** macOS/Xcode zorunludur; Windows'ta kurulamaz → 4. güvenlik freni (bkz. PLATFORM_REHBERI.md).
**Python:** `venv` içine kurulum; sistem Python'una dokunulmaz.
**Web:** `npm install` proje içinde yapılır; `node_modules` asla commit edilmez.

---

## ⛔ Yasaklar
- Kullanıcı izni olmadan ücretli hiçbir şey kurmak.
- Sistem ayarlarını, PATH'i veya kritik dosyaları sessizce değiştirmek.
- Kaynağı doğrulanmamış binary çalıştırmak.
- Kurulumu "yaptım" diye raporlayıp sürümünü doğrulamamak.
- Zaten kurulu bir aracı tekrar kurmak.

---
*Bu rehber sayesinde kullanıcı "hangi programı nereden indireceğim?" diye düşünmez; AI kurar, doğrular, yazar ve geri alma yolunu da bırakır.*
