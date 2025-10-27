# 🗒️ Kapsamlı Sürüm Notları (Release Notes) Kılavuzu ve Şablonu

---

## 1. Sürüm Notlarının Önemi

Sürüm Notları (veya Değişiklik Günlüğü - Changelog), bir yazılımın yeni bir sürümüyle birlikte yayınlanan, o sürümdeki **değişiklikleri, yenilikleri ve düzeltmeleri** özetleyen resmi bir dokümandır.

Bu doküman, projenizin "gazetesi" gibidir ve basit bir formaliteden çok daha fazlasıdır:

* **Şeffaflık ve Güven:** Kullanıcılarınıza (son kullanıcı veya geliştirici) neyin değiştiğini açıkça söylemek, onlarla aranızda bir güven bağı oluşturur.
* **Dahili İletişim:** Destek (Support) ekibinizin, satış (Sales) ekibinizin ve diğer geliştiricilerin, yeni sürümde neyin geldiğini veya hangi hatanın çözüldüğünü bilmesini sağlar. Destek ekibi, "Bu sorun `v1.2.1`'de çözüldü" diyebilir.
* **Teknik Dokümantasyon:** Projenin evrimini takip etmek için paha biçilmez bir tarihsel kayıt (log) görevi görür. Bir hatanın ne zaman ortaya çıktığını veya bir özelliğin ne zaman eklendiğini bulmayı kolaylaştırır.
* **Pazarlama:** Yeni ve heyecan verici özellikleri duyurmak için bir araç olarak kullanılabilir.
* **Risk Yönetimi:** Kullanıcıları "Kırıcı Değişiklikler (Breaking Changes)" konusunda önceden uyararak, güncelleme yaptıklarında yaşayacakları sorunları en aza indirir.

## 2. "İyi" Sürüm Notu Yazmak İçin İpuçları

1.  **Kitleyi Tanıyın (Audience-Aware):**
    * **Teknik Kitle (API, Kütüphane):** Detaylı olun. Fonksiyon adları, API endpoint'leri ve teknik değişiklikler (örn. "N+1 sorgusu çözüldü") yazmaktan çekinmeyin.
    * **Son Kullanıcı (Masaüstü, Mobil):** Jargondan kaçının. Kullanıcıya *değer* katan şeyleri vurgulayın. "Null pointer exception düzeltildi" YERİNE "Raporlar sayfasındaki çökme sorunu giderildi" deyin.
    * **Uzman Kitle (Endüstriyel, Bilimsel):** Spesifik olun. "Haberleşme iyileştirildi" YERİNE "Modbus TCP okuma gecikmesi 500ms'den 100ms'ye düşürüldü" deyin.

2.  **Anlamlı Gruplama Yapın:** (Şablonumuzdaki gibi) Değişiklikleri `Yeni`, `Düzeltme`, `İyileştirme` gibi mantıksal kategorilere ayırın. "Diğer değişiklikler" başlığı altında 20 madde listelemekten kaçının.

3.  **Spesifik ve Eyleme Yönelik Olun:**
    * **Kötü:** "Çeşitli hata düzeltmeleri ve iyileştirmeler yapıldı." (Bu hiçbir şey ifade etmez).
    * **İyi:** "Kullanıcı silindiğinde, o kullanıcıya ait siparişlerin anonimleştirilmemesi sorunu düzeltildi."

4.  **Görev (Task) ID'lerini Ekleyin:** Özellikle dahili veya teknik sürüm notlarında, her değişikliği ilgili Jira, Trello veya GitHub Issue ID'si ile ilişkilendirin. Bu, izlenebilirliği sağlar.
    * Örn: `- [TASK-123] Kullanıcılar artık profillerine fotoğraf ekleyebilir.`

5.  **Commit Mesajlarını Kopyalamayın:** Git commit mesajları (`git log`) geliştiriciye yöneliktir ve genellikle çok teknik veya dağınıktır. Sürüm notları ise *son ürüne* odaklanır.

---

## 3. Kapsamlı Sürüm Notları Şablonu (Keep a Changelog Formatı)

Aşağıdaki şablon, "Keep a Changelog" (keepachangelog.com) standardına dayalıdır ve endüstrinin en iyi uygulamalarını yansıtır.

