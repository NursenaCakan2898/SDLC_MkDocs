> **NOT: ÖRNEK ŞABLON (TEMPLATE)**
>
> Aşağıdaki doküman, kapsamlı bir "Master Test Planı" için **örnek bir şablon** niteliğindedir. Gerçek bir projede kullanılmadan önce, projenin özel gereksinimlerine, kapsamına, kaynaklarına, takvimine ve organizasyonel standartlarına göre **mutlaka uyarlanmalı ve özelleştirilmelidir**.

# [Proje Adı] - Kapsamlı Test Planı (Master Test Plan)

---

## 1. Giriş ve Planın Önemi

### 1.1. Test Planının Önemi
Test Planı, bir yazılım projesinin kalitesini sağlamanın **"Anayasası"** veya **"Yol Haritası"**dır. Sadece test ekibinin ne yapacağını listeleyen basit bir kontrol listesi değil, tüm proje paydaşlarının (Geliştirme, QA, Ürün Yönetimi, DevOps ve Müşteri) üzerinde anlaştığı **resmi bir sözleşmedir**.

Bu planın temel önemi şunlardır:
* **Ortak Anlayış Yaratır:** "Ne"yin test edileceğini (kapsam) ve daha da önemlisi "ne"yin test edilmeyeceğini (kapsam dışı) netleştirerek belirsizliği ortadan kaldırır.
* **Kaynakları Planlar:** "Kim", "hangi" araçla ve "hangi" ortamda (çevre) test yapacak sorularını yanıtlar.
* **Stratejiyi Belirler:** Test eforunun, en riskli ve en önemli alanlara (riske dayalı test) odaklanmasını sağlar.
* **Riskleri Öngörür:** Test sürecini sekteye uğratabilecek riskleri (örn. "Test ortamı gecikirse ne yaparız?") henüz proje başındayken tanımlar ve azaltma planları sunar.
* **Başarıyı Ölçer:** Bir ürünün ne zaman "canlıya çıkmaya hazır" olduğuna karar vermek için objektif ölçütler (Giriş/Çıkış Kriterleri - Definition of Done) sunar.

Kısacası, bu doküman; kaos yerine yapılandırılmış bir doğrulama süreci sağlar, bütçe ve zamanın verimli kullanılmasını temin eder ve projenin kalite hedeflerine ulaşmasının güvencesidir.

### 1.2. Doküman Kimliği

| Nitelik | Değer |
| :--- | :--- |
| **Doküman Adı** | [Proje Adı] - Kapsamlı Test Planı (Master Test Plan) |
| **Doküman ID** | [Örn: `P-2025-001-TEST-PLAN`] |
| **Proje Adı** | [Proje Adı] |
| **Hazırlayan** | [İsim Soyisim, Rol] |
| **Onaylayan** | [İsim Soyisim, Yönetici Rolü], [İsim Soyisim, Müşteri/PO] |
| **Yayın Tarihi** | [GG.AA.YYYY] |
| **Sürüm** | `v1.0.0` |

### 1.3. Revizyon Geçmişi

| Sürüm | Tarih | Yazar | Değişiklik Özeti |
| :--- | :--- | :--- | :--- |
| v0.1.0 | [Tarih] | [İsim] | İlk taslak oluşturuldu. |
| v0.2.0 | [Tarih] | [İsim] | Kapsam ve Riskler eklendi. |
| v1.0.0 | [Tarih] | [İsim] | Onaylandı ve yayınlandı. |

### 1.4. Referanslar

Bu doküman, aşağıdaki dokümanlar ve standartlar temel alınarak hazırlanmıştır:
* `[Doküman ID]` - [Proje Adı] Yazılım Gereksinim Spesifikasyonu (SRS)
* `[Doküman ID]` - [Proje Adı] Mimari Tasarım Dokümanı (SDD)
* `[Doküman ID]` - [Proje Adı] Proje Yönetim Planı
* `SDLC.md` - Organizasyonel SDLC Kılavuzu
* `Testing.md` - Organizasyonel Test ve Doğrulama Kılavuzu
* `Maintenance.md` - Organizasyonel Bakım ve Operasyon Kılavuzu
* *(Varsa)* `IEEE 829-2008 Standard for Software and System Test Documentation`

