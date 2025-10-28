![Title](image-4.png)

# Backup Restore Process

![Return to Readme File](README.md)

## Overview

This document outlines the restore procedures for both Proxmox (PBS) and Hyper-V (Veeam) environments within our homelab backup infrastructure. All restore procedures have been thoroughly tested with successful results for both full VM restoration and file-level recovery.

## Proxmox Backup Server (PBS) Restore Procedures

### Full VM Restore

**Tested Environment**: Proxmox VE 8.4.13 → PBS on OptiPlex  
**Test Subject**: service-netvps-1 (CasaOS & Docker services)

<img width="809" height="605" alt="Proxmox cluster overview showing Mac Mini nodes" src="https://github.com/user-attachments/assets/c0e4cd95-30a0-4973-b822-443ec569caae" />

#### Prerequisites
- PBS datastore accessible and healthy
- Target Proxmox node has sufficient storage capacity
- VM to be restored must be powered off or removed to avoid conflicts

![PBS backup configuration with 7-1-1 retention policy](image-3.png)

#### Test Setup
Before initiating the backup, a deliberate modification was made inside the VM to validate the restore process accuracy.

Inside one of the services hosted on netvps-1 (running CasaOS & Docker), the name of an item was intentionally changed. After applying this modification, the VM was shut down, and a backup was performed using Proxmox's integrated backup functionality, with the target set to PBS running on Hyper-V.

#### Restore Process
1. **Access Proxmox Web UI** → Navigate to target node
2. **Select Backup Location** → Choose PBS datastore 
3. **Browse Available Backups** → Locate desired VM backup snapshot
4. **Initiate Restore** → Select restore point (daily/weekly/monthly from 7-1-1 retention)
5. **Configure Restore Options**:
   - Restore to original location (default)
   - Verify storage target has adequate space
   - Confirm network configuration to avoid conflicts

6. **Execute Restore** → Monitor progress via Proxmox task viewer

**Tested Results**: Full VM restoration completed successfully with all services intact. CasaOS configuration and Docker containers restored to exact pre-backup state.

![Email notification of successful backup completion](image-8.png)

#### Security Features
- **Transport Encryption**: All data transfer protected by TLS between Proxmox nodes and PBS
- **At-Rest Encryption**: Backup data stored encrypted on PBS datastore
- **Authentication**: Proxmox API tokens secure access to PBS

### File-Level Recovery

**Tested Environment**: Proxmox VE 8.4.13 (Note: Version 8.4.0 had known file restore bug)  
**Test Subject**: Domain Controller VM - hamster image recovery from Pictures folder

![File restore error in Proxmox VE 8.4.0](image-9.png)

Following an upgrade (`apt update && apt dist-upgrade`) to version 8.4.13, the file restore issue was resolved.

#### Prerequisites  
- Proxmox VE version 8.4.13 or later (earlier versions may have file restore issues)
- PBS backup containing target files
- Write access to target VM filesystem

#### Recovery Process
1. **Navigate to PBS Interface** → Access via Proxmox datacenter view
2. **Browse Backup Contents** → Select specific backup snapshot
3. **File System Navigation** → Browse VM filesystem structure within backup

![Browsing backup snapshot contents in PBS](image-10.png)

4. **Locate Target Files** → Navigate to specific directories/files for recovery

![File selection interface in PBS backup browser](image-11.png)

5. **Download/Extract Files** → PBS provides direct download capability

![PBS file extraction interface](image-12.png)

6. **Manual File Placement** → Place recovered files in target VM locations

![Successfully restored hamster image](image-13.png)

**Tested Results**: Successfully recovered individual image file from Windows VM Pictures folder. File integrity confirmed post-recovery - the restored hamster image verified the success of the procedure.

#### Limitations
- **Manual Process**: Unlike Veeam, PBS requires manual file placement after download
- **Version Dependency**: File restore feature requires Proxmox VE 8.4.13+
- **Guest Agent**: Some functionality may require Proxmox guest agent installation

## Veeam Backup & Replication Restore Procedures

### Full VM Restore

**Tested Environment**: Veeam Community Edition on Windows Server 2025  
**Test Subject**: service-zomboid (Game server VM)

![Veeam backup repositories configuration showing local HDD and USB](image-19.png)

#### Prerequisites
- Veeam Backup & Replication installed and licensed
- Access to backup repositories (local HDD and USB replicas)
- Hyper-V host with adequate resources
- Target VM removed if restoring to original location

![Daily backup job configuration at 4 AM](image-21.png)

![Backup job guest processing settings](image-22.png)

#### Test Setup
To provide a realistic restore scenario, the service-zomboid VM was completely deleted, including manual removal of the hard disk files.

<img width="714" height="608" alt="VM deletion confirmation" src="https://github.com/user-attachments/assets/9a73a10d-ac18-4215-9499-8972b90ff329" />

