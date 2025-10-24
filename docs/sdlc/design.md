
## 4) `docs/sdlc/design.md`
```markdown
# Tasarım & Mimariler

## Çıktılar
- Yüksek seviye mimari diyagram (Context/Container/Component)
- Veri modeli (ERD)
- API tasarımı (OpenAPI/Swagger)
- Performans & güvenlik kararları

=== "Context"
    ```mermaid
    graph TD
      User((Kullanıcı)) --> App[Uygulama]
      App --> DB[(Veritabanı)]
      App --> Ext[Harici Servisler]
    ```
=== "Container"
    ```mermaid
    graph TD
      Web[Web UI] --> API[Backend API]
      API --> DB[(PostgreSQL)]
      API --> Cache[(Redis)]
    ```

## Mimarî İlkeler
- Gevşek bağlılık, yüksek uyum
- Temiz katmanlar (UI / Uygulama / Domain / Infra)
- SLA/SLO/SLI tanımları

## DOD
- [ ] Diyagramlar versiyonlandı (`assets/diagrams`)
- [ ] API sözleşmeleri yazıldı
- [ ] Güvenlik (authz/authn) ve performans planı