### 1.5. Terimler ve Kısaltmalar
* **SRS:** Yazılım Gereksinim Spesifikasyonu
* **PO:** Ürün Sahibi (Product Owner)
* **UAT:** Kullanıcı Kabul Testi
* **NFR:** Fonksiyonel Olmayan Gereksinim (Non-Functional Requirement)
* **APM:** Uygulama Performans İzleme (Application Performance Monitoring)
* **SEV:** Ciddiyet Seviyesi (Severity)
* **RCA:** Kök Neden Analizi (Root Cause Analysis)

---

## 2. Test Kapsamı ve Stratejisi

### 2.1. Test Edilecek Kalem (Test Item)
Bu plan, `[Proje Adı]`, Sürüm `[Sürüm Numarası, örn. 2.0]` yazılımının test süreçlerini kapsar. Bu yazılım, `[Yazılımın 1-2 cümlelik kısa tanımı, örn. Django tabanlı bir üretim yönetim web uygulaması ve buna bağlı bir PySide6 masaüstü HMI istemcisi]`'dir.

### 2.2. Test Kapsamı (Scope)

#### 2.2.1. Kapsam Dahilindeki Özellikler (In-Scope)
Aşağıdaki modüller ve fonksiyonel/fonksiyonel olmayan gereksinimler *test edilecektir*:
* **[Modül 1: örn. Kullanıcı Yönetimi & Yetkilendirme]**
    * [Gereksinim ID: F-101] - Rol bazlı erişim kontrolü (Admin, Operatör, İzleyici).
    * [Gereksinim ID: F-102] - Active Directory (LDAP) ile entegre SSO girişi.
* **[Modül 2: örn. Reçete Yönetimi (Web & Desktop)]**
    * [Gereksinim ID: F-201] - Web arayüzünden reçete oluşturma ve parametre (süre, sıcaklık) tanımlama.
    * [Gereksinim ID: F-202] - Masaüstü HMI uygulamasından reçeteyi seçme ve PLC'ye (Data Block) yazma.
* **[Modül 3: örn. Endüstriyel Haberleşme (Sadece Masaüstü)]**
    * [Gereksinim ID: F-301] - Modbus TCP ve/veya Siemens S7 (ISO-on-TCP) protokolleri üzerinden PLC veri okuma.
    * [Gereksinim ID: F-302] - 500ms'den kısa süren bağlantı kesintilerinin otomatik olarak tolere edilmesi (Resilience).
* **[Modül 4: örn. Raporlama (Web)]**
    * [Gereksinim ID: F-401] - Üretim sonu verilerinin PDF olarak dışa aktarılması.
* **[Modül 5: Fonksiyonel Olmayan Gereksinimler (NFR)]**
    * [NFR-01] - Web API yanıt süresi, 100 eşzamanlı kullanıcı yükü altında p95 < 800ms olmalıdır.
    * [NFR-02] - Masaüstü uygulama başlangıç süresi < 5 saniye olmalıdır.
    * [NFR-03] - Sistem, OWASP Top 10 güvenlik açıklarına (SQLi, XSS) karşı korumalı olmalıdır.

#### 2.2.2. Kapsam Dışındaki Özellikler (Out-of-Scope)
Aşağıdaki kalemler, bu test döngüsünde *test edilmeyecektir*:
* **Üçüncü Parti Bileşenler:** [Örn. PDF render motorunun iç çalışması, Siemens PLC'nin firmware'i, Active Directory sunucusunun kendisi] (Not: Bu sistemlerle olan *entegrasyon* test edilecektir.)
* **Donanım Arızası:** Fiziksel donanım arıza senaryoları (örn. kablo kopması, PLC CPU'sunun 'Stop'a geçmesi) simüle edilecektir, ancak fiziksel olarak test edilmeyecektir.
* **Gelecek Sürüm Özellikleri:** [Örn. "Makine Öğrenmesi ile Kestirimci Bakım Modülü (v3.0)"]
* **Belirli NFR'ler:** [Örn. "Felaket Kurtarma (Disaster Recovery) testi", "Günde 10 Milyon isteği aşan stres testi"] (Bu testler ayrı bir NFR Test Planı'nda ele alınacaktır).

### 2.3. Test Stratejisi

