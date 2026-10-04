> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# SRS Glossary

TitipO mengelola order harian rumah tangga ↔ vendor terpercaya dengan fee 12% dan repeat-order ≥40%.

## Terminology

- **TDD (Test-Driven Development):** Requirement → Red Test → Green Code → Refactor.
- **Acceptance Criteria:** Kondisi konkret yang harus dipenuhi feature untuk diterima QA.
- **QA Gate:** Barrier otomatis (test, coverage, review) sebelum merge/release.
- **DB per Service:** Database logis terpisah per divisi; satu cluster Postgres untuk pilot.
- **JWT Audiens:** Token JWT memiliki audiens ('aud') per service untuk validasi cross-service.

## Batasan

Glossary hanya referensi; definisi formal ada di spec teknis masing-masing.