```markdown
# Sürüm Notları: [Proje Adı]

## [vX.Y.Z] - [YYYY-AA-GG]

[Bu sürümdeki ana temayı veya en önemli değişikliği özetleyen kısa bir paragraf. Örn: "Bu sürüm, yeni Raporlama Modülümüzü tanıtıyor, kritik bir güvenlik açığını kapatıyor ve 20'den fazla performans iyileştirmesi içeriyor."]
```
### ✨ Yeni Özellikler (Added)
* [İlgili Görev ID, örn. TASK-123] - [Yeni eklenen ve kullanıcıya değer katan özellik. Örn: "Kullanıcılar artık profillerine fotoğraf ekleyebilir."]
* [TASK-124] - [Başka bir yeni özellik. Örn: "Raporlama modülüne 'PDF Dışa Aktar' seçeneği eklendi."]

### 🎨 İyileştirmeler (Changed / Enhanced)
* [TASK-130] - [Mevcut bir özellikte yapılan değişiklik veya iyileştirme. Örn: "Ana sayfa yükleme performansı, sorgular optimize edilerek %40 iyileştirildi."]
* [TASK-131] - [Bir UI değişikliği. Örn: "Giriş (Login) ekranının tasarımı daha modern bir görünümle güncellendi."]
* [TASK-132] - [Bir bağımlılık güncellemesi. Örn: "Sistem, .NET 6'dan .NET 8'e (LTS) yükseltildi."]

### 🐞 Hata Düzeltmeleri (Fixed)
* [BUG-201] - [Düzeltilen bir hata. Örn: "Belirli koşullarda sepet toplamının negatif görünmesi sorunu giderildi."]
* [BUG-202] - [Düzeltilen başka bir hata. Örn: "Firefox tarayıcısında 'Tarih Seçici' bileşeninin açılmaması sorunu düzeltildi."]
* [BUG-203] - [Düzeltilen daha spesifik bir hata. Örn: "Masaüstü uygulamasının, 1000'den fazla kayıt içeren bir projeyi açarken çökmesi sorunu giderildi."]

### 🔒 Güvenlik (Security)
* [SEC-040] - [Kapatılan bir güvenlik açığı. Örn: "Kullanıcı profili sayfasında bulunan bir XSS (Cross-Site Scripting) açığı kapatıldı. (CVE-2025-XXXX)"]
* [SEC-041] - [Bir yetkilendirme hatası. Örn: "Normal kullanıcıların, yönetici (admin) API endpoint'lerine yetkisiz erişebilmesi sorunu düzeltildi."]

