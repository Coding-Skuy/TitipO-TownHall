> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# Platform ~ Aplikasi (TitipO)

Folder ini adalah **indeks aplikasi** divisi TitipO: tujuh repo kode yang hidup di
organisasi Coding-Skuy ([tim TitipO](https://github.com/orgs/Coding-Skuy/teams/titipo/repositories)),
ditautkan ke repo ini sebagai **git submodule**.

- `platform/versions/` — dokumen formal beku (BRD/PRD/FRD/FSD/SRS).
- `platform/apps/` — kode aplikasi (submodule, repo terpisah).

## Repo aplikasi

| Submodule | Repo | Peran | Stack |
|---|---|---|---|
| `app-kmp/` | [titipo-app-kmp](https://github.com/Coding-Skuy/titipo-app-kmp) | Satu codebase, dua peran: vendor + rumah tangga (Navigation3) | Kotlin MPP / CMP |
| `web/` | [titipo-web](https://github.com/Coding-Skuy/titipo-web) | Web admin | Bun + SvelteKit + TS |
| `backend-service/` | [titipo-backend-service](https://github.com/Coding-Skuy/titipo-backend-service) | Layanan API; facade ke Lumbung | Rust |
| `ai-models/` | [titipo-ai-models](https://github.com/Coding-Skuy/titipo-ai-models) | Stub rekomendasi vendor | — |
| `data-pipeline/` | [titipo-data-pipeline](https://github.com/Coding-Skuy/titipo-data-pipeline) | Event pesanan ke Titeny | — |
| `design/` | [titipo-design](https://github.com/Coding-Skuy/titipo-design) | Tokens, flows, screens titip harian | Figma |
| `infra-devops/` | [titipo-infra-devops](https://github.com/Coding-Skuy/titipo-infra-devops) | CI build KMP/Rust/Web, deploy prod | CI/CD |

## Cara pakai

```bash
# clone beserta submodule
git clone --recurse-submodules https://github.com/Coding-Skuy/TitipO-TownHall.git
# bila repo sudah ter-clone tanpa submodule:
git submodule update --init --recursive
# perbarui ke commit main terbaru tiap repo app:
git submodule update --remote
```

## Batasan

Indeks ini hanya menautkan repo. Kode dan riwayatnya tetap milik repo app masing-masing;
keputusan versi di tulis di `platform/versions/vX.Y.Z/`, bukan di sini.