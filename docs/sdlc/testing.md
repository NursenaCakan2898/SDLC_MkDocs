# 🛡️ Kapsamlı Test ve Doğrulama Kılavuzu

## 1. Giriş: Doğrulama (Verification) ve Geçerleme (Validation)

Test ve Doğrulama (V&V), bir yazılım ürününün kalitesini, güvenilirliğini ve amacına uygunluğunu temin etmek için yürütülen kritik süreçler bütünüdür. Bu iki terim genellikle birbirinin yerine kullanılsa da, temel bir farkları vardır:

* **Doğrulama (Verification): "Ürünü doğru inşa ettik mi?"**
    * **Odak:** Gereksinimlere ve spesifikasyonlara uygunluk.
    * **Soru:** Kod, tasarım dokümanına, teknik şartnameye ve belirlenen standartlara (örn. kodlama standartları) uyuyor mu?
    * **Yöntemler:** Kod incelemeleri (Code Review), statik analiz, birim testleri (Unit Tests), entegrasyon testleri.
    * **Zamanlama:** Geliştirme süreci *boyunca* yapılır.

* **Geçerleme (Validation): "Doğru ürünü mü inşa ettik?"**
    * **Odak:** Kullanıcı ihtiyacını ve iş amacını karşılama.
    * **Soru:** Ürün, son kullanıcının beklentilerini karşılıyor, sorunlarını çözüyor ve piyasada bir değer yaratıyor mu?
    * **Yöntemler:** Kullanıcı kabul testleri (UAT), E2E testleri, kullanılabilirlik testleri, alfa/beta testleri.
    * **Zamanlama:** Geliştirme süreci *sonunda* (veya her sprint sonunda) yapılır.

Bu kılavuz, "hata bulmak"tan çok daha fazlasını; **risk yönetimi**, **kalite güvencesi (QA)** ve **paydaşlara güven verme** süreçlerini kapsamaktadır.

---

## 2. Test Stratejisinin Omurgası: Test Piramidi

Test Piramidi, bir projenin test stratejisinin maliyet, hız ve güvenilirlik dengesini kuran temel modeldir. Kural basittir: Hızlı, ucuz ve izole testler (Birim) en altta ve en çok sayıda olmalı; yavaş, pahalı ve entegre testler (E2E) ise en üstte ve en az sayıda olmalıdır.

Ters dönmüş bir piramit (çok fazla E2E, çok az Birim Testi) yavaş, kırılgan ve bakımı imkansız bir test süreci yaratır ("Ice Cream Cone Anti-Pattern").

### 🥇 2.1. Seviye 1: Birim Testleri (Unit Tests)

* **Nedir?** Yazılımın en küçük, izole edilebilen parçalarının (bir fonksiyon, bir metot, bir sınıf) tek başına test edilmesidir. Sistemin geri kalanından (veritabanı, API, dosya sistemi) *izole* (genellikle "Mock", "Stub" veya "Fake" kullanarak) çalıştırılır.
* **Amaç:** Kodun algoritmik ve mantıksal olarak doğru çalıştığını doğrulamak.
* **Kimin Sorumluluğunda?** Geliştirici (Developer).
* **Hız:** Çok hızlı (Milisaniyeler içinde binlercesi çalışır).
* **Neden Kritik?**
    1.  **Hızlı Geri Bildirim:** Hatanın *tam olarak* nerede olduğunu anında gösterir.
    2.  **Güvenli Refactor:** Kodun iç yapısını (refactor) değiştirirken, dış davranışını (testleri) bozmadığınızdan emin olmanızı sağlar.
    3.  **Tasarım Aracı (TDD):** Testi önce yazmak (Test-Driven Development), daha modüler ve temiz bir mimariyi zorunlu kılar.
    4.  **Canlı Dokümantasyon:** İyi yazılmış birim testleri, kodun nasıl kullanılması gerektiğini gösteren birer örnek teşkil eder.
* **Tüm Yazılım Türlerinde Örnekler:**
    * **Web/API:** Bir `isValidEmail(string email)` fonksiyonunun `test@test.com` için `true`, `hatali-email` için `false` döndüğünü test etmek.
    * **Masaüstü:** Bir "Hesapla" butonunun çağırdığı `calculatePrice(int quantity, float price)` fonksiyonunun `(10, 5.0)` için `50.0` döndürdüğünü test etmek (Butona tıklamayı *simüle etmez*).
    * **Endüstriyel (Modbus):** `parseTemperaturePacket(byte[] packet)` fonksiyonunun, `[0x01, 0x03, 0x02, 0x00, 0xC8]` (200) verisini `20.0` C dereceye doğru çevirdiğini test etmek.
    * **Gömülü (Embedded):** Bir `calculateSensorChecksum(byte[] data)` fonksiyonunun doğru CRC değerini ürettiğini test etmek.

