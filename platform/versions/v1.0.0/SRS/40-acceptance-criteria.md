> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# SRS Acceptance Criteria

TitipO mengelola order harian rumah tangga ↔ vendor terpercaya dengan fee 12% dan repeat-order ≥40%.

## QA Gate

- QA-001: Semua acceptance criteria menjadi test case; test case harus auto-executable.
- QA-002: Build gagal jika unit/integration test merah; tidak boleh skip test untuk release.
- QA-003: Regresi wajib punya test sebelum fix; test harus green sebelum merge ke main.
- QA-004: Coverage metrics harus tracked per sprint; target >80% untuk code paths kritis.

## Test Case Examples

- TC-001: [Given scenario] When [action] Then [expected result].
- TC-002: Error handling test; verify error code + message sesuai SRS.

## Batasan

Acceptance criteria hanya test behavior external; internal refactor tidak perlu test baru jika behavior tetap.