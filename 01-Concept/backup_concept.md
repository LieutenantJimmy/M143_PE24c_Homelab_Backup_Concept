# Backup Concept

## Objectives
Ensure service continuity

1. **Ensure service continuity**

   * Maintain availability of critical infrastructure (AD, DNS, core VPS) even in case of VM corruption, accidental deletion, or host failure.

2. **Protect data consistency**

   * Back up **paired services** (AD-1/AD-2, netvps-1/2) with identical RPO to avoid replication or directory drift.

3. **Minimize data loss**

   * Define Recovery Point Objectives (RPO) per tier and configure backup frequency to meet them.

4. **Enable rapid recovery**

   * Define Recovery Time Objectives (RTO) per tier and validate through restore testing that VMs/services can be recovered within target timeframes.

5. **Implement proven backup methodology**

   * Use **GFS (Grandfather-Father-Son)** retention for efficient restore points.
   * Target a **3-2-1 strategy** (onsite + offsite) to mitigate risks from hardware failure, human error, or disaster.

6. **Cover all workloads**

   * Back up **Proxmox VMs**, **Hyper-V VMs**, and **unstructured data** (camera recordings).

7. **Secure and compliant backups**

   * Ensure backups are encrypted in transit and at rest where applicable.
   * Isolate SMB1 shares used by cameras and limit permissions.

8. **Validate with restore tests**

   * Perform and document full VM restores, file-level restores, and unstructured data restores to prove the concept.

9. **Plan for capacity and growth**

   * Estimate required storage capacity with different GFS scenarios and verify feasibility within 1 TB PBS datastore.

10. **Provide documentation and runbooks**

    * Create clear, reproducible guides for setup, restore, and maintenance in alignment with TBZ M143 competency matrix.

---

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
