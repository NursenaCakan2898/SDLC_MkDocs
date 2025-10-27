# 🚀 Kapsamlı Geliştirme Süreci Kılavuzu

Geliştirme aşaması, analiz ve tasarımda planlanan soyut fikirlerin ve mimarilerin, somut, çalışan ve değerli bir ürüne dönüştüğü "inşa" aşamasıdır. Bu aşamanın amacı sadece "çalışan" kod yazmak değil; aynı zamanda **temiz, sürdürülebilir, test edilebilir ve ölçeklenebilir** kod yazmaktır.

Kötü yönetilen bir geliştirme süreci, projenin ilerleyen aşamalarında "Teknik Borç" (Technical Debt) olarak bilinen, düzeltilmesi çok maliyetli ve zaman alıcı sorunlara yol açar. Bu kılavuz, tüm yazılım projelerinde (Masaüstü, Web, Gömülü Sistemler, Mobil) kaliteyi güvence altına alacak standartları belirler.

---

## 📜 1. Kod Standartları ve Kalite

Kod standartları, bir ekibin aynı dili konuşmasını sağlayan "dil bilgisi" kurallarıdır. Kodun kimin tarafından yazıldığından bağımsız olarak, tek bir kişi tarafından yazılmış gibi tutarlı ve okunabilir olmasını hedefler.

### 1.1. Kodlama Rehberleri (Style Guides) ve Formatlama

Kodun nasıl görüneceğini tanımlayan kurallardır (değişken isimlendirme, girintileme, dosya yapısı vb.).

* **Önemi:**
    * **Okunabilirlik:** Kodun büyük bir kısmı yazılmaktan çok okunur. Tutarlı bir stil, kodu anlamayı hızlandırır.
    * **Bakım Kolaylığı:** Yeni geliştiriciler projeye daha hızlı adapte olur (onboarding).
    * **Hata Ayıklama:** Standart dışı kod, genellikle mantıksal hataları da gizler.

* **Teknikler ve Araçlar:**
    * **Rehber (Guide):** Her dil için endüstri standardı bir rehber seçilmelidir.
        * **Python:** `PEP8` (İsteğinizdeki örnek).
        * **JavaScript/React:** `Airbnb Style Guide` veya `StandardJS`.
        * **C# / .NET:** `Microsoft C# Coding Conventions`.
        * **C/C++ (Endüstriyel/Gömülü):** `MISRA C` (Güvenlik kritik sistemler için), `Google C++ Style Guide`.
    * **Linting (Kontrol):** Kodu statik olarak analiz edip bu rehberlere uymayan yerleri *raporlayan* araçlardır. (Örn: `Pylint`, `ESLint`, `Cppcheck`).
    * **Formatting (Formatlama):** Kodu otomatik olarak rehbere uygun hale *getiren* araçlardır. (Örn: `Black` (Python), `Prettier` (JS/TS), `go fmt` (Go)).

* **Kural:** Ekip hangi rehberi seçerse seçsin, "en iyi" rehber **"tutarlı olarak uygulanan"** rehberdir. Bu araçlar CI sürecine entegre edilmelidir.

### 1.2. Birim Test (Unit Testing)

Birim test, bir yazılımın en küçük, test edilebilir parçalarının (fonksiyonlar, metotlar, sınıflar) tek başına ve izole bir şekilde doğru çalışıp çalışmadığını doğrulayan otomatik testlerdir.

* **Önemi:**
    * **Güven (Confidence):** Kodunuzun bekendiği gibi çalıştığını kanıtlar.
    * **Regresyonu Önleme:** Yeni bir özellik eklerken veya iyileştirme yaparken, mevcut (eski) bir özelliği bozmadığınızdan emin olmanızı sağlar.
    * **Tasarımı İyileştirme:** Test edilmesi zor kod, genellikle kötü tasarlanmış koddur (yüksek bağlılık, düşük uyum). Test yazmak, sizi daha modüler ve temiz bir mimariye zorlar.
    * **Canlı Dokümantasyon:** Bir fonksiyonun nasıl kullanılması gerektiğini gösteren en iyi dokümantasyon, o fonksiyonun birim testidir.

