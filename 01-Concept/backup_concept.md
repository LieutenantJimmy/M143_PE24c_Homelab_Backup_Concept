# Backup Concept

## Objectives
- Protect **all productive VMs** and **camera recordings**
- Prove **working restore procedures** (full VM + single file)
- Document according to TBZ M143 (competency bands A1–E2)

## Tiers & Targets
- **Gold** (AD‑1, AD‑2): RPO ≤ 6 h, RTO ≤ 2 h
- **Silver** (nginx, netvps, pubvps): RPO 24 h, RTO 6–8 h
- **Bronze** (game servers, test): RPO 24–72 h, best‑effort RTO

## Strategy
- **Primary backup**: **Proxmox Backup Server (PBS)** VM on OptiPlex; datastore on the **1 TB HDD**
- **Retention (baseline)**: **GFS 5‑1‑1** → 5 daily incrementals, 1 weekly full, 1 monthly full
- **Alternative profiles**: 7‑4‑3 or 7‑4‑12 (depending on capacity)
- **3‑2‑1**: 3 copies (production + PBS + optional USB/cloud), 2 media types, 1 offsite (future extension)

## Capacity Overview
- Raw VM total: see `/tables/vm_inventory.csv`
- Scenario estimates: see `/tables/storage_estimates.csv`

## Risks & Mitigations
- **SMB1** required by Xiaomi cameras → isolate the share, least‑privilege user, mirror nightly to backup disk
- Single‑disk PBS datastore → consider offsite/USB copy and (later) immutability
