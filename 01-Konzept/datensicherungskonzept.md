# Datensicherungskonzept

## Ziele
- Schutz aller produktiven VMs und Kameraaufnahmen
- Nachweis von funktionierenden **Restore‑Prozessen** (VM + Datei)
- Dokumentation gemäss M143 (A1–E2)

## Tiering & RPO/RTO
- **Gold** (AD‑1, AD‑2): RPO ≤ 6h, RTO ≤ 2h
- **Silver** (nginx, netvps, pubvps): RPO 24h, RTO 6–8h
- **Bronze** (Game servers, test): RPO 24–72h, best‑effort RTO

## Strategie
- **Primary backup**: Proxmox Backup Server (PBS) VM on OptiPlex, datastore on 1TB HDD
- **GFS Schema (Baseline)**: **5 daily incrementals, 1 weekly full, 1 monthly full** (5‑1‑1)
- **Option (Expanded)**: 7‑4‑3 or 7‑4‑12 (depends on capacity; see storage estimates)
- **3‑2‑1**: 3 copies (prod + PBS + optional USB/cloud), 2 media types, 1 offsite (future)

## Speicherbedarf (Übersicht)
Raw VM sum: 568 GB

See `/tables/storage_estimates.csv` for scenarios and capacity fit.

## Risiken & Massnahmen
- **SMB1** required by Xiaomi cams → isolate share, least privilege, nightly copy to PBS/secondary
- Single PBS datastore disk → plan USB/offsite copy as improvement
