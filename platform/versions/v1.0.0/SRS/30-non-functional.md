> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# SRS Non-Functional Requirements

TitipO mengelola order harian rumah tangga ↔ vendor terpercaya dengan fee 12% dan repeat-order ≥40%.

## Non-Functional Requirements

- Performance: Operasi utama harus punya batas waktu terukur (misal <2 dtk untuk API, <100ms untuk UI).
- Security: JWT audien per service dan least privilege; API key perangkat dengan rotasi.
- Reliability: Rollback terdokumentasi; offline-first untuk lapangan.
- Scalability: Database per service; connection pool tuned per beban.
- Usability: Flow intuitif sesuai user story dari PRD.

## Batasan

Non-functional requirement harus diukur dan di-verify QA sebelum release.