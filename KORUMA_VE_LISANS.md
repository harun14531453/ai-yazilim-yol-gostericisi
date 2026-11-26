# 🛡️ Kaynak Kodu Koruma ve Lisans Stratejisi

Ürettiğimiz araçların **izinsiz kopyalanmasını, tersine mühendislikle çözülmesini ve ticari sır haline gelen mantığın sızdırılmasını caydırmak** için kullanılır.

> ⚠️ **Dürüst başlangıç:** Hiçbir yöntem tersine mühendisliği **tamamen önleyemez**. Hedef; caydırmak, tespit etmek, haklarımızı koruyacak kanıtı üretmek ve maliyeti yükseltmektir. "Koruma kesindir" demek halüsinasyondur.

## ⚙️ Varsayılan Davranış (Yeni Kural)
Kullanıcı bir araç isteyip **"açık kaynak olsun" / "MIT lisanslı olsun" / "herkes görebilir"** demediyse, üretilen araç **varsayılan olarak özel/kapalı kaynak** sayılır ve bu dosyadaki koruma katmanı uygulanır.

- **Açıkça açık kaynak istenirse:** Koruma eklenmez, `LICENSE.md` (MIT) kullanılır, kaynak paylaşılabilir.
- **Belirsiz veya kapalı/ticari:** Temel koruma zorunludur → source map kapalı, istemciye sır yok, lisans/EULA dosyası, private repo uyarısı, hash/checksum listesi.
- **Tam koruma** (obfuscate, code signing, lisans anahtarı): ürün ücretli/ödeme içeriyorsa veya kullanıcı isterse eklenir.
- AI rapora şunu yazar: **"Bu araç varsayılan olarak korumalı üretildi. Eklenen korumalar: [...] . Açık kaynak isteseydiniz MIT lisansı kullanılırdı."**

> Bu karar **kullanıcıya sorulmaz** (`ORCHESTRATOR.md` soru yasağı geçerlidir); varsayılır ve raporda kanıtıyla belirtilir.

---

## 1. Yasal Katman (En güçlü kalkan)

| Durum | Ne yapılmalı |
|---|---|
| Açık kaynak paylaşımı | MIT / Apache lisansı zaten var → **herkes okuyabilir**, koruma beklemeyin |
| Ticari / kapalı kaynak | **`LICENSE.md` yerine özel lisans + EULA** yazılır; kaynak repo **private** tutulur |
| Ekip / müşteri | **NDA** + katkıda bulunan anlaşması (CLA) |
| Sır niteliğindeki algoritma | Kaynak kod public repo'ya konmaz; sadece derlenmiş çıktı dağıtılır |

**AI kuralı:** AI, müşteriye/üçüncü şahsa **kaynak dosyaları vermez**; sadece çalışan paketi, kurulum dosyasını ve izin verilen dokümanı verir.

## 2. Teknik Katman (Dağıtım koruması)

**Web / Node / Electron**
- Dağıtıma **minify + obfuscate** edilmiş kod gider (terser / esbuild).
- `.map` (source map) dosyaları **asla yayınlanmaz**; üretim derlemesinde `sourceMap: false`.
- `.env`, API anahtarı, gizli sabit istemciye **gömülmez**; sunucuda tutulur.

**Python**
- Kaynak `.py` yerine **PyInstaller / Nuitka** ile derlenmiş paket dağıtılır.
- `__pycache__` ve `.pyc` bile Şifreli/paketli dağıtımda gizlenir.
- Gizli anahtarlar dosyaya değil, ortam değişkeni / güvenli depoya konur.

**.NET / Windows**
- Single-file + trim ile dağıtım, **Authenticode imzası**.
- Strong-name / assembly imzası.
- Kritik dosyaların **hash bütünlük kontrolü** açılışta yapılır.

**Android / iOS**
- Keystore / sertifika ile **imzalı APK/IPA**; imzalama dosyaları Git'e girmez (bkz. `PLATFORM_REHBERI.md`).
- iOS'ta App Store dağıtımı zaten imza + notarization zorunluluğu sağlar.

**Lisans anahtarı / aktivasyon**
- Çevrimdışı: HMAC imzalı lisans dosyası.
- Çevrimiçi: sunucu tarafında aktivasyon kontrolü.
- ⚠️ İstemci tarafındaki lisans kontrolü **aşılabilir**; asıl doğrulama sunucuda olmalı.

## 3. Süreç Katmanı

- Kaynak repo **private**, dağıtım için ayrı bir **release artifact** üretilir.
- CI/CD'de secret'lar ortam değişkeni olarak tutulur; loglara yazılmaz.
- Bağımlılıklar sabitlenir (`package-lock.json`, `requirements.txt`) → tedarik zinciri saldırısı riski azalır.
- Her sürümde **hash listesi (SBOM + checksum)** yayınlanır; değiştirilmiş paket tespit edilebilir.
- Erişim yetkileri en az yetki ilkesiyle verilir; eski çalışanların erişimi iptal edilir.

## 4. AI İçin Koruma Kuralları

**K1 — Kaynak sızıntısı yok:** AI, korumalı bir projenin kaynak kodunu izinsiz olarak sohbete, public repo'ya veya üçüncü şahsa aktarmaz.

**K2 — Lisans ihlali yok:** Açık kaynak lisansını koruma sanıp ihlal etmez; kapalı kaynak projenin EULA'sını uygular.

**K3 — İstemciye gizli veri yok:** API key, şifre, özel algoritma sabiti istemci paketine gömülmez.

**K4 — Koruma kanıtlı olur:** "Korumalı" demeden önce şunları çalıştırıp çıktıyı rapora koyar:
```
sourceMap kapalı mı?   → derleme konfigürasyonu
.map dosyası var mı?   → dosya listesi (olmalı: 0)
minify uygulandı mı?   → çıktı boyutu / içerik kontrolü
imza var mı?           → imza doğrulama komutu
hash listesi var mı?   → checksum dosyası
```

**K5 — Kötü niyetli koruma yasak:** Kullanıcıya zarar veren, gizli veri toplayan, güvenlik araştırmacısını suç sayan veya sistemı bozan "anti-debug" teknikleri **kullanılmaz**. Koruma caydırıcı ve şeffaf olur.

**K6 — Tersine mühendislik tespiti:** Bir paketin izinsiz açıldığı/kopyalandığı şüphesi varsa AI kanıtı raporlar (hash uyuşmazlığı, imzasız sürüm), ancak intikam niteliğinde kod yazmaz.

---

## 5. Ne Yapmamalı?

❌ "Şifrelendi, kimse çözemez" demek  
❌ Kaynak kodu public repo'ya koyup gizli sanmak  
❌ API anahtarını obfuscate edip güvenli sanmak  
❌ Lisans kontrolünü sadece istemciye bırakmak  
❌ Kullanıcı makinesini gizlice izleyen koruma eklemek  

**Özet:** MIT lisanslı bu navigator deposu **açık kaynaktır**; bu dosya, onunla **yapılacak ticari/kapalı ürünler** içindir. Açık kaynak ile gizlilik bir arada olamaz; gerekirse ürün için ayrı özel lisans yazılır.

---
*Koruma = yasal hak + teknik caydırma + kanıt. Üçü birden olmadan "korundu" denmez.*