* **Uygulama ve Hedefler:**
    * **Kapsam (Code Coverage):** Testlerinizin, kod tabanınızın yüzde kaçını çalıştırdığını gösteren bir metriktir.
    * **Hedef:** Tüm projeler için **minimum %85 kod kapsamı** hedeflenmelidir.
    * **Kritik Uyarı:** %100 kapsam, kodun hatasız olduğu anlamına gelmez. Sadece o kod satırlarının *çalıştırıldığını* garanti eder; tüm mantıksal yolları (örn: `if-else`'in tüm kombinasyonları) doğru *doğruladığınız* anlamına gelmez.
    * **Kapsam:**
        * *Web (API):* İş mantığı (business logic), veri doğrulama (validation) ve servis katmanları test edilmelidir.
        * *Masaüstü/Endüstriyel:* Veri işleme, ayrıştırma (parsing) ve hesaplama fonksiyonları test edilmelidir. (UI'ın kendisine değil, UI'ın çağırdığı *mantığa* odaklanılır).

---

## 🌳 2. Sürüm Kontrolü ve İş Akışı

Sürüm kontrolü (VCS - Version Control System), kodda yapılan değişikliklerin geçmişini tutan ve birden fazla geliştiricinin aynı proje üzerinde kaosa yol açmadan çalışmasını sağlayan bir sistemdir (örn: Git).

### 2.1. Branch (Dal) Stratejisi: Git Flow

Git Flow, projelerde sürüm yönetimini standartlaştıran, kanıtlanmış bir modeldir.

* **`main` (veya `master`):** 🚩 **ÜRETİM DALIDIR.**
    * Bu dal, her zaman *canlıda (production)* olan, stabil, test edilmiş ve onaylanmış kodun anlık görüntüsünü içerir.
    * Bu dala asla doğrudan commit atılmaz.
    * Sadece `hotfix` veya `release` dallarından "merge" kabul eder. Her merge, bir sürüm numarası ile etiketlenmelidir (örn: `v1.0.1`).

* **`develop`:** 🏗️ **ENTEGRASYON DALIDIR.**
    * Ana geliştirme dalıdır. Bir sonraki sürümde yer alacak *tamamlanmış* tüm özellikleri içerir.
    * Tüm `feature` (özellik) dalları bu daldan açılır ve işleri bittiğinde bu dala geri merge edilir.
    * CI sunucusu bu dalı otomatik olarak "staging" veya "test" ortamına dağıtmalıdır (deploy etmelidir).

* **`feature/*` (örn: `feature/user-login`):** 🧑‍💻 **ÖZELLİK DALI.**
    * Her yeni özellik, görev veya iyileştirme için `develop` dalından oluşturulur.
    * Geliştirme bu dalda yapılır.
    * İş bittiğinde, test edildiğinde ve kod incelemesinden geçtiğinde, `develop` dalına Pull Request (PR) ile birleştirilir.

* **`hotfix/*` (örn: `hotfix/critical-auth-bug`):** 🚑 **ACİL DÜZELTME DALI.**
    * *SADECE* `main` (üretim) üzerinde tespit edilen kritik hataları düzeltmek için *doğrudan `main` dalından* açılır.
    * Hata hızla düzeltilir.
    * İş bittiğinde, bu dal **HEM `main`'e (canlıyı düzeltmek için) HEM DE `develop`'a (gelecek sürümlerde aynı hatanın olmamasını sağlamak için)** merge edilir.

* **(Opsiyonel) `release/*` (örn: `release/v1.2.0`):** 🏷️ **SÜRÜM HAZIRLIK DALI.**
    * `develop` dalı bir sonraki sürüme hazır olduğunda, `develop`'dan açılır.
    * Bu dalda sadece sürüme hazırlık (versiyon numarasını güncelleme, son dakika küçük hata düzeltmeleri, dokümantasyon güncelleme) yapılır.
    * Hazır olduğunda `main`'e (sürümü yayınlamak için) ve `develop`'a (yapılan son değişiklikleri geri almak için) merge edilir.

### 2.2. Commit Mesajı Standartları (Conventional Commits)

Commit mesajları, projenin "neden" değiştiğini anlatan tarihçesidir. Konvansiyonel Commitler, bu mesajları makine tarafından okunabilir, standart bir formata sokar.

* **Format:** `<tür>(<opsiyonel-kapsam>): <konu>`
* **Önemi:**
    * Proje geçmişini (history) inanılmaz derecede okunabilir kılar.
    * Otomatik olarak Sürüm Notları (CHANGELOG) oluşturulmasını sağlar.
    * PR incelemelerini kolaylaştırır (değişikliğin niyetini anında gösterir).

* **Başlıca Türler (İsteğinizdeki örnekler):**
    * **`feat:`** (Feature) Yeni bir özellik eklendiğinde.
        * `feat: Kullanıcılar için e-posta ile parola sıfırlama eklendi`
    * **`fix:`** Koddaki bir hata (bug) düzeltildiğinde.
        * `fix: API'deki 'null pointer' hatası yanlış veri girişinde oluşuyordu`
    * **`docs:`** Sadece dokümantasyonda (örn: README, .md dosyaları) değişiklik yapıldığında.
        * `docs: Geliştirme süreci kılavuzu için branch stratejisi eklendi`
    * **`refactor:`** Kullanıcıyı etkilemeyen, mevcut bir kodun yeniden düzenlenmesi (daha temiz, daha performanslı hale getirilmesi).
        * `refactor: Veritabanı sorgu servisi 'async/await' kullanacak şekilde iyileştirildi`
    * **`style:`** Sadece kod formatlaması veya linting düzeltmeleri yapıldığında.
        * `style: PEP8 uyumluluğu için tüm projede 'Black' çalıştırıldı`
    * **`test:`** Eksik testlerin eklenmesi veya mevcut testlerin düzeltilmesi.
        * `test: Parola sıfırlama servisi için birim testler eklendi`
    * **`ci:`** CI/CD yapılandırma dosyalarında (örn: GitHub Actions, Jenkins) değişiklik yapıldığında.
        * `ci: Pull Request için 'pytest' adımını zorunlu hale getir`

---

## ⚙️ 3. Sürekli Entegrasyon (CI - Continuous Integration)

Sürekli Entegrasyon, geliştiricilerin kod değişikliklerini (genellikle `feature` dallarından `develop`'a) sık sık ve otomatik bir süreçle birleştirmesidir.

* **Önemi:**
    * **Erken Hata Tespiti:** Uyumsuz kod parçaları, birleşme (merge) anında, tüm projeyi kilitlemeden önce tespit edilir.
    * **"Entegrasyon Cehennemi"ni Önler:** Geliştiricilerin haftalarca kendi dallarında çalışıp, sonra birleştirmeye çalıştıklarında yaşadıkları devasa çakışmaları (conflict) engeller.
    * **Oto-Kalite Kontrol:** Her değişikliğin standartlara (lint) ve testlere (unit test) uyduğunu garanti eder.

### 3.1. Pull Request (PR) ile Otomatik Test

PR (veya GitLab'de Merge Request), bir daldaki değişiklikleri başka bir dala (örn: `feature` -> `develop`) birleştirmek için yapılan bir "tekliftir".

* **Akış:**
    1.  Geliştirici, `feature/user-login` dalında işini bitirir.
    2.  `develop`'a birleştirmek için bir **Pull Request** açar.
    3.  PR açıldığı anda, CI sunucusu (GitHub Actions, Jenkins, GitLab CI) otomatik olarak tetiklenir.
    4.  CI Pipeline (İş Akışı) şu adımları çalıştırır:
        * **Derleme (Build):** Bağımlılıkları yükler (örn: `pip install -r requirements.txt`) ve projeyi derler (eğer C#, Java, C++ gibi derlenen bir dil ise).
        * **Lint & Format:** Kodun standartlara uyup uymadığını kontrol eder (`black --check`, `eslint .`).
        * **Test:** Tüm birim testleri çalıştırır (`pytest`, `npm test`).
    5.  **Sonuç:** Bu adımlardan **herhangi biri başarısız olursa**, CI süreci "Başarısız" (FAILED) olur. PR'ın `develop`'a birleştirilmesi **otomatik olarak bloke edilir**.

### 3.2. Kod İncelemesi (Code Review)

CI makinelerin yaptığı kontroldür; Kod İncelemesi ise *insanların* yaptığı mantıksal ve mimari kontroldür.

* **Zorunluluk:** Bir PR'ın birleştirilebilmesi için, CI testlerini geçmenin yanı sıra, **en az 1 (veya kritik modüller için 2) başka ekip üyesinden "Onay" (Approve) alması zorunludur.**
* **Amaç:**
    * Sadece bariz hataları değil (CI onu yakalar), *mantıksal* hataları, *kötü mimari* kararlarını ve *performans* sorunlarını bulmak.
    * Bilgi Paylaşımı (Knowledge Sharing): Ekibin geri kalanının kod tabanının o kısmında neler değiştiğini öğrenmesini sağlar.
    * Standartlara uyumu (örn: "Bu API, tasarım dokümanındaki API sözleşmesine uymuyor") denetler.

---

## ✅ 4. Bitti Tanımı (Definition of Done - DoD)

DoD, bir iş kaleminin (feature, task, bugfix) "tamamlandı" olarak kabul edilmesi için gereken, herkesin üzerinde anlaştığı ve hiçbir yoruma yer bırakmayan **kontrol listesidir**. "Benim makinemde çalışıyor" ifadesini geçersiz kılar.

Bir görev (task) için DoD kontrol listesi:

* [ ] **Gereksinimler Karşılandı:** Kod, kabul kriterlerinde (Acceptance Criteria) belirtilen tüm işlevselliği karşılıyor.
* [ ] **Birim Testleri Yazıldı ve Geçti:** Kod, birim testleri ile kapsandı (örn: > %85 kapsam hedefi) ve `pytest` veya benzeri bir araçta tüm testler **BAŞARILI (GREEN)**. (İsteğinizden)
* [ ] **CI Pipeline Başarılı:** İlgili Pull Request'teki tüm otomatik CI adımları (Build, Lint, Test) **BAŞARILI (GREEN)**.
* [ ] **Kod İncelemesi Tamamlandı:** Gerekli sayıda (örn: 1) ekip üyesinden **"Onay" (Approve)** alındı ve tüm yorumlar (comments) çözümlendi (resolved).
* [ ] **PR Açıklaması Yeterli:** PR, yapılan değişikliği net bir şekilde açıklıyor ve ilgili görev kartına (Jira, Trello vb. issue) link verilmiş. (İsteğinizden)
* [ ] **Dokümantasyon Güncellendi:** Eğer bu değişiklik kullanıcıyı (Kullanım Kılavuzu), API'ı (Swagger/OpenAPI) veya sistem tasarımını (design.md) etkiliyorsa, ilgili dokümanlar güncellendi. (İsteğinizden)
* [ ] **Ana Dala Merge Edildi:** Kod, `develop` dalına (veya `hotfix` ise `main`'e) başarıyla ve çakışmasız (no conflicts) olarak birleştirildi.