### ⚠️ ÖNEMLİ: Kırıcı Değişiklikler (Breaking Changes)
* [TASK-150] - [Geliştiricilerin veya kullanıcıların **MUTLAKA** bilmesi gereken, geriye dönük uyumluluğu bozan değişiklik. Örn: "Web API: `/api/v1/user/{id}` endpoint'i kaldırıldı. Lütfen yerine `/api/v2/users/{id}` kullanın."]
* [TASK-151] - [Bir veri tabanı değişikliği. Örn: "Veritabanı şeması değişti. Bu sürüme geçerken `migration-v2.sql` script'inin çalıştırılması **ZORUNLUDUR**."]
* [TASK-152] - [Bir konfigürasyon değişikliği. Örn: `config.json` dosyasındaki `DbConnection` ayarı, `Database.ConnectionString` olarak değiştirildi."]

### 🗑️ Kaldırılan / Kullanımdan Düşen (Removed / Deprecated)
* [TASK-160] - [Kullanımdan kaldırılan ama *henüz* silinmeyen özellik. Örn: "Eski 'Raporlama v1' modülü 'Deprecated' olarak işaretlendi. v3.0.0'da tamamen kaldırılacaktır. Lütfen 'Raporlama v2'ye geçiş yapın."]
* [TASK-161] - [Kaldırılan bir özellik. Örn: "Internet Explorer 11 desteği bu sürümle birlikte sonlandırılmıştır."]

### 🔧 Teknik Notlar (Technical Notes)
* [Proje `Django`'yu v4.1'den v5.0'a yükseltti.]
* [`pymodbus` kütüphanesi v3.0'a güncellendi.]

### 🐛 Bilinen Sorunlar (Known Issues)
* [BUG-205] - [Bu sürümle birlikte yayınlandığını bildiğiniz ancak henüz düzeltilmemiş sorunlar. Örn: "Safari tarayıcısında 'PDF Dışa Aktar' özelliği çalışmayabilir. Geçici çözüm olarak Chrome kullanabilirsiniz."]

## [v2.5.0] - 2025-10-27

Bu sürüm, API'nin v2 versiyonuna geçişi tamamlamakta ve kritik bir N+1 sorgu problemini çözmektedir.

### ⚠️ Kırıcı Değişiklikler (Breaking Changes)
* **[ÖNEMLİ]** `/api/v1/auth` endpoint'i tamamen kaldırıldı. Lütfen JWT token almak için `/api/v2/token` (OAuth2 uyumlu) endpoint'ini kullanın.
* `/api/v1/products` yanıtındaki `price` alanı `string`'den `decimal`'e (para birimi uyumlu) çevrildi.

### ✨ Yeni Özellikler (Added)
* [TASK-451] Yeni `/api/v2/reports` endpoint'i eklendi. (Detaylar için güncellenen Swagger/OpenAPI dokümantasyonuna bakın).

### 🎨 İyileştirmeler (Changed)
* [TASK-444] `/api/v1/users` endpoint'inde N+1 sorgu problemi çözüldü, ortalama yanıt süresi 1200ms'den 150ms'ye düşürüldü.

## [Sürüm 1.2.0] - 27 Ekim 2025

Bu güncelleme ile sizlere yepyeni bir "Karanlık Mod" seçeneği sunuyor ve sık yaşanan bir çökme sorununu gideriyoruz!

### ✨ Yeni Özellikler
* **Karanlık Mod Geldi!:** Göz yorgunluğunu azaltmak için "Ayarlar -> Görünüm -> Karanlık Mod" seçeneğini kullanarak uygulamanın temasını değiştirebilirsiniz.
* **PDF Dışa Aktarma:** Projelerinizi artık "Dosya -> PDF Olarak Dışa Aktar" menüsünü kullanarak PDF formatında kaydedebilirsiniz.

### 🐞 Hata Düzeltmeleri
* Bazen 100'den fazla kayıt içeren büyük projeleri açmaya çalışırken uygulamanın çökmesine neden olan bir hata düzeltildi.
* "Kaydet" butonunun, değişiklik yapılmasına rağmen bazen aktifleşmemesi (gri kalması) sorunu giderildi.
* Çeşitli yazım hataları düzeltildi.

## [FW Sürümü: v3.1.5-HMI] - 2025-10-27

**UYARI:** Bu sürüm, alarm veritabanı şemasını güncellemektedir. Güncellemeden önce Bölüm 3'teki notları okuyun.

### 🐞 Hata Düzeltmeleri (Fixed)
* [BUG-112] Siemens S7-1200 (FW v4.4+) ile kurulan Modbus TCP bağlantısının, yaklaşık 2 saatlik sürekli çalışmanın ardından kopmasına neden olan bir hafıza sızıntısı (memory leak) düzeltildi.
* [BUG-113] "Acil Stop" basılıyken "Üretimi Başlat" butonuna basıldığında HMI ekranının donması sorunu giderildi.

### 🎨 İyileştirmeler (Changed)
* [TASK-204] Operatör hatalarını önlemek amacıyla, "Reçete" (Recipe) ekranındaki "PLC'ye Yükle" butonu için ekstra bir "Emin misiniz?" onay diyaloğu eklendi.

### ⚠️ Kırıcı Değişiklikler (Breaking Changes)
* Alarm veritabanı (`AlarmHistory.db`) şeması, milisaniye (ms) hassasiyeti eklemek için güncellendi. Bu sürümü yüklemeden önce eski alarm kayıtlarınızı (`C:\ProgramData\Proje\Logs\`) yedeklemeniz **ZORUNLUDUR**. Eski kayıtlar, güncelleme sonrası erişilemez olacaktır.