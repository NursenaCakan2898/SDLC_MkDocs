# 🧪 Kapsamlı Test Senaryoları (Test Cases) Kılavuzu ve Şablonu

---

## 1. Giriş ve Amaç

Bu doküman, `[Proje Adı]` yazılımı için yürütülecek test senaryolarını (test vakalarını) tanımlamak için bir şablon ve rehber görevi görür.

Test senaryoları, bir test planının eyleme dökülmüş halidir. `Test Planı` bize "nereye gideceğimizi" (hedef) ve "hangi yolları kullanacağımızı" (strateji) söylerken, **Test Senaryoları** bize "hangi adımları atacağımızı" (navigasyon) ve "vardığımızı nasıl anlayacağımızı" (beklenen sonuç) tarif eder.

İyi tanımlanmış test senaryoları:
* **Tutarlılık sağlar:** Her test uzmanı (veya otomasyon script'i) aynı adımları izleyerek aynı testi yapar.
* **Kapsamı ölçer:** Kaç gereksinimin test edildiğini ve ne kadarının başarılı olduğunu net bir şekilde gösterir.
* **Bilgi Aktarımı sağlar:** Projeye yeni katılan birinin, sistemin nasıl çalışması gerektiğini hızla öğrenmesini sağlar.
* **Regresyonu önler:** Bu senaryolar, projenin "regresyon test paketi"nin temelini oluşturur.

> **Not:** Bu `.md` dosyası bir şablondur. Modern projelerde bu senaryolar, `Jira (Xray, Zephyr)`, `TestRail`, `Azure DevOps (Test Plans)` gibi özel Test Yönetim Araçları'nda (Test Management Tools) tutulur ve yönetilir. Bu şablon, o araçlara yüklenecek verinin yapısını tanımlar.

---

## 2. Detaylı Test Senaryosu Şablonu

Aşağıdaki tablo, bir test senaryosunu tanımlamak için gereken tüm alanları (nitelikleri) göstermektedir.

| Alan Adı | Açıklama | Örnek |
| :--- | :--- | :--- |
| **Test Case ID** | Proje genelinde benzersiz ve sıralı kimlik. `TC-[MODÜL]-[NUMARA]` formatı önerilir. | `TC-LOGIN-001` |
| **Modül / Özellik** | Test senaryosunun ilgili olduğu ana sistem bileşeni veya kullanıcı hikayesi (User Story). | `Kimlik Doğrulama` |
| **Başlık (Amaç)** | Testin amacını *net, kısa ve öz* bir şekilde açıklayan başlık. | Başarılı kullanıcı girişi (Happy Path) |
| **Ön Koşullar** | Bu testin başlayabilmesi için *mutlaka* sağlanmış olması gereken şartlar. | 1. Kullanıcı `test_user_aktif` sistemde 'Aktif' statüde kayıtlı olmalı.<br>2. Kullanıcının şifresi `Sifre123!` olarak tanımlı olmalı.<br>3. `Staging` ortamı ayakta ve erişilebilir olmalı. |
| **Test Adımları** | Testi gerçekleştirmek için izlenecek 1'den N'e kadar sıralı, *küçük, net ve eyleme dönük* komutlar. | `1. Giriş sayfasını aç (URL: staging.proje.com/login).`<br>`2. 'Kullanıcı Adı' alanına [Test Verisi]'ni gir.`<br>`3. 'Şifre' alanına [Test Verisi]'ni gir.`<br>`4. 'Giriş Yap' butonuna tıkla.` |
| **Test Verisi** | Test adımlarında kullanılacak spesifik giriş verileri. | `Kullanıcı Adı: test_user_aktif`<br>`Şifre: Sifre123!` |
| **Beklenen Sonuç** | Test adımları uygulandığında *gözlemlenebilir ve ölçülebilir* sonuç. "Çalışmalı" gibi muğlak ifadelerden kaçınılmalı. | `1. Sistem, kullanıcıyı 2 saniyeden kısa sürede doğrulamalı.`<br>`2. Kullanıcı 'Ana Sayfa'ya (URL: /dashboard) yönlendirilmeli.`<br>`3. Sağ üst köşede 'Hoş Geldin, test_user_aktif' metni görünmeli.` |
| **Öncelik (Priority)** | Testin iş açısından önemi. Regresyon testlerinde hangi testlerin *önce* koşulacağını belirler. | `P0 (Critical)`, `P1 (High)`, `P2 (Medium)`, `P3 (Low)` |
| **Tür (Type)** | Testin kategorisi. | `Fonksiyonel`, `UI/UX`, `API`, `Performans`, `Güvenlik`, `Haberleşme`, `Regresyon` |
| **Yazar** | Test senaryosunu oluşturan kişi. | `C. Akan` |
| **Durum (Status)** | Test senaryosunun yaşam döngüsü. | `Draft`, `Ready for Review`, `Ready to Run`, `Needs Update` |
| **(Yürütme) Sonuç** | *(Test yürütülürken doldurulur)* | `Pass` (Başarılı), `Fail` (Başarısız), `Skip` (Atlandı), `Blocked` (Engellendi) |
| **(Yürütme) Bulgu ID**| *(Test 'Fail' olursa doldurulur)* İlgili hata (defect/bug) kaydının ID'si. | `BUG-452` |

---

## 3. Örnek Test Senaryoları (Yazılım Türlerine Göre)

Aşağıda, farklı yazılım türleri için doldurulmuş detaylı test senaryosu örnekleri bulunmaktadır.

### Örnek 1: Web / API - Başarılı Giriş (Happy Path)

| Alan Adı | Değer |
| :--- | :--- |
| **Test Case ID** | `TC-AUTH-001` |
| **Modül / Özellik** | Kimlik Doğrulama |
| **Başlık (Amaç)** | Geçerli ve aktif bir kullanıcının sisteme başarılı şekilde giriş yapması. |
| **Ön Koşullar** | 1. Kullanıcı `qa_user_valid` sistemde `Aktif` statüde kayıtlı olmalı.<br>2. Kullanıcının şifresi `ValidPass@123` olarak tanımlı olmalı.<br>3. `Staging` web sunucusu (staging.proje.com) çalışır durumda olmalı. |
| **Test Adımları** | 1. Tarayıcıyı aç.<br>2. `staging.proje.com/login` adresine git.<br>3. 'Kullanıcı Adı' metin kutusuna [Test Verisi]'ni gir.<br>4. 'Şifre' metin kutusuna [Test Verisi]'ni gir.<br>5. 'Giriş Yap' butonuna tıkla. |
| **Test Verisi** | `Kullanıcı Adı: qa_user_valid`<br>`Şifre: ValidPass@123` |
| **Beklenen Sonuç** | 1. Kullanıcı `/dashboard` (Ana Sayfa) adresine yönlendirilmeli.<br>2. Sayfanın sağ üst köşesinde 'Hoş Geldin, qa_user_valid' yazısı görünmeli.<br>3. 'Giriş Yap' veya 'Kayıt Ol' linkleri artık görünmemeli, yerine 'Çıkış Yap' linki gelmeli. |
| **Öncelik (Priority)** | `P0 (Critical)` (Bu çalışmazsa sistemin kalanı test edilemez) |
| **Tür (Type)** | `Fonksiyonel`, `Regresyon`, `UI` |
| **Yazar** | `C. Akan` |
| **Durum (Status)** | `Ready to Run` |

### Örnek 2: Web / API - Hatalı Giriş (Negative Path)

| Alan Adı | Değer |
| :--- | :--- |
| **Test Case ID** | `TC-AUTH-002` |
| **Modül / Özellik** | Kimlik Doğrulama |
| **Başlık (Amaç)** | Kayıtlı bir kullanıcı için *yanlış şifre* girildiğinde sistemin girişi engellemesi ve hata mesajı göstermesi. |
| **Ön Koşullar** | 1. Kullanıcı `qa_user_valid` sistemde `Aktif` statüde kayıtlı olmalı.<br>2. `Staging` web sunucusu (staging.proje.com) çalışır durumda olmalı. |
| **Test Adımları** | 1. Tarayıcıyı aç.<br>2. `staging.proje.com/login` adresine git.<br>3. 'Kullanıcı Adı' metin kutusuna [Test Verisi]'ni gir.<br>4. 'Şifre' metin kutusuna [Test Verisi]'ni gir.<br>5. 'Giriş Yap' butonuna tıkla. |
| **Test Verisi** | `Kullanıcı Adı: qa_user_valid`<br>`Şifre: YANLIS_SIFRE` |
| **Beklenen Sonuç** | 1. Kullanıcı `/dashboard` adresine **yönlendirilmemeli**. URL, `/login` olarak kalmalı.<br>2. 'Şifre' metin kutusunun altında veya sayfanın üstünde kırmızı renkte "Kullanıcı adı veya şifre hatalı." hata mesajı görünmeli.<br>3. Şifre alanı otomatik olarak temizlenmeli. |
| **Öncelik (Priority)** | `P1 (High)` |
| **Tür (Type)** | `Fonksiyonel`, `Güvenlik` |
| **Yazar** | `C. Akan` |
| **Durum (Status)** | `Ready to Run` |

### Örnek 3: Masaüstü (Desktop) - Proje Kaydetme

| Alan Adı | Değer |
| :--- | :--- |
| **Test Case ID** | `TC-DESKTOP-FILE-005` |
| **Modül / Özellik** | Dosya Yönetimi |
| **Başlık (Amaç)** | Aktif bir projeyi 'Farklı Kaydet' seçeneği ile `.projeX` formatında başarıyla kaydetmek. |
| **Ön Koşullar** | 1. Masaüstü uygulaması (`v2.1.0-staging`) Windows 11 test makinesine kurulmuş olmalı.<br>2. Uygulama açılmış ve boş bir proje ekranı görünür olmalı. |
| **Test Adımları** | 1. Uygulamada `[Veri Ekle]` butonuna tıkla.<br>2. Açılan pencerede 'Cihaz Adı' alanına `Test Cihazı` yaz ve 'Ekle'ye bas.<br>3. Ana menüden `Dosya -> Farklı Kaydet...` seçeneğine tıkla.<br>4. Açılan 'Kaydet' diyaloğunda, dosya adı olarak `TestProjem` yaz.<br>5. Kayıt yeri olarak `Masaüstü`'nü seç.<br>6. 'Kaydet' butonuna tıkla.<br>7. Uygulamayı kapat.<br>8. `Masaüstü` dizinine git. |
| **Test Verisi** | `Cihaz Adı: Test Cihazı`<br>`Dosya Adı: TestProjem` |
| **Beklenen Sonuç** | 1. Adım 6'dan sonra 'Kaydet' diyaloğu kapanmalı.<br>2. Uygulamanın başlık çubuğunda (title bar) `TestProjem.projeX` yazmalı.<br>3. Adım 8'den sonra `Masaüstü`'nde `TestProjem.projeX` isimli bir dosya var olmalı.<br>4. Dosyanın boyutu > 0 KB olmalı. |
| **Öncelik (Priority)** | `P0 (Critical)` |
| **Tür (Type)** | `Fonksiyonel` |
| **Yazar** | `C. Akan` |
| **Durum (Status)** | `Ready to Run` |

### Örnek 4: Endüstriyel (HMI/SCADA) - PLC Veri Okuma

| Alan Adı | Değer |
| :--- | :--- |
| **Test Case ID** | `TC-COMM-001` |
| **Modül / Özellik** | PLC Haberleşmesi (Modbus) |
| **Başlık (Amaç)** | HMI uygulamasının, bağlı PLC'den (Simülatör) `40001` adresindeki (Holding Register) sıcaklık verisini doğru okuması ve göstermesi. |
| **Ön Koşullar** | 1. Masaüstü HMI uygulaması (`v2.1.0-staging`) çalışıyor olmalı.<br>2. Modbus PLC Simülatörü (`ModbusPal`) çalışıyor ve `40001` adresine `1234` değerini basıyor olmalı.<br>3. HMI, `Ayarlar -> Haberleşme` ekranında Simülatör IP'sine (örn. 127.0.0.1:502) bağlı (`Bağlı` durumu yeşil) olmalı. |
| **Test Adımları** | 1. HMI uygulamasında `Ana Sayfa` ekranını aç.<br>2. `Sıcaklık Değeri` etiketli göstergeyi (gauge) izle.<br>3. PLC Simülatöründe, `40001` adresindeki değeri `1234`'ten `5678`'e değiştir.<br>4. HMI ekranını 5 saniye boyunca izle. |
| **Test Verisi** | `Adres: 40001`<br>`Değer 1: 1234`<br>`Değer 2: 5678` |
| **Beklenen Sonuç** | 1. Adım 2'de, HMI'daki gösterge `123.4` değerini göstermeli (Register değeri / 10 olarak ayarlandığı varsayıldı).<br>2. Adım 3'ten sonra, HMI'daki göstergenin değeri (en fazla 2 saniyelik okuma periyodu içinde) `567.8` olarak güncellenmeli. |
| **Öncelik (Priority)** | `P0 (Critical)` |
| **Tür (Type)** | `Fonksiyonel`, `Haberleşme` |
| **Yazar** | `C. Akan` |
| **Durum (Status)** | `Ready to Run` |

### Örnek 5: Mobil (Android/iOS) - Çevrimdışı (Offline) Mod

| Alan Adı | Değer |
| :--- | :--- |
| **Test Case ID** | `TC-MOB-OFFLINE-003` |
| **Modül / Özellik** | Görev Yönetimi (Offline Mod) |
| **Başlık (Amaç)** | Cihaz çevrimdışı iken yeni bir 'Görev' oluşturulması ve cihaz çevrimiçi olduğunda sunucuyla senkronize edilmesi. |
| **Ön Koşullar** | 1. Mobil uygulama (`v1.5.0-staging`) bir test cihazına (Android 12) kurulmuş olmalı.<br>2. Kullanıcı ` saha_calisani` sisteme giriş yapmış olmalı. |
| **Test Adımları** | 1. Cihazın WiFi ve Mobil Veri bağlantılarını kapat (Uçak Moduna Al).<br>2. Mobil uygulamayı aç.<br>3. `+ Yeni Görev` butonuna tıkla.<br>4. 'Görev Başlığı' alanına `Offline Test Görevi` yaz.<br>5. 'Kaydet'e tıkla.<br>6. 'Görev Listesi' ekranına geri dön.<br>7. Cihazın Uçak Modunu kapat (WiFi veya Mobil Veriyi aç).<br>8. 30 saniye bekle.<br>9. Web arayüzünden (`staging.proje.com`) `saha_calisani` kullanıcısı ile giriş yap.<br>10. `Görevler` sayfasına git. |
| **Test Verisi** | `Görev Başlığı: Offline Test Görevi` |
| **Beklenen Sonuç** | 1. Adım 5'ten sonra, 'Görev Listesi'nde `Offline Test Görevi` görünmeli ve yanında 'Senkronize Bekliyor' (örn. bulut-çizgi ikonu) simgesi olmalı.<br>2. Adım 7'den sonra, bu simge 'Senkronize Edildi' (örn. yeşil tik) simgesine dönüşmeli.<br>3. Adım 10'da, Web arayüzündeki görev listesinde `Offline Test Görevi` görünür olmalı. |
| **Öncelik (Priority)** | `P1 (High)` |
| **Tür (Type)** | `Fonksiyonel`, `Kesinti & Kurtarma` |
| **Yazar** | `C. Akan` |
| **Durum (Status)** | `Ready to Run` |