#### 2.3.1. Test Seviyeleri ve Yaklaşımı
* **Birim Testleri (Unit):** Geliştirme ekibi sorumluluğundadır. TDD (Test-Driven Development) teşvik edilir. Kod kapsama (Code Coverage) hedefi `[örn. %85]`'tir.
* **Entegrasyon Testleri (Integration):** QA ve Geliştirme ekipleri sorumluluğundadır. İki seviyede yapılır:
    1.  *Bileşen Entegrasyonu (API/Servis):* Mikroservislerin veya API'nin veritabanı ile konuşması (API -> DB).
    2.  *Sistem Entegrasyonu (Uçtan Uca):* Masaüstü HMI'nin Web API'ye, Web API'nin PLC Simülatörüne bağlandığı senaryolar.
* **Sistem Testleri (System):** QA ekibi sorumluluğundadır. Sistem *bir bütün olarak* ele alınır. Fonksiyonel gereksinimlerin (SRS) tamamı bu seviyede "black-box" (kara kutu) tekniği ile doğrulanır.
* **Kabul Testleri (Acceptance):**
    1.  *Alfa Testi:* Ürün Sahibi (PO) ve şirket içi "uzman kullanıcılar" tarafından Staging ortamında yapılır.
    2.  *Beta Testi (Varsa):* Seçilmiş [Örn. 1-2 pilot fabrika] tarafından canlı-öncesi (pre-prod) ortamda yapılır.

#### 2.3.2. Test Türleri ve Odak Alanları
| Test Türü | Amaç | Yöntem | Sorumlu |
| :--- | :--- | :--- | :--- |
| **Duman (Smoke) Testi** | Build'in temel fonksiyonlarının (Login, Ana Sayfa) çalışıp çalışmadığını kontrol etmek. | Otomatize (CI/CD Pipeline) | DevOps / QA |
| **Sanity Testi** | Hata düzeltmesi (bug fix) sonrası *sadece o alanın* düzgün çalıştığını hızla kontrol etmek. | Manuel | QA |
| **Fonksiyonel Test** | Tüm SRS gereksinimlerinin detaylı doğrulanması. | Manuel & Otomatize | QA |
| **Regresyon Testi** | Yeni kodun, eski ve çalışan özellikleri bozmadığından emin olmak. | Otomatize (ağırlıklı) | QA |
| **API Testi** | API endpoint'lerinin (GET, POST, PUT, DELETE) sözleşmeye (Swagger) uygunluğunu, yetkilendirmeyi ve veri bütünlüğünü test etmek. | Otomatize (Postman, vb.) | QA / Dev |
| **UI/UX & Kullanılabilirlik**| Arayüzün (Web & Masaüstü) sezgisel, tutarlı ve erişilebilir (accessibility) olup olmadığını test etmek. | Manuel (Keşifsel) | QA / PO |
| **Lokalizasyon (L10N)** | Uygulamanın [Örn. Türkçe, Çince] dillerinde doğru çalışması (Tarih formatları, para birimi, UI bozulmaları). | Manuel | QA |
| **Performans (NFR)** | NFR-01 ve NFR-02'yi doğrulamak (Yük, Gecikme, Başlangıç Süresi). | Otomatize (JMeter, k6) | QA (Performans) |
| **Güvenlik (NFR)** | NFR-03'ü (OWASP) doğrulamak. | Otomatize (SAST, DAST) & Manuel (PenTest) | Güvenlik Ekibi |
| **Uyumluluk (NFR)** | Bkz. 4.2 - Farklı platformlarda (tarayıcı, OS) çalışırlık. | Manuel & Otomatize | QA |
| **Kesinti & Kurtarma** | NFR-302'yi (Bağlantı kesintisi) ve uygulama çökme sonrası (Masaüstü) veri kurtarmayı test etmek. | Manuel (Simülasyon) | QA |

#### 2.3.3. Test Tasarım Teknikleri
Test senaryolarının (Test Case) etkinliğini artırmak için aşağıdaki teknikler kullanılacaktır:
* **Denklik Sınıfları (Equivalence Partitioning):** (Örn. Sıcaklık girişi için: Negatif, Geçerli [0-100], Geçersiz [100+])
* **Sınır Değer Analizi (Boundary Value Analysis):** (Örn. Geçerli aralık [0-100] ise; -1, 0, 100, 101 değerleri test edilir)
* **Durum Geçiş (State Transition) Testi:** (Örn. Üretim durumu: `DURDU` -> `BAŞLIYOR` -> `ÜRETİMDE` -> `BİTTİ`)
* **Karar Tablosu (Decision Table) Testi:** (Örn. Kullanıcı Rolü + Aksiyon = İzin Verildi / Reddedildi)
* **Kullanım Senaryosu (Use Case) Testi:** Uçtan uca kullanıcı hikayelerinin testi.
* **Keşifsel (Exploratory) Test:** Deneyimli QA mühendislerinin, önceden tanımlı senaryolar *dışına çıkarak* hata aradığı, seans bazlı (session-based) testler.

