# 🚀 Kapsamlı Dağıtım (Deployment) ve Yayın Yönetimi Kılavuzu

Dağıtım (Deployment), test edilmiş bir yazılım paketinin (artifact) son kullanıcıların veya sistemlerin erişebileceği bir çevreye kurulması ve çalıştırılması sürecidir. Yayın Yönetimi (Release Management) ise bu sürecin planlanması, zamanlanması ve kontrol edilmesidir.

Bu kılavuz, basit bir web sitesinden karmaşık bir endüstriyel sisteme kadar her türlü yazılımın dağıtım stratejilerini ve en iyi uygulamalarını kapsamaktadır.

---

## 1. 🏛️ Çevre Yönetimi (Environment Management)

Yazılım, canlı (Production) ortama ulaşmadan önce kalitesini güvence altına almak için bir dizi çevreden geçmelidir. Bu, "bir kez inşa et, her yerde dağıt" (Build once, deploy everywhere) prensibine dayanır.

1.  **Geliştirme (Development / `dev`)**
    * **Amaç:** Geliştiricinin kendi makinesinde (veya `localhost`) kod yazdığı, anlık testler yaptığı, son derece kararsız (unstable) ortamdır.
    * **Veri:** Sahte (mock) veri veya kısıtlı bir veritabanı kopyası.
    * **Kural:** Kod sık sık bozulabilir. Diğer geliştiricileri etkilemez.

2.  **Entegrasyon (Integration / `int`)**
    * **Amaç:** Farklı geliştiricilerin yazdığı kodların (feature branch'ler) ana geliştirme dalına (örn. `develop`) birleştiği yerdir.
    * **Süreç:** Genellikle Sürekli Entegrasyon (CI) sunucusu tarafından otomatik olarak tetiklenir. Birim testleri (Unit Tests) ve temel entegrasyon testleri burada çalışır.
    * **Kural:** Build hatası (bozuk derleme) burada yakalanmalı ve anında düzeltilmelidir.

3.  **Test / Staging (Prova / `staging`)**
    * **Amaç:** Canlı (Production) ortamın *birebir kopyası* olan, kalite güvence (QA) ekibinin ve ürün sahiplerinin test yaptığı ortamdır.
    * **Veri:** Anomimleştirilmiş (temizlenmiş) canlı veri kopyası veya çok gerçekçi test verisi.
    * **Süreç:** Sadece CI'dan başarıyla geçen ve test edilmeye hazır "kararlı" sürümler buraya dağıtılır. Kullanıcı Kabul Testleri (UAT) burada yapılır. *Dağıtım senaryosunun kendisi* (örn. veritabanı migrasyonu) ilk burada test edilir.
    * **Kural:** Bu ortam her zaman "çalışır" durumda olmalıdır.

4.  **Üretim (Production / `prod`)**
    * **Amaç:** Son kullanıcının/müşterinin kullandığı "canlı" ortam.
    * **Veri:** Gerçek müşteri verisi.
    * **Kural:** Sıfır hata toleransı. En yüksek seviyede izleme (monitoring) ve güvenlik bu ortamda uygulanır. Değişiklikler sadece sıkı bir onay süreci ve planlı bir dağıtım penceresi ile yapılır.

### Yazılım Türlerine Göre Çevre Örnekleri

| Çevre | Web / API | Masaüstü (Desktop) | Mobil (Mobile) | Endüstriyel (PLC/HMI) |
| :--- | :--- | :--- | :--- | :--- |
| **Dev** | `localhost:3000` | Geliştiricinin VS'de derlediği `debug.exe` | Android/iOS Simülatörü | PLC Simülasyon Yazılımı (örn. PLCSIM) |
| **Test/Staging** | `staging.api.sirket.com` | QA ekibine dağıtılan özel `.msi` paketi | TestFlight / Google Play Internal Test | Fabrika dışındaki test tezgahı (Test Rig) |
| **Prod** | `api.sirket.com` | Otomatik güncelleme (auto-updater) sunucusu | App Store / Google Play Store | Fabrika sahasındaki gerçek makine |

---

## 2. 📦 Yayın (Release) Yönetimi

Yayın yönetimi, dağıtımın "ne, ne zaman ve nasıl" yapılacağını planlayan süreçtir.

### 2.1. Sürüm Numaralama (Versioning)

Her yayın paketinin benzersiz ve anlamlı bir etiketi olmalıdır. En yaygın standart **Semantik Sürümlemedir (Semantic Versioning - SemVer)**.

**Format: `MAJOR.MINOR.PATCH`** (Örn: `2.5.1`)

* **`MAJOR` (Ana Sürüm):** Geriye dönük uyumluluğu bozan (breaking change) API değişiklikleri yaptığınızda artırılır. (Örn: `1.x`'ten `2.0.0`'a geçiş).
* **`MINOR` (Ara Sürüm):** Geriye dönük uyumlu (backward-compatible) yeni özellikler (feature) eklediğinizde artırılır. (Örn: `2.4.0`'dan `2.5.0`'a geçiş).
* **`PATCH` (Yama Sürümü):** Geriye dönük uyumlu hata düzeltmeleri (bug fix) yaptığınızda artırılır. (Örn: `2.5.0`'dan `2.5.1`'e geçiş).

### 2.2. Değişiklik Günlüğü (Changelog)

**Zorunludur.** Her sürümle birlikte, o sürümde nelerin değiştiğini açıklayan bir `CHANGELOG.md` dosyası yayınlanmalıdır. Bu, hem teknik ekip (destek, QA) hem de son kullanıcılar için kritik öneme sahiptir.

**Örnek Format (Keep a Changelog):**

```markdown
## [1.2.0] - 2025-10-27

### Eklendi (Added)
- Kullanıcılar artık profillerine fotoğraf ekleyebilir.
- Raporlama modülüne 'PDF Dışa Aktar' özelliği eklendi.

### Değiştirildi (Changed)
- Ana sayfa yükleme performansı optimize edildi.

### Düzeltildi (Fixed)
- Belirli koşullarda sepet hesaplamasının yanlış yapılması hatası giderildi.

### Kaldırıldı (Removed)
- Eski ve kullanılmayan 'Legacy Rapor' sayfası sistemden kaldırıldı.
```