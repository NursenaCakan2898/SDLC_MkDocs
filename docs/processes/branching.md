# 🌳 Kapsamlı Branching (Dallanma) Stratejisi Kılavuzu

## 1. Branching (Dallanma) Stratejisi Nedir ve Neden Hayatidir?

**Branching Stratejisi**, bir sürüm kontrol sisteminde (Git gibi) kodun nasıl yönetileceğine dair bir kurallar bütünüdür. Birden fazla geliştiricinin aynı kod tabanı üzerinde *aynı anda*, *birbirini ezmeden* ve *sistemin kararlılığını bozmadan* çalışabilmesini sağlayan bir trafik yönetim sistemidir.

**Neden Hayatidir?**

* **İzolasyon (Isolation):** Her yeni özellik veya hata düzeltmesi, ana kod tabanından izole bir "dalda" (branch) geliştirilir. Bu sayede, yarım kalan veya hatalı bir kod, çalışan ana sistemi (`main`) asla etkilemez.
* **Paralel Geliştirme (Parallel Development):** Eş zamanlı olarak birden fazla `feature` (özellik) ve `hotfix` (acil düzeltme) üzerinde çalışılabilir.
* **Kararlılık (Stability):** Üretim (production) kodunu (`main`) ve bir sonraki sürüm kodunu (`develop`) her zaman kararlı ve çalışır durumda tutar.
* **İzlenebilirlik (Traceability):** Her değişikliğin (commit), bir özellik (feature) veya hata (bug) ile net bir şekilde ilişkilendirilmesini sağlar. Bu, geriye dönük inceleme (auditing) ve hata ayıklama (debugging) için paha biçilmezdir.

Bu kılavuzda, "GitFlow" olarak bilinen, endüstri standardı haline gelmiş ve karmaşık projeler için en uygun olan modeli detaylandıracağız.

## 2. Ana Dallanma (Branch) Akışı (GitFlow Modeli)

Aşağıdaki diyagram, bu stratejideki tüm ana dalları ve aralarındaki ilişkiyi (kod akışını) göstermektedir.

```mermaid
flowchart TD
    subgraph Legend [Lejant]
        direction LR
        L1[main] --- L2[develop]
        L2 --- L3[feature]
        L2 --- L4[release]
        L1 --- L5[hotfix]
    end

    subgraph Production [Uretim Production]
        M1[v1.0] -- Tag --> M2[v1.0.1] -- Tag --> M3[v1.1] -- Tag --> M4[...]
        style M1 fill:#f9d,stroke:#333,stroke-width:2px
        style M2 fill:#f9d,stroke:#333,stroke-width:2px
        style M3 fill:#f9d,stroke:#333,stroke-width:2px
        style M4 fill:#f9d,stroke:#333,stroke-width:2px
    end
    
    subgraph Integration [Entegrasyon Develop]
        D1[commit] --> D2[commit] --> D3[commit] --> D4[commit] --> D5[...]
        style D1 fill:#d9f,stroke:#333,stroke-width:2px
        style D2 fill:#d9f,stroke:#333,stroke-width:2px
        style D3 fill:#d9f,stroke:#333,stroke-width:2px
        style D4 fill:#d9f,stroke:#333,stroke-width:2px
        style D5 fill:#d9f,stroke:#333,stroke-width:2px
    end

    subgraph Feature_A [Ozellik Gelistirme feature-user-login]
        F1[commit] --> F2[commit]
        style F1 fill:#dfd,stroke:#333,stroke-width:2px
        style F2 fill:#dfd,stroke:#333,stroke-width:2px
    end

    subgraph Release [Surum Hazirligi release-v1.1]
        R1[commit] --> R2[Sadece bug fix]
        style R1 fill:#ffc,stroke:#333,stroke-width:2px
        style R2 fill:#ffc,stroke:#333,stroke-width:2px
    end

    subgraph Hotfix [Acil Duzeltme hotfix-login-bug]
        H1[commit]
        style H1 fill:#fdd,stroke:#333,stroke-width:2px
    end
    
    D1 --> F1
    F2 -- PR Pull Request --> D2
    
    D3 --> R1
    R2 -- Merge --> M3
    R2 -- Merge --> D4
    
    M1 --> H1
    H1 -- Merge --> M2
    H1 -- Merge --> D3

    linkStyle 0 stroke:#aaa,stroke-width:1px
    linkStyle 1 stroke:#aaa,stroke-width:1px
    linkStyle 2 stroke:#aaa,stroke-width:1px
    linkStyle 3 stroke:#aaa,stroke-width:1px
    linkStyle 4 stroke:#aaa,stroke-width:1px

```    