---

## 3. Test Otomasyon Stratejisi

* **Hedef:** Regresyon test süresini [Örn. 3 günden 4 saate] indirmek ve CI/CD pipeline'da hızlı geri bildirim sağlamak.
* **Araçlar (Tool Stack):**
    * *Unit Test:* [Örn. PyTest (Python), Jest (React)]
    * *API Test:* [Örn. Postman (Newman), REST Assured]
    * *Web UI Test:* [Örn. Cypress veya Playwright]
    * *Desktop UI Test:* [Örn. PyAutoGUI (koordinat bazlı) veya (varsa) PySide6 Test Kütüphanesi]
    * *Performans Test:* [Örn. k6 (JavaScript) veya JMeter]
* **Çalıştırma (Execution):**
    * *Her Commit (PR):* Unit Testler + API Duman Testleri.
    * *Gece (Nightly Build):* Tam Regresyon Paketi (API + Web UI + Desktop UI) `Staging` üzerinde.
* **Çerçeve (Framework):** [Örn. Page Object Model (POM) Web UI için, BDD (Cucumber/Behave) senaryoları için]

---

## 4. Test Çevresi ve Veri Yönetimi

### 4.1. Çevre Mimarisi
| Çevre | URL / IP | Veritabanı | Amaç | Sorumlu |
| :--- | :--- | :--- | :--- | :--- |
| **Dev** | `localhost` | Yerel (SQLite / Docker) | Geliştirme, Unit Test | Geliştirici |
| **CI** | `Jenkins/GitLab` | In-Memory (H2) | Sürekli Entegrasyon | DevOps |
| **Staging (QA)**| `staging.proje.com` | `staging-db-server` (PostgreSQL) | Ana Test Ortamı (Manuel, Otomasyon, NFR) | DevOps / QA |
| **UAT** | `uat.proje.com` | `uat-db-server` (Staging Klonu) | Kullanıcı Kabul Testleri | DevOps / PO |
| **Prod** | `proje.com` | `prod-db-cluster` | Canlı Sistem | Operasyon |

### 4.2. Test Yatağı (Test Bed) - Donanım & Yazılım
* **Web Test (İstemci):**
    * *Tarayıcılar:* Chrome (son 2 sürüm), Firefox (son 2 sürüm), Safari (son sürüm), Edge (son 2 sürüm).
    * *Mobil Emülasyon:* iOS 16 (Safari), Android 13 (Chrome).
* **Masaüstü Test (İstemci):**
    * *OS:* Windows 11 (x64), Windows 10 (x64), [Varsa: Ubuntu 22.04].
    * *Donanım:* Minimum [Örn. 4GB RAM, Core i3] ve Tavsiye Edilen [Örn. 16GB RAM, Core i7] konfigürasyonlar.
* **Endüstriyel Test (Sunucu/Donanım):**
    * *Gerçek Donanım (Staging):* [Örn. Siemens S7-1500 (CPU 1511), TP1200 HMI Panel].
    * *Simülatör (Dev/CI):* [Örn. PLCSIM Advanced v4.0, ModbusPal Simülatörü].

### 4.3. Test Veri Yönetimi (TDM)
* **Veri Kaynağı:** `Staging` ve `UAT` ortamları, canlı (Production) verinin *anonimleştirilmiş* (KVK/GDPR uyumlu) bir kopyasını kullanacaktır. Bu kopya her ayın 1'inde yenilenir.
* **Veri Setleri:**
    * *Golden Dataset:* Regresyon testleri için "temiz" ve bilinen bir veri seti.
    * *Senaryo Setleri:* UAT için PO tarafından sağlanan "tipik bir gün" ve "ay sonu kapanışı" veri setleri.
    * *Endüstriyel Veri:* PLC Simülatörüne "normal çalışma", "hatalı sensör verisi (örn. -999)" ve "limit dışı" değerleri basacak script'ler.