### 🧩 2.2. Seviye 2: Entegrasyon / Servis Testleri (Integration Tests)

* **Nedir?** İki veya daha fazla birimin (modülün, servisin, katmanın) bir araya gelerek doğru çalışıp çalışmadığını test eder. "İzolasyon" burada biter; bu testler genellikle gerçek (veya test) altyapıya dokunur.
* **Amaç:** Bileşenler arasındaki *bağlantı noktalarını*, *veri alışverişini* ve *arayüzleri* doğrulamak.
* **Kimin Sorumluluğunda?** Geliştirici veya QA Otomasyon Mühendisi.
* **Hız:** Yavaş (Veritabanı bağlantısı, API çağrısı, dosya okuma/yazma içerir).
* **Tüm Yazılım Türlerinde Örnekler:**
    * **Web/API (Servis Testi):** API'ye `POST /api/products` isteği atıp, verinin *gerçek test veritabanına* doğru yazıldığını `GET` ile kontrol etmek.
    * **Masaüstü:** "Dosya -> Kaydet" menü seçeneğini tetikleyip, beklenen `.xml` veya `.json` dosyasının diskte *gerçekten* oluşup oluşmadığını kontrol etmek.
    * **Endüstriyel:** Yazılımın, bir *simüle edilmiş PLC'ye* (veya test PLC'sine) Modbus üzerinden "Start" komutu gönderip, PLC'den doğru "Acknowledge" (Onay) yanıtını aldığını doğrulamak.
    * **Gömülü:** Cihazın, bir sensörden (test sensörü) I2C yoluyla veriyi okuyup, bu veriyi doğru formatta bir SD karta *kaydettiğini* test etmek.
* **Özel Tür: Sözleşme Testleri (Contract Testing)**
    * Özellikle mikroservis mimarilerinde kullanılır. İki servis (örn: `Sipariş Servisi` ve `Ödeme Servisi`) arasındaki entegrasyonu test etmek için servislerin *tamamını* çalıştırmak yerine, "Sözleşme" (Contract) kullanılır.
    * **Süreç:** `Consumer` (Tüketici) beklediği istek/yanıt formatını (`Sözleşme`) tanımlar. `Provider` (Sağlayıcı) bu sözleşmeye uyup uymadığını *izole* bir şekilde test eder. Bu, tam entegrasyon testlerinden çok daha hızlıdır ve servislerin bağımsız geliştirilmesine olanak tanır.

### 🎭 2.3. Seviye 3: Uçtan Uca (E2E) / UI Testleri

