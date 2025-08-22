# M143 Backup & Restore – Homelab Project

<img width="991" height="1008" alt="image" src="https://github.com/user-attachments/assets/79feadd1-96b4-48ce-beda-e2848448b386" />

**Use Case (real-world background)**  
This project uses an existing homelab that is already in **production use**:
- A **Proxmox cluster** on two Mac mini servers (“Macmi‑1/2”) runs core infrastructure: **Active Directory**, DNS forwarders, internal/public reverse proxies, and internal/public VPS services.
- A **Windows Server 2025 (OptiPlex)** host runs multiple **Hyper‑V** game servers and will also host a **Proxmox Backup Server (PBS)** VM with a **dedicated 1 TB HDD** for backup storage.
- Edge devices include a **Raspberry Pi** (HomeAssistant) and **Xiaomi 2K Pro** cameras (which can write recordings to a Windows **SMB1** share).

**Current situation / risk**  
These services are already **in operation without any reliable backups**. A hardware failure, human error, or malware could cause prolonged downtime or data loss (e.g., losing AD, DNS, configuration, or game saves). The goal of this project is to design and implement a **real, working backup & restore system** and to document **tested restore procedures** to meet the TBZ M143 competency matrix.

**Project goal**  
Design, implement, and document a **complete backup & restore** solution for:
- **All Proxmox VMs** (Macmi‑1/2) via **Proxmox Backup Server (PBS)** running on the OptiPlex
- **Hyper‑V VMs** (game servers) exported/snapshotted on the OptiPlex and stored on the same backup disk
- **Camera recordings** written to a Windows **SMB1** share and mirrored nightly

The solution will use a **GFS (Grandfather–Father–Son)** retention policy, target a **3‑2‑1** strategy, and include **restore proof** (full VM restore, single-file restore, and camera‑clip restore).

---

## Environment
- **Macmi‑1** (Proxmox 8): AD‑1, netvps‑1, pubvps‑1 (OS+data), nginx‑ext, nginx‑int  
- **Macmi‑2** (Proxmox 8): AD‑2, netvps‑2 (redundant core services)  
- **OptiPlex** (Windows Server 2025 + Hyper‑V): game server VMs, **PBS VM** (backup target on **1 TB HDD**)  
- **Edge:** Raspberry Pi (HomeAssistant), Xiaomi 2K Pro cameras (SMB1 recording share)

## Deliverables (aligned with TBZ M143)
- **Concept:** GFS 5‑1‑1 baseline, 3‑2‑1, RPO/RTO per tier, capacity planning
- **Implementation:** PBS install + datastore, Proxmox backup jobs, Hyper‑V VM exports/agent‑based backups, camera share + nightly mirror
- **Tests:** full VM restore, file‑level restore, camera‑clip restore (with screenshots and timings)
- **Documentation:** runbooks, configuration snippets, topology/backup diagrams, lessons learned

See `/01-Concept`, `/02-Implementation`, `/03-Tests`, `/04-Reflection`.
