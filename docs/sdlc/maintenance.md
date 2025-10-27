# 🔧 Kapsamlı Yazılım Bakım ve Operasyon Kılavuzu

Yazılım Bakım ve Operasyon (M&O), bir yazılımın canlı (production) ortamda çalışır, sağlıklı, performanslı ve güvenli kalmasını sağlamak için yürütülen tüm proaktif (önleyici) ve reaktif (düzeltici) faaliyetler bütünüdür.

Bu süreç, "dağıtım" (deployment) tamamlandıktan sonra başlar ve yazılımın ömrü boyunca devam eder.

---

## 1. 🔭 Gözlemlenebilirlik (Observability): İzleme & Alarm

Gözlemlenebilirlik, sistemin iç durumunu, dışarıya verdiği çıktılara (metrikler, loglar, trace'ler) bakarak anlama yeteneğidir. "Sistem neden yavaş?" veya "Neden hata alıyoruz?" sorularını yanıtlamamızı sağlar.

### 1.1. Metrikler (Metrics) - "Ne oluyor?"

Metrikler, belirli bir zaman aralığındaki sayısal ölçümlerdir. Sistemin genel sağlığını (nabzını, tansiyonunu) gösterirler.

* **Sistem Metrikleri (Altyapı):**
    * `CPU Kullanımı (%)`: Sunucunun işlemci yükü.
    * `RAM Kullanımı (GB/%)`: Bellek tüketimi.
    * `Disk G/Ç (IOPS)` ve `Disk Kullanımı (%)`: Disk okuma/yazma hızı ve doluluk.
    * `Ağ Trafiği (GB/s)`: İçeri/dışarı giden veri.
* **Uygulama Performans Metrikleri (APM):**
    * `Gecikme (Latency)`: Bir isteğin ne kadar sürede yanıtlandığı (örn. p95, p99 - %95'inin ne kadar sürdüğü).
    * `Trafik (Throughput)`: Saniyedeki istek sayısı (RPS - Request Per Second).
    * `Hata Oranı (%)`: Başarısız olan isteklerin (HTTP 5xx, 4xx) toplam isteklere oranı.
* **İş Metrikleri (Business Metrics):**
    * `Dakikadaki Satış Sayısı`
    * `Aktif Kullanıcı Sayısı (DAU/MAU)`
    * `Sepete Ekleme Başarı Oranı`
* **Yazılım Türüne Göre Örnek Metrikler:**
    * **Web/API:** HTTP 5xx hata sayısı, API yanıt süresi (p99).
    * **Masaüstü:** Uygulama başlangıç süresi (saniye), yaşanan "Donma" (Not Responding) sayısı.
    * **Endüstriyel (SCADA/HMI):** PLC ile bağlantı kesilme (disconnect) sayısı, Modbus sorgu gecikmesi (ms).
    * **Mobil:** Uygulama çökme (crash) oranı, API isteklerinin ağdaki gecikmesi.

### 1.2. Loglama (Logging) - "Neden oluyor?"

Loglar, sistemde meydana gelen *spesifik olayların* zaman damgalı metin kayıtlarıdır. Bir hata oluştuğunda "tam o anda" ne olduğunu anlamak için kullanılırlar.

* **Log Seviyeleri (Önem sırasına göre):**
    * `DEBUG`: Geliştirme aşamasında, kodun her adımını görmek için (Canlıda kapalı olmalı).
    * `INFO`: Normal operasyonel bilgiler (örn. "Kullanıcı 123 giriş yaptı", "Servis başlatıldı").
    * `WARN` (Uyarı): Beklenmedik, ancak sistemi durdurmayan durumlar (örn. "Config dosyası bulunamadı, varsayılan kullanıldı", "API yanıtı yavaşladı").
    * `ERROR`: Sistemin o anki işlemi tamamlayamadığı ciddi hatalar (örn. "Veritabanına yazılamadı", "Null pointer exception").
    * `FATAL`/`CRITICAL`: Sistemin çalışmasını tamamen durduran, çökme (crash) yaratan olaylar.
* **En İyi Pratik: Yapısal Loglama (Structured Logging)**
    * Logları düz metin yerine `JSON` formatında yazmaktır. Bu, logları sorgulamayı (query) ve filtrelemeyi (filter) çok kolaylaştırır.
    * *Kötü Log:* `Hata: Kullanıcı 123 sipariş veremedi.`
    * *İyi Log (JSON):* `{"level": "ERROR", "timestamp": "...", "user_id": 123, "action": "create_order", "error": "Insufficient stock for product 456"}`

### 1.3. İzleme (Distributed Tracing) - "Nerede oluyor?"

Özellikle mikroservis mimarilerinde, bir isteğin sistemler arasındaki *yolculuğunu* takip etmektir. "Yavaşlığın" hangi servisten kaynaklandığını bulmayı sağlar.

* **Örnek:** Bir "Satın Al" butonu tıklandığında:
    1.  `Request-ID: abc-123` -> `API Gateway` (20ms)
    2.  `Request-ID: abc-123` -> `Sipariş Servisi` (50ms)
    3.  `Request-ID: abc-123` -> `Stok Servisi` (300ms) **(Sorun burada!)**
    4.  `Request-ID: abc-123` -> `Ödeme Servisi` (150ms)
    * **Toplam Süre: 520ms.** Trace sayesinde yavaşlığın `Stok Servisi`'nden kaynaklandığı anında görülür.

### 1.4. Alarm (Alerting)

İzleme, alarm üretmiyorsa (sadece dashboard'da duruyorsa) etkisizdir. Alarm, metrikler belirli bir eşiği (threshold) aştığında ilgili ekibe haber veren mekanizmadır.

* **Kural:** Alarmlar *eylem alınabilir (actionable)* olmalıdır.
    * *Kötü Alarm:* "CPU %80 oldu." (Ee, ne yapalım? Belki normaldir.)
    * *İyi Alarm:* "CPU 5 dakikadır %95'in üzerinde ve P99 API gecikmesi 3 saniyeyi aştı." (Bu, kullanıcıların yavaşlık hissettiği anlamına gelir ve müdahale gerektirir).
* **Kanallar:** PagerDuty, Opsgenie, E-posta, Slack, SMS.
* **Risk: Alarm Yorgunluğu (Alert Fatigue):** Çok fazla gereksiz alarm (noise) gelirse, ekipler gerçek ve önemli alarmları kaçırmaya başlar.

---

## 2. 🚨 Olay Yönetimi (Incident Management)

Olay (Incident), hizmetin kesintiye uğraması veya kalitesinin kabul edilemez seviyede düşmesidir. Olay yönetimi, bu kesintiyi *en hızlı şekilde* çözme ve normale dönme sürecidir.

**Anahtar Metrik: MTTR (Mean Time to Resolution) - Ortalama Çözüm Süresi.** Hedef, MTTR'ı sürekli düşürmektir.

### 2.1. Sınıflandırma (Triage & Severity)

Bir alarm geldiğinde veya bir sorun bildirildiğinde, ilk olarak ciddiyet seviyesi belirlenir.

* **SEV-1 (Kritik):** Sistem tamamen durmuş (çökmüş), veri kaybı var, güvenlik açığı oluşmuş veya tüm kullanıcılar etkilenmiş.
    * *Müdahale:* 7/24, *ANINDA*. Tüm ilgili ekipler (savaş odası) toplanır.
* **SEV-2 (Yüksek):** Sistemin ana bir özelliği (örn. ödeme) çalışmıyor, ancak alternatif bir yol (workaround) var veya kullanıcıların büyük bir kısmı etkilenmiş.
    * *Müdahale:* Mesai saatleri içinde *ANINDA*, mesai dışında "nöbetçi" (on-call) ekip tarafından.
* **SEV-3 (Orta):** Sistemin ikincil bir özelliği (örn. rapor export) çalışmıyor veya ana bir özellik yavaş çalışıyor.
    * *Müdahale:* Planlı (hotfix) veya bir sonraki sprint'te ele alınır. Acil müdahale gerektirmez.
* **SEV-4 (Düşük):** Kozmetik bir sorun, yazım hatası (typo) veya performansa etkisi olmayan küçük bir hata.
    * *Müdahale:* Normal iş akışında (backlog) planlanır.

### 2.2. İletişim (Communication)

Olay yönetiminde teknik çözüm kadar iletişim de kritiktir.

* **İç İletişim:**
    * Bir "Savaş Odası" (War Room) kurulur (örn. `#incident-20251027` Slack kanalı).
    * Bir `Incident Commander` (Olay Yöneticisi) atanır. Bu kişi teknik çözüm yapmak zorunda değildir, *süreci yönetir* (Kim neye bakıyor? Durum nedir?).
    * Diğer ekiplere (Destek, Yönetim) düzenli olarak (örn. 30 dakikada bir) durum güncellemesi geçilir.
* **Dış İletişim (Kullanıcı/Müşteri):**
    * **`status.sirket.com`** gibi bir "Durum Sayfası" üzerinden şeffaf bilgilendirme yapılır.
    * 1. "Sorunun farkındayız, araştırıyoruz."
    * 2. "Sorunu teşhis ettik, geçici çözüm üzerinde çalışıyoruz."
    * 3. "Geçici çözüm uygulandı, sistemi izliyoruz."
    * 4. "Sorun tamamen giderildi. Detaylı analiz (postmortem) yayınlanacaktır."

### 2.3. Çözüm (Resolution)

* **Geçici Çözüm (Workaround/Mitigation):** İlk hedef, sistemi *en hızlı* şekilde ayağa kaldırmaktır (MTTR). Bu, sorunu kalıcı olarak çözmese de kesintiyi bitirir.
    * *Örnekler:* Hatalı servisi yeniden başlatmak (restart), hatalı sürümü geri almak (rollback), trafiği başka bir bölgeye yönlendirmek.
* **Kalıcı Çözüm (Permanent Fix):** Kesinti bittikten sonra, sorunun *kök nedenini* ortadan kaldıran kalıcı düzeltme (örn. kod değişikliği, veritabanı optimizasyonu) planlanır ve uygulanır.

### 2.4. Postmortem (Olay İncelemesi)

Olay çözüldükten sonra, *bir daha yaşanmaması için* yapılan analiz toplantısı ve raporudur.

* **Kültür: Suçlama Olmayan (Blameless) Postmortem**
    * *Yanlış:* "Ahmet, yanlış konfigürasyonu canlıya çıktı."
    * *Doğru:* "Sistemimiz, bir geliştiricinin yanlış konfigürasyonu canlıya çıkmasını (testlerde veya CI/CD'de) neden engelleyemedi?"
* **Rapor İçeriği:**
    * `Özet`: Ne oldu?
    * `Etki`: Müşteriler nasıl etkilendi? Ne kadar sürdü (MTTR)? Ne kadar gelir/itibar kaybı oldu?
    * `Zaman Çizelgesi (Timeline)`: Olayın saniye saniye dökümü.
    * `Kök Neden Analizi (RCA)`: Sorunun *asıl* kaynağı neydi?
    * `Eylem Maddeleri (Action Items)`: Bir daha olmaması için yapılacak *somut* işler (Örn: "Ödeme servisine timeout alarmı eklenecek", "Deployment script'ine ekstra kontrol adımı eklenecek").

---

## 3. 📈 Sürekli İyileştirme (Continuous Improvement)

Operasyon, sadece yangın söndürmek (reaktif) değil, aynı zamanda yangın çıkmasını engellemektir (proaktif).

### 3.1. Kök Neden Analizi (Root Cause Analysis - RCA)

Bir sorunun *görünen* semptomu yerine *gerçek* kaynağını bulma tekniğidir.

* **Yöntem: 5 Neden (5 Whys)**
    * 1. *Problem:* Müşteriler "Sipariş Ver" butonuna basınca hata aldı.
    * 2. *Neden?* `Sipariş Servisi` API'si `HTTP 500` döndü.
    * 3. *Neden?* Servis, veritabanına bağlanamadı.
    * 4. *Neden?* Veritabanının bağlantı limiti (connection pool) dolmuştu.
    * 5. *Neden?* `Rapor Servisi`'ndeki yeni bir özellik, veritabanına çok fazla sorgu atarak tüm bağlantıları tüketmişti.
    * **Kök Neden:** `Rapor Servisi`'nin kontrolsüz sorgu atması. (Sadece `Sipariş Servisi`'ni restart etmek sorunu kalıcı çözmezdi).

### 3.2. Proaktif Bakım

* **Teknik Borç (Technical Debt) Yönetimi:** Kod kalitesini artırmak, eski kütüphaneleri güncellemek, yavaş sorguları optimize etmek için *planlı* zaman ayırmak.
* **Planlı Bakımlar:** Sertifika yenilemeleri, işletim sistemi güncellemeleri, veritabanı bakımları (indexing vb.).
* **Kaos Mühendisliği (Chaos Engineering):** (İleri seviye) Canlı sisteme *kasıtlı* olarak küçük arızalar (örn. bir sunucuyu kapatmak, ağı yavaşlatmak) vererek sistemin bunlara karşı dayanıklılığını (resilience) test etmek.

### 3.3. Hedef Takibi (SLI/SLO ve KPI/OKR)

Performansın "iyi" olup olmadığını bilmek için hedefler belirlenmelidir.

* **SLI (Service Level Indicator):** Ölçülebilir bir metrik (Örn: `Hata Oranı`).
* **SLO (Service Level Objective):** Bu metriğin *iç hedefi* (Örn: `Hata Oranı < %0.1`). Alarmlar bu hedefe göre kurulur.
* **SLA (Service Level Agreement):** Müşteriye verilen *söz* (Örn: `%99.9 Uptime`). Yasal ve finansal bağlayıcılığı vardır.
* **KPI (Key Performance Indicator):** İş hedefleri (Örn: `MTTR < 15 dakika`).

---

## 4. ✅ Operasyonel "Bitti" Tanımı (Definition of Done - DoD)

Bu liste, operasyonel süreçlerin sağlıklı işlediğini gösteren periyodik (örn. haftalık/aylık) bir kontrol listesidir.

* [ ] **Olay Yönetimi:**
    * [ ] Tüm `SEV-1` ve `SEV-2` olayları çözüldü (Resolved) ve kapatıldı (Closed).
    * [ ] Tüm `SEV-1` ve `SEV-2` olayları için "Suçlama Olmayan Postmortem" raporu yazıldı ve ilgili ekiplerle paylaşıldı.
* [ ] **İyileştirme:**
    * [ ] Önceki Postmortem'lerden çıkan *tüm* "Eylem Maddeleri" (Action Items) oluşturuldu, atandı ve iş takip sistemine (Jira, Trello vb.) eklendi.
    * [ ] Kritik eylem maddeleri (örn. güvenlik açığı, alarm eksikliği) tamamlandı.
* [ ] **Performans:**
    * [ ] **Performans metrikleri hedeflerde:** Tüm kritik servislerin SLO (Service Level Objective) hedefleri (örn. %99.9 Uptime, p99 Latency < 500ms) tutturuldu.
    * [ ] Hedeflerin altında kalan servisler için iyileştirme planı oluşturuldu.
* [ ] **Gözlemlenebilirlik:**
    * [ ] İzleme (monitoring) panoları (dashboard) incelendi, "bilinen" veya "beklenen" hatalar (örn. `WARN` seviyesi) dışında anormal bir durum gözlemlenmedi.
    * [ ] Bilinmeyen ancak `ERROR` seviyesindeki tüm loglar incelendi ve bunlar için görev (task) veya olay (incident) kaydı açıldı.
* [ ] **Proaktif Bakım:**
    * [ ] (Periyoda bağlı olarak) Planlı bakımlar (kütüphane güncellemesi, sertifika yenileme, veritabanı bakımı) tamamlandı veya bir sonraki bakım penceresi için planlandı.