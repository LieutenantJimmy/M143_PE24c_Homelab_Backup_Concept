# Backup Concept

## Objectives
- Protect **all productive VMs** and **camera recordings**
- Prove **working restore procedures** (full VM + single file)
- Document according to TBZ M143 (competency bands A1–E2)

## Tiers & Targets

| VM/Service                                                   | Priority Tier | Reason / Role                             | RPO (Max Data Loss) | RTO (Max Downtime) |
| ------------------------------------------------------------ | ------------- | ----------------------------------------- | ------------------- | ------------------ |
| **AD-1**                                                     | **Critical**  | Primary Active Directory / DNS root       | 6h                  | 1–2h               |
| **netvps-1**                                                 | **Critical**  | Pi-hole, DNS forwarder, internal services | 6h                  | 1–2h               |
| **AD-2**                                                     | Medium        | Secondary AD (consistency with AD-1)      | 6h                  | 48h                |
| **netvps-2**                                                 | Medium        | Secondary DNS forwarder (consistency)     | 6h                  | 48h                |
| **Game servers** (SCP, BeamMP, Zomboid, Minecraft, Unturned) | High          | Must be available for players             | 24h                 | 1–2h               |
| **Public proxy (nginx-ext)**                                 | High          | External access / entry point             | 24h                 | 24h                |
| **Public VPS (pubvps-1)**                                    | High          | Hosts public apps (RxResume, Ollama)      | 24h                 | 24h                |
| **Internal proxy (nginx-int)**                               | Low           | Only used internally, non-critical        | 24h                 | 48h                |
| **Ubuntu test VM**                                           | Low           | School sandbox / dev                      | 24h                 | 48h                |


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