* **Nedir?** Tüm sistemin bir arada olduğu, *gerçek bir kullanıcı senaryosunun* baştan sona simüle edilmesidir. Genellikle Grafik Kullanıcı Arayüzü (GUI) üzerinden yapılır. Bu testler, sistemin bir bütün olarak çalışıp çalışmadığını doğrular (Sistem Testi'nin bir parçasıdır).
* **Amaç:** Sistemin bir bütün olarak, gerçek dünya koşullarında bir iş akışını tamamlayabildiğini doğrulamak.
* **Kimin Sorumluluğunda?** QA Otomasyon Mühendisi veya Manuel Test Uzmanı.
* **Hız:** Çok Yavaş (Dakikalar sürebilir).
* **Neden Az Sayıda Olmalı?** Çok *kırılgandır* (UI'daki küçük bir buton değişikliği testi bozabilir) ve bakımı *pahalıdır*. Sadece en kritik "Mutlu Yol" (Happy Path) senaryoları (ve birkaç kritik olumsuz senaryo) için kullanılmalıdır.
* **Tüm Yazılım Türlerinde Örnekler:**
    * **Web (UI Testi):** Tarayıcıyı aç (Selenium/Cypress), siteye git, "login" butonuna tıkla, kullanıcı adı/şifre gir, "Giriş Yap"a tıkla, ana sayfada "Hoş Geldin, cakan" yazısını gör.
    * **Masaüstü (UI Testi):** Uygulamayı başlat (WinAppDriver/PyAutoGUI), "Proje Aç" diyaloğunu aç, bir proje dosyası seç, verinin yüklenmesini bekle, bir ayarı değiştir, "Kaydet"e bas, uygulamayı kapat.
    * **Endüstriyel (HMI Testi):** HMI ekranını (SCADA) aç, "Reçete" sayfasına git, "Reçete 5"i seç, "Üretime Yükle" butonuna bas, PLC'deki ilgili Data Block'ların (DB) güncellendiğini ve HMI'daki durumun "Başarılı"ya döndüğünü doğrula.

### 🏁 2.4. Piramidin Zirvesi: Kabul Testleri (Acceptance Testing)

* **Nedir?** Yazılımın, müşterinin veya son kullanıcının gereksinimlerini ve beklentilerini karşılayıp karşılamadığını belirlemek için yapılan resmi test seviyesidir. Bu, genellikle manuel olarak yapılır.
* **Amaç:** "Doğru ürünü mü inşa ettik?" (Validation) sorusunu yanıtlamak.
* **Türleri:**
    * **Kullanıcı Kabul Testi (UAT - User Acceptance Testing):** Ürünün *gerçek son kullanıcılar* tarafından, kendi ortamlarında (veya bu ortama çok yakın bir ortamda) test edilmesi. (Örn: "Operatörün, yeni HMI ekranını kullanarak bir üretimi başlatıp bitirebilmesi.")
    * **Alfa Testi (Alpha Testing):** Ürünün, *şirket içindeki* testçiler veya gönüllü bir iç kullanıcı grubu tarafından test edilmesi (Geliştirme ortamında).
    * **Beta Testi (Beta Testing):** Ürünün, *şirket dışındaki* sınırlı sayıda gerçek kullanıcıya (beta tester) sunularak, gerçek dünya verileriyle geri bildirim toplanması.

---

## 3. Temel Test Türleri: Fonksiyonel ve Fonksiyonel Olmayan

Testler sadece seviyelere (piramit) ayrılmaz, aynı zamanda *amaçlarına* göre de sınıflandırılır.

### 3.1. Fonksiyonel Testler (Functional Testing)

Sistemin "ne yapması gerektiğini" (iş gereksinimlerini) test eder.

* **Kara Kutu Testi (Black-Box Testing):**
    * Sistemin iç yapısını (kodunu) *bilmeden*, sadece girdilere karşılık beklenen çıktıların doğrulanmasıdır. Kullanıcı gibi davranılır.
    * *Örnek:* Web sitesine "12345" şifresini girip, "Şifre en az 8 karakter olmalı" hatasını görmeyi beklemek. Kodun bunu nasıl kontrol ettiğini bilmeyiz.
* **Beyaz Kutu Testi (White-Box Testing):**
    * Sistemin iç yapısını (kodunu, algoritmalarını) *bilerek* yapılan testlerdir. Kodun içindeki tüm yolların (path) ve koşulların (condition) test edilmesi hedeflenir.
    * *Örnek:* Bir `if (user.isAdmin || user.isManager)` koşulunu test etmek için (1) Admin olan, (2) Manager olan, (3) İkisi de olmayan kullanıcılar için test yazmak. Birim testleri, beyaz kutu testlerinin en yaygın örneğidir.
* **Gri Kutu Testi (Grey-Box Testing):**
    * Kara kutu ve beyaz kutu arasında bir yaklaşımdır. Sistemin iç yapısı kısmen bilinir (örn. hangi veritabanı tablosuna yazıldığı bilinir), ancak kodun detayına inilmez.
    * *Örnek:* API'ye bir istek atıp (kara kutu), ardından veritabanına gidip kaydın doğru oluştuğunu sorgulamak (beyaz kutu bilgisi). Entegrasyon testleri genellikle gri kutudur.
* **Regresyon Testi (Regression Testing):**
    * "Geriye dönük uyumluluk testi." Koda yeni bir özellik eklendiğinde veya bir hata düzeltildiğinde, *mevcut ve çalışan eski özelliklerin bozulmadığından* emin olmak için yapılır.
    * Otomasyonun en kritik olduğu yer burasıdır.
* **Duman Testi (Smoke Testing):**
    * Yeni bir build (sürüm) alındığında, sistemin en temel ve kritik fonksiyonlarının (örn. "Uygulama açılıyor mu?", "Login oluyor mu?") ayakta olup olmadığını kontrol eden hızlı bir test setidir.
    * Başarısız olursa, build "kabul edilemez" sayılır ve daha detaylı testlere geçilmez.
* **Akıl Sağlığı Testi (Sanity Testing):**
    * Duman testine benzer, ancak genellikle bir hata düzeltmesi (bug fix) veya küçük bir değişiklik sonrası yapılır. Sadece *değişiklik yapılan alanın* ve ona bağlı temel fonksiyonların düzgün çalışıp çalışmadığına hızlıca bakılır.

### 3.2. Fonksiyonel Olmayan Testler (Non-Functional Testing - NFR)

Sistemin "nasıl çalışması gerektiğini" (performans, güvenlik, kullanılabilirlik gibi kalite niteliklerini) test eder.

* **Performans Testleri:**
    * **Yük Testi (Load Testing):** Sistemin *beklenen* kullanıcı yükü (örn. 1000 anlık kullanıcı) altında nasıl davrandığını (cevap süreleri, kaynak kullanımı) ölçer.
    * **Stres Testi (Stress Testing):** Sistemin *beklenen yükün çok üzerinde* (örn. 5000 kullanıcı) veya kaynakların kısıtlandığı (örn. az RAM) durumlarda "kırılma noktasını" bulmayı hedefler. Sistem çöküyor mu? Çöküyorsa, veriyi bozmadan mı çöküyor (graceful degradation)?
    * **Dayanıklılık Testi (Soak / Endurance Testing):** Sistemin, *normal* bir yük altında *uzun bir süre* (örn. 24 saat) boyunca çalıştırılmasıdır. Hafıza sızıntısı (memory leak) veya kaynak tüketimi sorunlarını bulmak için idealdir.
    * **Ani Yüklenme Testi (Spike Testing):** Yükün *aniden* sıfırdan zirveye (örn. 0'dan 2000 kullanıcıya 1 saniyede) çıktığı senaryoları test eder (örn. bilet satışı, kampanya başlangıcı).
* **Güvenlik Testleri (Security Testing):**
    * Sistemin yetkisiz erişimlere, veri sızıntılarına ve saldırılara karşı ne kadar korunaklı olduğunu test eder.
    * (Örn: SQL Injection, XSS, Yetkilendirme (Authorization) kontrolleri - "Normal kullanıcı, admin sayfasına erişebiliyor mu?").
* **Kullanılabilirlik Testleri (Usability Testing):**
    * Sistemin son kullanıcı için ne kadar "kolay", "anlaşılır" ve "verimli" olduğunu ölçer. Bu genellikle gerçek kullanıcılarla yapılan gözlem seansları ile yapılır. (Örn: "Kullanıcı, bir ürünü sepete eklemeyi 5 saniyede bulabiliyor mu?").
* **Uyumluluk Testleri (Compatibility Testing):**
    * Yazılımın farklı ortamlarda (farklı tarayıcılar - Chrome/Firefox/Safari, farklı işletim sistemleri - Windows/macOS/Linux, farklı cihazlar - Mobil/Tablet, farklı PLC modelleri) doğru çalışıp çalışmadığını test eder.
* **Lokalizasyon (L10N) ve Uluslararasılaştırma (I18N) Testleri:**
    * **I18N:** Yazılımın, farklı dil ve bölgelere *uyarlanabilecek* altyapıya sahip olup olmadığını test eder (örn. metinlerin hard-coded olmaması).
    * **L10N:** Yazılımın belirli bir dile (örn. Türkçe, Çince) çevrildiğinde arayüzün bozulmadığını, tarih/saat/para birimi formatlarının doğru gösterildiğini test eder.

---

## 4. Test Planlaması ve Dokümantasyonu

Test süreci rastgele değil, planlı ve dokümante edilmiş adımlarla yürütülmelidir.

### 4.1. Test Planı (Test Plan)

Test Planı, test sürecinin "Anayasası"dır. *Ne, neden, nasıl, ne zaman ve kim tarafından* test edilecek sorularını yanıtlayan resmi dokümandır.

* **Kapsam (Scope):**
    * *Neler Test Edilecek?* (Örn: "Kullanıcı yönetimi, reçete oluşturma ve Modbus TCP iletişimi.")
    * *Neler Test Edilmeyecek?* (Örn: "Üçüncü parti API'nin performans testi", "Eski Windows XP desteği.")
* **Strateji:**
    * Hangi test seviyesine ne kadar odaklanılacak (Test Piramidi).
    * Hangi test türleri uygulanacak (Fonksiyonel, NFR).
    * Otomasyon ve manuel test dengesi ne olacak?
* **Test Çevresi (Test Environment):**
    * Test nerede yapılacak? (Donanım, yazılım, ağ yapılandırması).
    * (Örn: "Staging sunucusu: AWS T3.Medium, PostgreSQL 14. Test PLC'si: Siemens S7-1200, Firmware v4.4").
* **Roller ve Sorumluluklar (Roles):**
    * Kim ne yapar? (Geliştiriciler: Unit Test. QA: Entegrasyon, E2E, Manuel Test. Ürün Sahibi: Kullanıcı Kabul Testi - UAT).
* **Risk Analizi (Risk):**
    * Projedeki en riskli alanlar nereler? (Örn: "Operatör hatasına bağlı tehlikeli makine hareketi", "Finansal hesaplama modülü", "Veri kaybı riski").
    * **Strateji:** Test eforu, *risk ile doğru orantılı* olmalıdır. En riskli modüller, en yoğun şekilde test edilmelidir.
* **Test Çıktıları (Test Deliverables):**
    * Bu süreç sonunda hangi dokümanlar üretilecek? (Test Planı, Test Senaryoları, Hata Raporları, Test Özet Raporu).
* **Giriş/Çıkış Kriterleri (Entry/Exit Criteria):**
    * *Giriş (Ne zaman teste başlarız?):* "Build başarılı olmalı, CI duman testleri geçmeli ve özellik `staging` ortamına deploy edilmeli."
    * *Çıkış (Ne zaman test biter?):* "Tüm 'Kritik' ve 'Yüksek' öncelikli hatalar (defect) kapatılmalı. Test senaryolarının %95'i 'Başarılı' olmalı. Kod kapsama (code coverage) oranı %80'in üzerinde olmalı."

### 4.2. Temel Test Dokümanları (Artifacts)

* **Test Senaryosu (Test Case):**
    * Belirli bir özelliği doğrulamak için gereken adımları tanımlayan en küçük birimdir. İyi bir test senaryosu şunları içermelidir:
        * **Benzersiz ID:** (Örn: `TC-LOGIN-001`)
        * **Başlık/Amaç:** (Örn: "Geçerli kullanıcı adı ve şifre ile başarılı giriş")
        * **Ön Koşullar:** (Örn: "Kullanıcı 'cakan' sistemde kayıtlı ve aktif olmalı")
        * **Test Adımları:** (1. Siteye git, 2. 'login' tıkla, 3. 'cakan' gir...)
        * **Beklenen Sonuç:** (Örn: "Kullanıcı ana sayfaya yönlendirilmeli ve 'Hoş Geldin, cakan' yazısı görülmeli")
        * **Gerçekleşen Sonuç:** (Test çalıştırıldığında alınan sonuç)
        * **Durum:** (Başarılı / Başarısız / Atlandı)
* **Test Paketi (Test Suite):**
    * İlgili test senaryolarının bir araya getirildiği bir koleksiyondur. (Örn: "Giriş İşlemleri Test Paketi", "Reçete Yönetimi Regresyon Paketi").
* **Test Raporu (Test Summary Report):**
    * Bir test döngüsü (cycle) bittiğinde paydaşlara sunulan özet rapordur. (Kaç test koşuldu? Kaçı başarılı/başarısız? Bulunan kritik hatalar neler? Test süreci bloke oldu mu?).

---

## 5. Test Otomasyon Stratejisi

Manuel test paha biçilmezdir, ancak *tekrarlanabilir* testler (regresyon, yük testleri) için otomasyon şarttır.

### 5.1. CI/CD Pipeline Entegrasyonu

* **CI Pipeline'da Otomatik Çalıştırma (Hızlı Geri Bildirim):**
    * **`Unit Testleri`:** *Her commit'te ve her Pull Request'te* çalışmalıdır. 1-2 dakikadan uzun sürmemeli. Başarısız olursa PR'ı bloke etmelidir.
    * **`Integration Testleri`:** *Her Pull Request'te* (veya günde birkaç kez `develop` dalında) çalışmalıdır. 5-10 dakikayı geçmemelidir.
* **Periyodik Çalıştırma (Nightly Build):**
    * **`E2E / UI Testleri`:** Yavaş ve kırılgandırlar. Her commit'te çalıştırılmazlar.
    * **Strateji:** Her gece yarısı, sistemin `staging` (test) ortamına karşı tüm E2E test paketi otomatik olarak çalıştırılır. Ekip, sabah işe geldiğinde "Gece Test Raporu"nu inceler. Bu, gün içinde bir regresyon olup olmadığını (bir özellik diğerini bozdu mu?) anlamanın en verimli yoludur.

### 5.2. Ne Zaman Otomasyon *Yapılmamalı*?

Otomasyon her derde deva değildir. Aşağıdaki durumlarda manuel test daha verimlidir:

* **Keşifsel Test (Exploratory Testing):** Senaryonun önceden belli olmadığı, testçinin "sistemi kırmaya" çalıştığı yaratıcı testler.
* **Kullanılabilirlik Testi (Usability Testing):** Bir insanın "bu butonun yeri kullanışsız" demesini otomasyonla ölçemezsiniz.
* **Sürekli Değişen Arayüzler (UI):** Henüz oturmamış, sık değişen ekranları otomatize etmek, sürekli bakım maliyeti çıkarır.
* **Sadece Bir Kez Yapılacak Testler:** Otomasyonu yazmak, manuel yapmaktan daha uzun sürecekse.

---

## 6. Manuel Testin Yeri ve Önemi

Otomasyon, bilinen senaryoları tekrar eder. Manuel test ise *insan sezgisi, deneyimi ve yaratıcılığı* ile *bilinmeyen* hataları bulur.

* **Keşifsel Test (Exploratory Testing):**
    * Test uzmanının, önceden tanımlanmış test senaryolarına *bağlı kalmadan*, tecrübesine dayanarak sistemi serbestçe test etmesidir. "Bu alana 5000 karakter girersem ne olur?", "Kaydet butonuna art arda 10 kez basarsam ne olur?" gibi sorularla ilerler.
* **Ad-hoc Test (Rastgele Test):**
    * Keşifsel teste benzer ancak daha az yapıdadır, genellikle "akla ilk gelen" senaryoların denenmesidir.

---

## 7. Hata (Defect) Yaşam Döngüsü

Bir hata (bug) bulunduğunda, onun "kapatılması" süreci net bir yaşam döngüsü izlemelidir. Bu, kaos yerine düzen sağlar.

1.  **New (Yeni):** Test uzmanı bir hata bulur ve raporlar (Ekran görüntüsü, loglar, adımlar).
2.  **In Analysis (Analizde):** Proje yöneticisi veya takım lideri hatayı inceler. (Gerçek bir hata mı? Yinelenen (Duplicate) mi? Hangi özellikten kaynaklı?)
3.  **Assigned (Atandı):** Hata ilgili geliştiriciye atanır.
4.  **In Progress (Çalışılıyor):** Geliştirici hatayı çözmek için kodlama yapmaya başlar.
5.  **Fixed (Düzeltildi / Ready for QA):** Geliştirici düzeltmeyi tamamlar ve kodu `develop` (veya `hotfix`) dalına gönderir. Build alınıp `staging` ortamına deploy edilir.
6.  **In Verification (Doğrulamada):** Test uzmanı, hatayı *ilk bulduğu* ortamda (staging) tekrar test eder.
7.  **(Sonuç 1) Closed (Kapatıldı):** Test uzmanı, hatanın düzeltildiğini onaylar. Döngü biter.
8.  **(Sonuç 2) Re-Opened (Yeniden Açıldı):** Hata devam ediyor veya düzeltme başka bir şeyi bozmuş. Hata, açıklama ile birlikte tekrar "Assigned" statüsüne alınır.
9.  **(Diğer Durumlar) Deferred (Ertelendi):** Hata kabul edildi ancak düşük öncelikli olduğu için bu sürümde düzeltilmeyecek.
10. **(Diğer Durumlar) Rejected (Reddedildi):** Raporlanan durum bir hata değil, "tasarım gereği böyle çalışıyor".

### 7.1. Öncelik (Priority) vs. Ciddiyet (Severity)

Bu, hata yönetimindeki en önemli ayrımdır:

* **Ciddiyet (Severity):** Hatanın teknik veya işlevsel *etkisidir*. (QA tarafından belirlenir)
    * `Blocker/Critical:` Sistemin çökmesi, veri kaybı, güvenlik açığı, ana işlevin çalışmaması.
    * `High:` Ana işlevin yanlış çalışması, alternatif yol olmaması.
    * `Medium:` Ana işlevin bir kısmının yanlış çalışması, ama alternatif yol (workaround) olması.
    * `Low/Trivial:` Kozmetik sorun, yazım hatası (typo), UI'da küçük kayma.

* **Öncelik (Priority):** Hatanın *ne kadar hızlı* düzeltilmesi gerektiğidir. (Ürün Sahibi/Proje Yöneticisi tarafından belirlenir)
    * `High:` Hemen düzeltilmeli (Genellikle tüm `Blocker` ve `Critical`'lar).
    * `Medium:` Bir sonraki planlı sürüme (sprint) dahil edilmeli.
    * `Low:` Vakit olursa düzeltilir (Backlog'a atılır).

**Örnek:** Ana sayfadaki şirket logosunun yanlış olması `Low Severity` (düşük ciddiyet) ama `High Priority` (yüksek öncelik) olabilir. Sadece 31 Aralık gecesi saat 23:59'da çöken bir raporlama hatası `Critical Severity` (kritik ciddiyet) ama `Medium Priority` (orta öncelik) olabilir.

---

## 8. Kapsamlı "Bitti" Tanımı (Definition of Done - DoD)

Bir özelliğin veya sprint'in "bitti" kabul edilmesi için gereken kapsamlı kontrol listesidir.

### Kod ve Kalite Kriterleri
* [ ] **Kodlama:** Kod yazımı tamamlanmıştır ve belirlenen kodlama standartlarına (örn. PEP8, ESLint) uygundur.
* [ ] **Birim Testleri:** İlgili kod için Birim Testleri yazılmış, tümü başarıyla çalışmıştır.
* [ ] **Kod Kapsama (Coverage):** Birim testleri, yeni eklenen kodun en az %80'ini kapsamaktadır (veya proje standardı ne ise).
* [ ] **Statik Analiz:** Kod, statik analiz araçlarından (örn. SonarQube, Lint) "Kritik" veya "Yüksek" ciddiyette uyarı almadan geçmiştir.
* [ ] **Kod İncelemesi (Review):** Kod, en az bir (veya iki) takım arkadaşı tarafından incelenmiş (Pull Request) ve onaylanmıştır.

### Test ve Doğrulama Kriterleri
* [ ] **Entegrasyon Testleri:** İlgili modülün entegrasyon testleri yazılmış ve başarıyla çalışmıştır.
* [ ] **CI/CD Pipeline:** Tüm kod `develop` (veya `main`) dalına merge edilmiş ve CI pipeline (tüm birim ve entegrasyon testleri dahil) başarıyla tamamlanmıştır.
* [ ] **Test Senaryoları:** Özellik için tanımlanan tüm manuel ve otomatize fonksiyonel test senaryoları `staging` ortamında çalıştırılmış ve "Başarılı" olmuştur.
* [ ] **Regresyon Testi:** Özelliğin etkilediği alanlardaki regresyon testleri (otomatize veya manuel) çalıştırılmış ve mevcut fonksiyonların bozulmadığı teyit edilmiştir.
* [ ] **Hata Durumu:** Özellikle ilgili "Çıkış Kriterleri" karşılanmıştır (Örn: Açıkta `Critical` veya `High` öncelikli hata bulunmamaktadır).
* [ ] **Fonksiyonel Olmayan Testler:** Gerekiyorsa, ilgili NFR testleri (örn. yeni bir API için yük testi, yeni bir sayfa için uyumluluk testi) yapılmıştır.

### Ürün ve Paydaş Kriterleri
* [ ] **Kullanıcı Kabul Testi (UAT):** Özellik, Ürün Sahibi (Product Owner) veya ilgili paydaş tarafından `staging` ortamında test edilmiş ve "Kabul" onayı almıştır.
* [ ] **Dokümantasyon:**
    * [ ] Gerekli API dokümantasyonu (örn. Swagger/OpenAPI) güncellenmiştir.
    * [ ] Gerekli son kullanıcı dokümantasyonu (Kullanım Kılavuzu) güncellenmiştir.
    * [ ] Teknik dokümantasyon (mimari, tasarım kararları) güncellenmiştir.
* [ ] **Sürüme Hazırlık:** Özellik, `production` ortamına deploy edilmeye hazırdır (gerekli environment değişkenleri, deployment script'leri vb. tanımlanmıştır).