<img width="700" height="226" alt="VM and disk deletion process" src="https://github.com/user-attachments/assets/da2b04b7-1dae-45de-ac84-92e8ef8aabc5" />

<img width="462" height="77" alt="Final confirmation of VM removal" src="https://github.com/user-attachments/assets/c872be58-e674-4b0b-b94c-4372ed181e00" />

#### Restore Process
1. **Launch Veeam Console** → Access Backup & Replication interface
2. **Navigate to Restore** → Select "Entire VM restore" option

<img width="1334" height="581" alt="Veeam restore options interface" src="https://github.com/user-attachments/assets/d8bc454f-ed5c-4af7-bd5c-b43d475dc208" />

3. **Choose Backup Source** → Select from available repositories (detects both primary HDD and USB replicas)
4. **Browse VM Backups** → Locate target VM in backup catalog
5. **Select Restore Point** → Choose from available incremental or full backup points
6. **Configure Restore Options**:
   - Restore to original location (tested configuration)
   - Verify Hyper-V host selection
   - Confirm storage path and network settings

<img width="564" height="174" alt="Restore job execution" src="https://github.com/user-attachments/assets/b16eb0ce-f1d0-415e-97ea-8dc4e479acbb" />

<img width="751" height="538" alt="Restore configuration interface" src="https://github.com/user-attachments/assets/535558b8-85af-40fb-bd3f-03c9c1b6a1b2" />

7. **Execute Restore** → Monitor restore progress

<img width="603" height="460" alt="Restore to original location option" src="https://github.com/user-attachments/assets/680957aa-0022-4896-aae0-fed121be8bda" />

<img width="706" height="578" alt="Successful VM restoration" src="https://github.com/user-attachments/assets/e89743a5-6116-4812-a315-3972d3851438" />

<img width="1025" height="852" alt="Restored VM in Hyper-V Manager" src="https://github.com/user-attachments/assets/3f21bc78-e818-4c70-904c-6f38f2c485a2" />

**Tested Results**: Complete VM restore executed flawlessly in approximately 3 minutes. service-zomboid game server restored with full functionality, all configurations intact.

#### Advanced Features
- **Repository Selection**: Automatically detects multiple backup repositories
- **Point-in-Time Recovery**: Choose specific backup snapshots from retention policy
- **Quick Recovery**: Optimized restore process with minimal downtime

### File-Level Recovery (Guest File Restore)

**Tested Environment**: Veeam Community Edition  
**Test Subject**: service-zomboid - individual file recovery

#### Prerequisites
- Veeam backup containing target VM
- VM can be online during file restore process
- Sufficient permissions on target system

#### Recovery Process
1. **Access Veeam Console** → Navigate to restore options
2. **Select Guest File Restore** → Choose file-level recovery option

<img width="410" height="67" alt="Veeam guest file restore option" src="https://github.com/user-attachments/assets/3db8dfcb-5de1-4f2c-b8c2-9349ed58ed21" />

3. **Choose Backup Point** → Select incremental or full backup snapshot

<img width="780" height="553" alt="Backup point selection interface" src="https://github.com/user-attachments/assets/36080441-8662-4a8d-b15a-4a82e95105b9" />

<img width="777" height="552" alt="Incremental backup selection" src="https://github.com/user-attachments/assets/0e8473f8-9ae2-4fc3-b714-51cef1314dc1" />

4. **Mount Backup** → Veeam mounts backup as browsable filesystem
5. **Navigate File Structure** → Browse complete VM filesystem within backup

<img width="1024" height="687" alt="Complete filesystem browser in Veeam" src="https://github.com/user-attachments/assets/3a6d72d2-d5fa-4189-9bec-5cd184bb4deb" />

6. **Select Target Files** → Choose specific files/folders for recovery
7. **Configure Restore Destination**:
   - **Direct VM Restore**: Files restored directly to running VM (tested method)
   - **Alternative Location**: Save to different location if needed

<img width="1033" height="691" alt="Direct VM file restore interface" src="https://github.com/user-attachments/assets/27bef9db-9934-42a3-9ade-6a7871c28fc9" />

<img width="325" height="232" alt="File restore completion confirmation" src="https://github.com/user-attachments/assets/b73a15ce-5d9e-4adb-857b-634bf52edc6f" />

8. **Execute File Restore** → Automatic placement in original VM location

**Tested Results**: Individual file recovery completed seamlessly with direct restoration to productive VM. Superior user experience compared to PBS manual process.

#### Key Advantages
- **Live VM Restore**: Files can be restored to running VMs without downtime
- **Direct Integration**: Automatic file placement in original VM locations  
- **Intelligent Backup Access**: Seamlessly accesses full backup chain for complete filesystem view
- **No Manual Intervention**: Unlike PBS, no manual download/upload process required