* **Veri Temizliği:** Her otomasyon testinden sonra, testin yarattığı veriler (örn. sahte siparişler) API aracılığıyla temizlenecektir (Tear Down).

---

## 5. Roller, Sorumluluklar ve Kaynaklar

| Rol | İsim / Ekip | Temel Sorumluluklar |
| :--- | :--- | :--- |
| **Proje Yöneticisi (PM)** | [İsim] | Test planını onaylamak, kaynakları (bütçe, zaman) yönetmek, riskleri izlemek. |
| **Ürün Sahibi (PO)** | [İsim] | Gereksinimleri netleştirmek, UAT testlerini yürütmek, bug'ları önceliklendirmek, *Exit Criteria* için son onayı vermek. |
| **QA Lideri / Yöneticisi**| [İsim] | Bu planı yazmak ve sürdürmek, test eforunu tahminlemek, ekibi koordine etmek, nihai test raporlarını sunmak. |
| **QA Mühendisi (Manuel)**| [Ekip] | Test senaryoları tasarlamak, manuel testleri (fonksiyonel, keşifsel, L10N) yürütmek, bug raporlamak. |
| **QA Mühendisi (Otomasyon)**| [Ekip] | Otomasyon script'lerini yazmak, framework'ü sürdürmek, CI/CD pipeline'ı yönetmek, regresyon sonuçlarını analiz etmek. |
| **Geliştirme Ekibi (Dev)** | [Ekip Lideri] | Birim testlerini yazmak, kod incelemesi yapmak, bug'ları ciddiyete (Severity) göre düzeltmek. |
| **DevOps / Altyapı** | [Ekip] | Test çevrelerini (4.1) kurmak ve sürdürmek, CI/CD pipeline'ı yönetmek, veritabanı klonlama/anonimleştirme işlemlerini yapmak. |

---

## 6. Takvim, Çıktılar ve Kilometre Taşları

### 6.1. Kilometre Taşları
| Kilometre Taşı | Hedef Tarih | Açıklama |
| :--- | :--- | :--- |
| Test Planı Onayı | [Tarih] | Bu dokümanın imzalanması (Bkz. Bölüm 10). |
| Test Çevresi Hazırlığı | [Tarih] | Staging, UAT ve Test Donanımlarının hazır olması. |
| Test Senaryosu Tasarımı | [Tarih] | Tüm kritik fonksiyonel test senaryolarının yazılması. |
| Otomasyon Altyapısı | [Tarih] | Duman Testi paketinin CI'a entegre edilmesi. |
| Sistem Testi Başlangıcı| [Tarih] | Ana test fazının başlaması. |
| Performans Testleri | [Tarih] | NFR testlerinin tamamlanması. |
| UAT Başlangıcı | [Tarih] | PO ve son kullanıcı testlerinin başlaması. |
| GO/NO-GO Toplantısı | [Tarih] | Canlıya çıkış kararının verilmesi. |

### 6.2. Test Çıktıları (Deliverables)
Test sürecinde üretilecek ana dokümanlar:
* Test Planı (Bu doküman)
* Test Senaryoları (Test Case Repository) `[Araç: örn. TestRail, Xray]`
* Test Otomasyon Script'leri `[Araç: örn. GitLab Repository]`
* Hata Raporları (Defect Reports) `[Araç: örn. Jira]`
* Haftalık Test İlerleme Raporu
* Test Döngüsü Kapatma Raporu (Test Closure Report)

---

## 7. Risk Yönetimi (Test Süreci)

Test sürecini tehlikeye atabilecek riskler ve stratejiler:

