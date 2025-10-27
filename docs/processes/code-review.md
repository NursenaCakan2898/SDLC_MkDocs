# 🔍 Kapslı Kod Gözden Geçirme (Code Review) & Pull Request (PR) Kılavuzu

---

## 1. Kod Gözden Geçirmenin Önemi

Kod Gözden Geçirme (Code Review), bir geliştiricinin yazdığı kodun, başka bir (veya daha fazla) ekip üyesi tarafından sistematik olarak incelenmesi sürecidir. Bu, bir Pull Request (PR) veya Merge Request (MR) üzerinden yapılır.

Bu süreç, bir "hata bulma" seansından çok daha fazlasıdır; projenin sağlığı, kalitesi ve sürdürülebilirliği için **en kritik** süreçlerden biridir.

* **Hata Tespiti (Bug Detection):** Hataları (mantıksal, algoritmik) henüz geliştirme aşamasındayken yakalamanın en ucuz ve en hızlı yoludur. Canlı (production) ortama ulaşan bir hatayı düzeltmenin maliyeti, bu aşamada düzeltmenin maliyetinden katbekat fazladır.
* **Kod Kalitesi ve Tutarlılık (Code Quality & Consistency):** Ekibin üzerinde anlaştığı kodlama standartlarının (coding standards), isimlendirme kurallarının ve tasarım desenlerinin (design patterns) korunmasını sağlar. Bu, kod tabanının "temiz" (clean code), okunabilir ve sürdürülebilir kalmasını sağlar.
* **Bilgi Paylaşımı ve Mentorluk (Knowledge Sharing & Mentorship):** Projedeki bilgi birikimini tek bir kişiden (bus factor) alıp tüm ekibe yayar. Kıdemli (senior) geliştiriciler için kıdemsiz (junior) olanlara mentorluk yapma, kıdemsizler için de kıdemlilerin tecrübelerinden öğrenme fırsatıdır.
* **Ortak Sorumluluk (Shared Ownership):** "Bu *benim* kodum" anlayışını, "Bu *bizim* kodumuz" anlayışına dönüştürür. Ekipteki herkes kod tabanının farklı bölümleri hakkında fikir sahibi olur ve sorumluluk alır.
* **Tasarım ve Mimari İyileştirme (Design & Architectural Improvement):** Farklı bir bakış açısı, mevcut bir tasarım sorununu, potansiyel bir performans darboğazını veya mimari bir tutarsızlığı (geliştiricinin gözden kaçırdığı) ortaya çıkarabilir.

## 2. Roller ve Sorumluluklar

* **Yazar (Author / Submitter):**
    * Kodu yazan, değişikliği yapan ve Pull Request'i (PR) açan kişidir.
    * Sorumluluğu, PR'ı *gözden geçirilmeye hazır* halde sunmaktır (Bkz. En İyi Uygulamalar).
    * Gelen geri bildirimleri (feedback) anlamak, savunmacı olmadan değerlendirmek ve gerekli düzeltmeleri uygulamakla yükümlüdür.
* **Gözden Geçiren (Reviewer / Approver):**
    * Kodu inceleyen, geri bildirimde bulunan ve son onayı (Approve) veren kişidir.
    * Sorumluluğu, değişikliği aşağıdakiler de dahil olmak üzere birçok açıdan (Bkz. 3. Kriterler) *detaylıca* incelemektir.
    * Geri bildirimi *yapıcı, saygılı ve net* bir dille iletmekle yükümlüdür.

## 3. Kod Gözden Geçirme Kriterleri (Neye Bakılmalı?)

İyi bir gözden geçirme, aşağıdaki ana kriterleri kapsamalıdır:

