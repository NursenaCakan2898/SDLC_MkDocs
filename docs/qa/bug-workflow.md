# 🐞 Kapsamlı Hata (Bug) Yönetimi ve Yaşam Döngüsü Kılavuzu

---

## 1. Giriş ve Amaç

Hata (Bug) Yönetimi, bir yazılım ürünündeki kusurların (defect) sistematik olarak bulunması, raporlanması, sınıflandırılması, atanması, düzeltilmesi ve doğrulanması sürecidir.

Bu yaşam döngüsünün amacı **kaosu önlemektir**. Net bir iş akışı olmadan, bulunan hatalar kaybolabilir, öncelikleri anlaşılamaz, kimin düzelteceği belli olmaz ve en önemlisi, düzeltilip düzeltilmediği doğrulanamaz.

Bu kılavuz, bir hatanın doğumundan ölümüne kadar olan standart yaşam döngüsünü (Defect Life Cycle) ve bu süreçteki en iyi uygulamaları tanımlar.

---

## 2. Kapsamlı Hata (Bug) Yaşam Döngüsü

Sağladığınız temel akış, bir hatanın ideal yolculuğunu gösterir. Ancak gerçek dünyada bu akış, hataların reddedilmesi veya yeniden açılması gibi ek adımları da içerir.

Aşağıdaki şema, bu **kapsamlı** ve **gerçekçi** iş akışını göstermektedir:

```mermaid
flowchart TD
    A[Hata Bulundu] --> B(Triage Tasnif);

    B --> C{Gercek Bir Hata mi?};
    C -- Hayir --> D[Rejected Reddedildi];
    C -- Evet --> E{Daha Once Raporlanmis mi?};
    E -- Evet --> F[Duplicate Yinelenen];
    E -- Hayir --> G{Bu Surumde Duzeltilecek mi?};
    G -- Hayir --> H[Deferred Ertelendi Backlog];
    G -- Evet --> I[Assigned Atandi];

    I --> J[In Progress Duzeltiliyor];
    J --> K[Fixed Duzeltildi];
    K --> L[Ready for QA Dogrulama Bekliyor];
    L --> M(In Verification Dogrulamada);

    M --> N{Hata Giderilmis mi?};
    N -- Evet --> O[Closed Kapatildi];
    N -- Hayir --> P[Re-Opened Yeniden Acildi];
    P --> I;

    classDef state fill:#f9f9f9,stroke:#333,stroke-width:2px;
    classDef decision fill:#ECECFF,stroke:#333,stroke-width:2px;
    classDef endstate fill:#FFECEC,stroke:#A30000,stroke-width:2px;

    class A,B,I,J,K,L,M,P state;
    class C,E,G,N decision;
    class D,F,H,O endstate;
```    