| Kategori | Risk | Olasılık (D/O/Y) | Etki (D/O/Y) | Azaltma Stratejisi (Mitigation) | Acil Durum Planı (Contingency) | Sorumlu |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **Kaynak** | Deneyimli otomasyon mühendisinin projeden ayrılması. | Düşük | Yüksek | Bilgi paylaşım seansları, detaylı dokümantasyon, çift-programlama (pair-programming). | Dışarıdan danışmanlık almak veya otomasyon kapsamını daraltmak. | PM / QA Lideri |
| **Zamanlama**| Geliştirme fazının gecikmesi ve test takviminin sıkışması. | Yüksek | Yüksek | Agile (Çevik) metodoloji ile testleri sprint içine yaymak, erken geri bildirim vermek. | **Riske Dayalı Test (Risk-Based Testing)**: Sadece en kritik ve en çok kullanılan modülleri (Happy Path) test etmek. | PM |
| **Teknik** | Staging (QA) çevresinin kararsız olması, sık çökmesi. | Orta | Yüksek | DevOps ile SLA belirlemek, çevre için de "Duman Testi" otomasyonu kurmak. | Testleri yerel (local) veya UAT ortamına kaydırmak (ideal değil). | DevOps |
| **Kapsam** | Gereksinimlerin test aşamasında sürekli değişmesi (Scope Creep). | Yüksek | Yüksek | Değişiklik Kontrol Kurulu (CCB) oluşturmak. Yeni talepleri v2.1'e ertelemek için PO ile müzakere etmek. | Değişiklik başına eklenecek test eforunu ve süreyi PM'e raporlamak. | PO / PM |
| **Donanım** | Endüstriyel test tezgahındaki (PLC/HMI) donanımın arızalanması. | Orta | Yüksek | Donanımın yedeğini bulundurmak, bakımını yapmak. | Testleri %100 simülatör (PLCSIM) üzerinde devam ettirmek (doğruluk payı düşer). | QA Lideri |

---

## 8. Giriş, Çıkış, Askıya Alma ve Sürdürme Ölçütleri