### 3.1. Tasarım ve Mimari (Design & Architecture)
* **Prensip Uyumu:** Kod, SOLID, DRY (Don't Repeat Yourself), KISS (Keep It Simple, Stupid) gibi temel yazılım prensiplerine uyuyor mu?
* **Mimari Uyum:** Değişiklik, projenin genel mimarisine (örn. Mikroservis, Katmanlı Mimari, MVVM) uygun mu? Gereksiz yere mimariyi karmaşıklaştırıyor veya "kısayollar" (shortcut) mı içeriyor?
* **Tasarım Desenleri (Design Patterns):** Problemi çözmek için doğru tasarım deseni kullanılmış mı? Veya gereksiz yere karmaşık bir desen (over-engineering) mi uygulanmış?

### 3.2. Okunabilirlik ve Sürdürülebilirlik (Readability & Maintainability)
* **İsimlendirme:** Değişken, fonksiyon, sınıf ve metot isimleri *anlamlı, net ve tutarlı* mı? (`data`, `temp`, `list1` gibi belirsiz isimler var mı?)
* **Kodlama Standartları:** Ekibin kodlama standartlarına (Linter - örn. PEP8, ESLint, Checkstyle) uyulmuş mu?
* **Karmaşıklık (Complexity):** Fonksiyonlar/metotlar çok mu uzun? (İdeal olarak bir fonksiyon *tek bir iş* yapmalıdır - Single Responsibility Principle). İç içe (nested) `if/for` döngüleri çok mu fazla? (Sikklomatik karmaşıklık yüksek mi?)
* **"Sihirli Sayılar" (Magic Numbers):** Kodun içinde ne anlama geldiği belli olmayan sayılar (örn. `if (status == 3)`) var mı? Bunlar `const` veya `enum` olarak tanımlanmalı mı?

### 3.3. Test Edilebilirlik ve Test Kapsamı (Testability & Coverage)
* **Test Edilebilirlik:** Yazılan kod *test edilebilir* mi? Bağımlılıklar (örn. veritabanı, API) soyutlanmış mı (Dependency Injection), yoksa kod "hard-coded" bağımlılıklar içeriyor mu?
* **Test Ekleme:** Yeni eklenen özellik veya mantık için **Birim Testleri (Unit Tests)** eklenmiş mi?
* **Test Güncelleme:** Mevcut bir özellik değiştirildiyse, ilgili testler güncellenmiş mi?
* **Test Kapsamı:** Testler sadece "mutlu yolu" (happy path) mu, yoksa *negatif senaryoları* (negative path) ve *sınır durumlarını* (edge cases - örn. boş liste, null değer, sıfıra bölme) da kapsıyor mu?

### 3.4. Güvenlik (Security)
Bu, özellikle Web, API ve Mobil uygulamalar için hayati önem taşır.
* **Girdi Doğrulama (Input Validation):** Kullanıcıdan (veya başka bir servisten) gelen *tüm* girdiler doğrulanıyor ve temizleniyor mu (sanitize)?
    * **SQL Injection (SQLi):** Sorgular parametreli (parameterized) mi, yoksa string birleştirme (`+`) ile mi yapılıyor?
    * **Cross-Site Scripting (XSS):** Kullanıcıdan gelen veri, HTML'e basılmadan önce encode ediliyor mu?
* **Yetkilendirme (Authorization):** Kod, işlemi yapan kullanıcının bu işlemi yapmaya *yetkisi olup olmadığını* kontrol ediyor mu? (örn. "Normal bir kullanıcı, başka bir kullanıcının sipariş detayını görebiliyor mu?" - IDOR açığı).
* **Hassas Veri (Sensitive Data):** Şifreler, API anahtarları, token'lar *asla* log dosyalarına, konsola veya hata mesajlarına yazılmamalıdır.

### 3.5. Performans (Performance)
* **Veritabanı Erişimi:** Döngü içinde veritabanı sorgusu (N+1 problemi) var mı? Sorgular verimli mi, `index` kullanıyor mu?
* **Algoritmik Verimlilik:** Verimsiz bir algoritma (örn. büyük bir listede O(n^2) arama) veya veri yapısı (örn. sık arama yapılan yerde `List` yerine `Dictionary/Map` kullanmak) seçilmiş mi?
* **Kaynak Kullanımı:** (Özellikle Masaüstü, Mobil, Gömülü/Endüstriyel için)
    * Hafıza sızıntısı (memory leak) var mı? (Açılan kaynaklar - dosya, soket, veritabanı bağlantısı - kapatılıyor mu?)
    * Arayüzü (UI Thread) kilitleyen uzun süreli bir işlem (I/O, hesaplama) var mı? (Bu işlemler `async` veya `background thread` ile yapılmalı).
    * (Endüstriyel) PLC'ye çok sık (örn. 5ms) ve gereksiz sorgu atılıyor mu (ağ yükü)?

### 3.6. Belgeleme (Documentation)
* **Kod Yorumları (Comments):** Kod *ne yaptığını* değil (bu zaten kodun kendisidir), *neden yaptığını* açıklamalıdır. Karmaşık bir iş mantığını veya optimizasyonun sebebini açıklayan yorumlar eklenmiş mi?
* **Doküman Güncelleme:**
    * (Gerekliyse) API dokümantasyonu (Swagger, OpenAPI) güncellenmiş mi?
    * (Gerekliyse) `README.md` veya diğer kullanıcı/geliştirici dokümanları güncellenmiş mi?

---

## 4. Pull Request (PR) Şablonu / Kontrol Listesi

İyi bir PR süreci, PR'ı açan yazarın doldurması gereken standart bir şablonla (template) başlar. Bu, gözden geçirenin işini *muazzam* düzeyde kolaylaştırır.

```markdown
---
### İlgili Görev (Related Task/Issue)
### Değişikliğin Türü (Type of Change)
- [ ] 🐞 Hata Düzeltmesi (Bug Fix)
- [ ] ✨ Yeni Özellik (New Feature)
- [ ] ♻️ Kod İyileştirme (Refactor)
- [ ] ⚡ Performans İyileştirmesi (Performance Improvement)
- [ ] 🔒 Güvenlik Düzeltmesi (Security Patch)
- [ ] 📖 Dokümantasyon Güncellemesi (Documentation Update)
- [ ] 🔧 Diğer (Other): ...


### Açıklama (Description)
### Nasıl Test Edildi (How Has This Been Tested?)
- [ ] Birim Testi (Unit Test) - `[Test senaryosunun adı/amacı]`
- [ ] Entegrasyon Testi (Integration Test)
- [ ] Manuel Test (Aşağıdaki adımlar izlendi):
    1. `...`
    2. `...`
- [ ] Endüstriyel/Gömülü Simülasyon (PLC Simülatörü, HMI)


### (Varsa) Ekran Görüntüleri / Videolar
| Öncesi | Sonrası |
| :---: | :---: |
| [Görsel] | [Görsel] |


---
## 🚨 Yazarın Kontrol Listesi (Author's Checklist)
- [ ] Kodum `develop` (veya `main`) dalındaki son değişiklikleri içeriyor (Rebase/Merge yapıldı).
- [ ] Kodum, ekibin kodlama standartlarına (Linter) uyuyor.
- [ ] Anlamlı ve temiz commit mesajları yazdım (Bkz. `branching.md`).
- [ ] Yeni kodum için **Birim Testleri (Unit Tests)** ekledim.
- [ ] Mevcut testleri değişikliğime göre güncelledim.
- [ ] Tüm testler (yeni ve mevcut) yerel makinemde `%100` başarılı (Pass).
- [ ] Kodumun yeni bir **güvenlik açığı** yaratmadığını kontrol ettim (örn. SQLi, XSS, yetkilendirme).
- [ ] (Varsa) API dokümantasyonunu (Swagger/OpenAPI) güncelledim.
- [ ] (Varsa) `README.md` veya diğer ilgili dokümanları güncelledim.

## Reviewer (Gözden Geçiren) Kontrol Listesi
- [ ] **Tasarım:** Kod, SOLID prensiplerine ve genel mimariye uyuyor mu?
- [ ] **Okunabilirlik:** İsimlendirmeler (değişken, fonksiyon) açık ve net mi?
- [ ] **Test:** Testler yeterli mi? Negatif senaryolar (edge cases) düşünülmüş mü?
- [ ] **Performans:** Kodda bariz bir performans sorunu (N+1 sorgusu, sonsuz döngü) var mı?
- [ ] **Güvenlik:** Kullanıcı girdisi güvenli bir şekilde işleniyor mu?
- [ ] **Belgeleme:** Gerekli yorumlar ve doküman güncellemeleri yapılmış mı?
---