## Disaster Recovery Procedures

### Complete Infrastructure Loss Scenario

**Recovery Sequence for Total Hardware Failure**:

#### Phase 1: Infrastructure Rebuild
1. **Replace Hardware**: Acquire replacement OptiPlex server and Mac Mini nodes
2. **Install Base Systems**: 
   - Windows Server 2025 on OptiPlex
   - Proxmox VE on Mac Mini nodes
   - Configure basic networking (VLANs, IP addressing)

#### Phase 2: Backup Infrastructure Recovery
1. **Install Veeam**: Deploy Veeam Backup & Replication on new OptiPlex
2. **Install Proton Drive**: Download and configure Proton Drive desktop app
3. **Access Cloud Repository**: Connect to Proton Drive cloud storage via desktop app
4. **Restore PBS VM**: Use Veeam to restore PBS VM from cloud repository
5. **Configure PBS Access**: Establish connection between new Proxmox cluster and restored PBS

#### Phase 3: Service Recovery
1. **Restore Proxmox VMs**: Use restored PBS to recover all production VMs to new cluster
2. **Restore Hyper-V VMs**: Direct Veeam restore of game servers (if replacement OptiPlex includes Hyper-V)
3. **Validate Services**: Verify all restored services function correctly
4. **Resume Backup Operations**: Re-establish automated backup schedules

**Recovery Dependencies**:
- Proton Drive desktop app installation (provides Explorer-like access to cloud repository)
- Hardware replacement availability
- Internet connectivity for cloud repository access
- Valid Veeam licensing for new installation

**Estimated Recovery Time**: 
- **Infrastructure Setup**: 4-8 hours
- **PBS VM Recovery**: 1-2 hours  
- **Production Service Recovery**: 2-4 hours
- **Total RTO**: 8-14 hours (within defined targets for most services)

## PBS Redundancy & Cloud Backup Implementation

### Local Redundancy (3-2-1 Strategy - Second Medium)

To comply with the 3-2-1 backup strategy, specifically the *two media* requirement, the Proxmox Backup Server (PBS) was extended with an additional storage medium.


A 1TB HDD was physically attached to the host server, and an additional 512GB virtual disk (.vhdx) was assigned to the PBS VM. This resulted in two independent datastores of equal size (512GB each).

Both datastores were integrated into PBS with a Local Sync Job created to replicate the contents of Datastore1 onto Datastore2.

![Local sync job configuration for datastore replication](image-18.png)

The job configuration ensures that the target datastore remains synchronized and that notifications are sent via email upon successful completion or in case of errors.

### Cloud Backup Integration

For the third element of the 3-2-1 strategy (offsite storage), the system integrates with Proton Cloud through a simple yet effective approach.

<img width="188" height="68" alt="Proton Drive backup repository" src="https://github.com/user-attachments/assets/c15b3afb-0711-4d7b-912b-8a94bff028c3" />

<img width="1207" height="117" alt="Veeam cloud backup configuration" src="https://github.com/user-attachments/assets/177490a0-3626-42ec-9ba9-6ee7d49a3bfd" />

The implementation utilizes Veeam's repository cloning capabilities combined with the Proton Drive desktop application. Veeam creates a backup repository on the C: drive, and the Proton Application automatically synchronizes changes to the cloud.

<img width="262" height="57" alt="Cloud storage temporary staging indicator" src="https://github.com/user-attachments/assets/c8e36230-1e31-48b4-9545-32c999cf591d" />

This approach provides encrypted, secure offsite backup storage while maintaining simplicity and cost-effectiveness for the homelab environment.

### Repository Health Monitoring
- **Known Issues**: Primary HDD (B:) showing CRC errors and mechanical grinding
- **Mitigation**: USB replica provides immediate fallback capability
- **Cloud Backup**: Ensures offsite recovery capability despite local hardware issues

## Testing & Validation Summary

### Successful Test Cases
| Test Type | Platform | VM Tested | Result | Duration |
|-----------|----------|-----------|---------|----------|
| **Full VM Restore** | PBS | service-netvps-1 | ✅ Success | ~10 minutes |
| **Full VM Restore** | Veeam | service-zomboid | ✅ Success | ~3 minutes |  
| **File Recovery** | PBS | Domain Controller | ✅ Success | Manual process |
| **File Recovery** | Veeam | service-zomboid | ✅ Success | Direct restore |

### Performance Observations
- **Veeam**: Superior restore speed and user experience
- **PBS**: Solid functionality but requires manual intervention for file restores  
- **Both Platforms**: Meet defined RPO/RTO targets for all service tiers
- **Reliability**: No restore failures encountered during testing

---
*Last Updated: September 2025*  
*Testing completed for M143: Backup- und Restore-Systeme implementieren*
