# SDLC Genel Bakış

Aşağıdaki akış, uçtan uca yazılım yaşam döngüsünü özetler:

```mermaid
flowchart LR
  A[Gereksinimler] --> B[Tasarım & Mimariler]
  B --> C[Geliştirme]
  C --> D[Test & Doğrulama]
  D --> E[Yayın]
  E --> F[Bakım & Operasyon]
  F -.-> A
