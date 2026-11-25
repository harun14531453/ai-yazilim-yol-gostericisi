# 📱 Platform Rehberi (Android / iOS / Windows / Web)

## 🔒 Değişmeyen Kural
**Anayasa, iş akışı, modlar, otonom karar motoru ve kalite/güvenlik kuralları platformdan bağımsızdır.**
Bir Android uygulaması da, bir iOS uygulaması da aynı döngüden geçer:
`Analiz → Hasar Tespiti → Plan → Onay → Yaz → Kanıtla → Kaydet → Teslim Et`

Değişen sadece üç şeydir: **kurulum aracı**, **test kanıtının türü** ve **dağıtım (mağaza) adımları**.

---

## 🗺️ Platform Karşılaştırma Tablosu

| Konu | 🤖 Android | 🍎 iOS | 🖥️ Windows | 🌐 Web |
|---|---|---|---|---|
| **Ana Dil** | Kotlin / Java | Swift | C# / Python | JS / TypeScript |
| **Geliştirme Aracı** | Android Studio + Gradle | Xcode + Swift Package Manager | Visual Studio / VS Code | VS Code |
| **Paket Yöneticisi** | Gradle | CocoaPods / SPM | NuGet / pip | npm / pnpm |
| **Test Kanıtı** | Emulator ekran görüntüsü + `adb logcat` | Simulator ekran görüntüsü + Xcode logu | Terminal çıktısı / EXE çalıştırma | Tarayıcı konsolu + localhost görüntüsü |
| **Derleme Komutu** | `./gradlew assembleDebug` | `xcodebuild` | `dotnet build` / `pyinstaller` | `npm run build` |
| **İzin Tanımı** | `AndroidManifest.xml` | `Info.plist` | Uygulama ayarları | Tarayıcı izinleri |
| **İmzalama** | Keystore (`.jks`) | Sertifika + Provisioning Profile | Code signing (opsiyonel) | Yok |
| **Mağaza** | Google Play | App Store | Microsoft Store / direkt EXE | Hosting (Vercel, Netlify vb.) |
| **Windows PC'de Tam Yapılabilir mi?** | ✅ Evet | ❌ Hayır (Xcode sadece macOS) | ✅ Evet | ✅ Evet |

---

## ⚠️ En Kritik Fark: iOS ve macOS Zorunluluğu

Bir iOS uygulamasını **gerçekten derleyip test etmek için macOS + Xcode gerekir.** Windows'ta bu mümkün değildir.

AI otonom modda şu duruma düşerse **durmak ve kullanıcıya bildirmek zorundadır** (bkz: ORCHESTRATOR.md güvenlik freni):
- Kullanıcı Windows kullanıyor ama iOS istiyor.
- Çözüm seçenekleri sunulur:
  1. Kullanıcının bir Mac'i varsa orada devam edilir.
  2. Bulut Mac servisi (örn: MacinCloud) kullanılır → **ücretlidir, izin istenir.**
  3. Kod Flutter / React Native ile yazılır, Android tarafı Windows'ta test edilir, **iOS derlemesi için yine bir Mac gerekir** → bu açıkça belirtilir.

> **Yalan söylemek yasaktır:** AI, Windows'ta "iOS uygulamasını test ettim" diyemez. En fazla "kod yazıldı, iOS tarafı derlenip test edilmedi" diyebilir.

---

## 🤖 AI İçin Platform Kuralları

**P1 — Platformu doğrula.** İlk mesajda hedef platformu (Android / iOS / Windows / Web) netleştir. Belirtilmemişse sor **(yalnızca Rehber modda)**; otonom modda AI seçer ve gerekçesini rapora yazar.

**P2 — Hedef sürümü sor (yalnızca Rehber modda; otonomda varsayılır).** "Android 10+ mı? iOS 16+ mı? Eski telefonlar önemli mi?" Bu, kullanılacak kütüphaneleri belirler.

**P3 — Ortamı kontrol et.** Kod yazmaya başlamadan önce gereken araçların (Android SDK, Xcode, Node) mevcut olup olmadığını kontrol et. Eksikse kurulum öncesi kullanıcıya açıkla.

**P4 — Kanıtı platforma göre üret.**
- Android: emulator görüntüsü + logcat çıktısı
- iOS: simulator görüntüsü + derleme logu
- Windows: çalışan EXE / terminal çıktısı
- Web: tarayıcı görüntüsü + konsol logu

**P5 — İzinleri kullanıcıya sorarak al.** Kamera, konum, bildirim, dosya erişimi gibi izinler kullanıcıya açıklanmadan eklenmez. (Acemi/Rehber modda: "Uygulamanın kamerayı kullanabilmesi için bir izin ekleyeceğim, tamam mı?") **(Otonom modda AI makul varsayımla ekler ve raporda belirtir; ücretli izin/servisler 4. güvenlik frenine girer.)**

**P6 — İmzalama dosyalarını koru.** Keystore, sertifika ve provisioning profile dosyaları **asla Git'e eklenmez** (bkz: QUALITY_AND_SECURITY.md ve `.gitignore`).

**P7 — Mağaza kurallarını ihlal etme.** App Store ve Google Play'in kullanıcı gizliliği, reklam ve izin metinleri kuralları kontrol edilir. AI emin değilse "emin değilim, kontrol etmelisin" der.

**P8 — Hafızaya platformu yaz.** `CLAUDE.md` / proje hafıza dosyasında hedef platform, SDK sürümü ve test yöntemi mutlaka yer alır. Yeni sohbette AI bunu ilk okur.

---

## 📱 Hızlı Başlangıç Komutu (Mobil İçin)

Kullanıcı mobilde bir proje istediğinde AI şu soruları sorar **(yalnızca Rehber modda; otonom modda sorulmaz)**:
1. Android mi, iOS mu, yoksa ikisi birden mi? (İkisi için Flutter/React Native önerilir)
2. Hedef telefon / işletim sistemi sürümü?
3. Ücretsiz mi, yoksa ücretli bir servis (Firebase, harita API'si vb.) kullanılabilir mi?
4. Mağaza'ya mı çıkacaksın, yoksa sadece kendi telefonunda mı çalışacak?

Sonrasında standart akışa dönülür. Kurallar aynı, sadece araçlar değişir.

---
*Özet: Platform değişir, disiplin değişmez. AI hangi platformda olursa olsun "kanıtsız bitti" diyemez.*
