# M143 Backup & Restore – Homelab Project

**Use-Case:** Proxmox cluster (Macmi-1/2) + Hyper‑V (OptiPlex) with PBS on OptiPlex.

**Goal:** Implement and document a complete backup & restore system (VMs + camera data) compliant with TBZ M143 competency matrix.

## Environment
- **Macmi-1** (Proxmox 8): AD-1, netvps-1, pubvps-1 (OS+data), nginx-ext, nginx-int
- **Macmi-2** (Proxmox 8): AD-2, netvps-2 (redundant core services)
- **OptiPlex** (Windows Server 2025 + Hyper‑V): game server VMs, PBS VM (backup target on 1TB HDD)
- **Edge:** Raspberry Pi (HomeAssistant), Xiaomi 2K Pro cameras (SMB1 recording share)

## Deliverables
- **Konzept**: strategy (GFS 5‑1‑1 baseline), 3‑2‑1, RPO/RTO per tier
- **Implementierung**: PBS install + datastore, Proxmox jobs, Hyper‑V VM backup, camera share backup
- **Tests**: VM full restore, single file restore, camera clip restore
- **Dokumentation**: runbooks, screenshots, lessons learned

See `/01-Konzept`, `/02-Implementierung`, `/03-Tests`, `/04-Reflexion`.