### 8.1. Teste Giriş Ölçütleri (Entry Criteria)
Bir test döngüsünün (Sistem Testi, UAT) *başlaması* için GEREKLİ şartlar:
* [ ] Bu Test Planı'nın tüm paydaşlarca onaylanmış (imzalanmış) olması.
* [ ] Test edilecek tüm özelliklerin geliştirilmesinin tamamlanmış ("Feature Complete") ve `Staging` ortamına dağıtılmış olması.
* [ ] Geliştirici Birim Testleri'nin %100 başarılı olması.
* [ ] "Duman Testi" otomasyonunun %100 başarılı olması (Build'in "sağlıklı" olduğunun teyidi).
* [ ] Test senaryolarının en azından `[örn. %80]`'inin yazılmış ve gözden geçirilmiş (reviewed) olması.
* [ ] Test verisinin (Bkz. 4.3) ilgili ortama (Staging/UAT) yüklenmiş olması.

### 8.2. Testi Askıya Alma Ölçütleri (Suspension Criteria)
Test sürecinin *durdurulması* için GEREKLİ şartlar:
* [ ] "Duman Testi" paketinin %25'ten fazlasının başarısız olması (Temel fonksiyonlar çalışmıyor).
* [ ] Sistemin ana akışını (örn. Login, Ana Sayfa, Reçete Açma) engelleyen bir `Blocker` seviyeli hata bulunması.
* [ ] Test çevresinin (Staging) çökmesi ve `[örn. 4]` saatten uzun süre erişilemez olması.
* [ ] Geliştirme ekibinin, test edilen sürüm üzerine acil ve büyük bir değişiklik (hotfix) yapma zorunluluğu.

### 8.3. Testi Sürdürme Ölçütleri (Resumption Criteria)
Askıya alınan testin *devam etmesi* için GEREKLİ şartlar:
* [ ] Askıya alınmaya neden olan `Blocker` hatanın Geliştirme Ekibi tarafından düzeltilmiş ("Fixed") ve QA tarafından doğrulanmış ("Verified") olması.
* [ ] Test çevresinin tekrar stabil hale gelmesi.
* [ ] Düzeltmeyi içeren yeni build'in Duman Testi'ni %100 başarıyla geçmesi.

### 8.4. Testten Çıkış Ölçütleri (Exit Criteria) - "BİTTİ TANIMI"
Test sürecinin "tamamlandı" ve sürümün "canlıya çıkmaya hazır" (Go) kabul edilmesi için GEREKLİ şartlar:
* [ ] Planlanan tüm test senaryolarının (manuel ve otomasyon) %100'ünün yürütülmüş (executed) olması.
* [ ] Test senaryolarının genel başarı oranının (Pass Rate) en az `[örn. %98]` olması.
* [ ] Açıkta **0 (Sıfır)** adet `Blocker` (Engelleyici) veya `Critical` (Kritik) ciddiyette (Severity) hata bulunması.
* [ ] Açıkta **`[örn. 3]`'ten az** `High` (Yüksek) ciddiyette hata bulunması.
* [ ] Ertelenen (Deferred) tüm `High` ve `Medium` ciddiyetteki hatalar için Ürün Sahibi'nin (PO) yazılı onayının alınmış ve bir sonraki sürüm için `Backlog`'a eklenmiş olması.
* [ ] Tüm Kullanıcı Kabul Testleri (UAT) senaryolarının Ürün Sahibi tarafından "Başarılı" olarak imzalanmış olması.
* [ ] Test Kapatma Raporu'nun (Test Closure Report) hazırlanmış ve paydaşlara sunulmuş olması.
* [ ] NFR hedeflerinin (performans, güvenlik) karşılandığına dair raporların sunulmuş olması.

---

## 9. Hata (Defect) Yönetimi

* **Araç:** Tüm hatalar `[Araç Adı, örn. Jira]` üzerinde, proje şablonuna uygun olarak kaydedilecektir.
* **Yaşam Döngüsü:** `New` -> `In Analysis` (PO/QA Lideri) -> `Assigned` (Dev Ekibi) -> `In Progress` -> `Fixed` (Dev Ekibi) -> `Ready for QA` -> `In Verification` (QA Ekibi) -> `Closed` VEYA `Re-Opened`.
* **Sınıflandırma:**
    * **Ciddiyet (Severity) - (QA belirler):**
        * `Blocker`: Sistemi tamamen durduran, veri kaybına yol açan, testleri imkansız kılan hata.
        * `Critical`: Ana iş akışını engelleyen, alternatif yolu (workaround) olmayan hata.
        * `High`: Ana iş akışını engelleyen, ancak alternatif yolu olan hata. VEYA İkincil bir özelliğin tamamen çalışmaması.
        * `Medium`: İkincil bir özellikte yanlış çalışma, UI/UX bozuklukları.
        * `Low`: Kozmetik sorunlar, yazım hataları (typo).
    * **Öncelik (Priority) - (PO belirler):**
        * `High`: Acilen (bu build'de) düzeltilmeli.
        * `Medium`: Bir sonraki planlı sprint'e alınmalı.
        * `Low`: Vakit kalırsa (Backlog'a atılır) düzeltilmeli.

---

## 10. Test Raporlaması ve İletişim

* **Test Yönetim Aracı:** Tüm test senaryoları, yürütmeler ve hatalar `[örn. Jira + Xray, TestRail, Azure DevOps]` üzerinden yönetilecektir.
* **Hata (Defect) Raporlaması:** Bulunan tüm hatalar `maintenance.md` dosyasında belirtilen [Severity (Ciddiyet) ve Priority (Öncelik)] kurallarına göre sınıflandırılacaktır.
* **İletişim Planı:**
    * **Günlük İletişim:** Günlük Scrum (Stand-up) toplantılarında ilerleme, bulunan `Blocker`'lar ve engeller bildirilir.
    * **Haftalık Test Özeti Raporu:** Her Cuma günü PM ve PO'ya e-posta ile gönderilir. İçeriği:
        * Test Senaryosu İlerlemesi (Planlanan vs Çalıştırılan vs Başarılı/Başarısız)
        * Hata Durumu (Açılan vs Kapatılan - Ciddiyete göre döküm)
        * Karşılaşılan Engeller ve Riskler
    * **Test Kapatma Raporu (Test Closure Report):** Testten Çıkış Ölçütleri (8.4) karşılandığında, "Go/No-Go" toplantısı için hazırlanır. İçeriği:
        * Test sürecinin tam özeti.
        * Kapsamın ne kadarının test edildiği (Code Coverage ve Gereksinim Coverage).
        * Kalan risklerin (açık bug'lar) dökümü ve PO onayı.
        * Sürümün kalitesine dair QA ekibinin nihai görüşü (Go/No-Go tavsiyesi).

---

## 11. Onay

Bu Test Planı aşağıda imzası bulunan paydaşlar tarafından okunmuş, anlaşılmış ve projenin test faaliyetleri için temel alınması kabul edilmiştir.

| Rol | İsim | İmza | Tarih |
| :--- | :--- | :--- | :--- |
| **Ürün Sahibi (PO)** | [İsim Soyisim] | .................... | |
| **QA Lideri / Yöneticisi** | [İsim Soyisim] | .................... | |
| **Geliştirme Lideri** | [İsim Soyisim] | .................... | |
| **Proje Yöneticisi (PM)** | [İsim Soyisim] | .................... | |