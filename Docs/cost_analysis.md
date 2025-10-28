# Cost Analysis
## Summary
This cost analysis provides a detailed breakdown of the finances in the compute and networking infrastructure for the described Proxmox cluster and supporting systems. The project leverages a mix of used hardware acquisitions, including budget-friendly deals from friends, and some factory new purchased eqquiptment. Software components are mostly open-source or free-tier, resulting in no additional licensing expenses.

The total hardware investment for the core components (compute nodes, windows server, and networking) amounts to **855 CHF**. Operational costs (e.g., electricity, domain renewal) are not included but are estimated to be minimal.

## Hardware Costs Breakdown

### Compute Layer (Proxmox Cluster and Supporting Devices)
The Proxmox cluster comprises three nodes (two 2012 Mac Minis and one 2019 MacBook Pro), supplemented by a Dell OptiPlex for backup and replication tasks, and a Raspberry Pi for dedicated Home Assistant operations. Costs are based on used market purchases:

- **srv-macmi-1-51 (2012 Mac Mini, quad core, 16GB DDR3 RAM)**: 125 CHF  
- **srv-macmi-2-52 (2012 Mac Mini, dual core, 8GB DDR3 RAM)**: 19 CHF  
- **srv-macmi-3-53 (2019 MacBook Pro, quad core, 8GB DDR3 RAM)**: 305 CHF  
- **srv-optiplex (Dell OptiPlex 7070, 8-core/8-thread, 64GB DDR4 RAM, with Hyper-V and backup software)**: 110 CHF  
  *(Note: Additional hardware upgrades obtained via informal arrangements with friends are not quantified in this analysis.)*  

**Subtotal for Compute Layer**: 559 CHF  

### Networking Layer
The Ubiquiti UniFi ecosystem provides managed switching, routing, and wireless access. Items were sourced as used (except the Cloud Gateway, purchased new):

- **Ubiquiti UniFi Switch 24 PoE**: 150 CHF (used)  
- **UniFi Cloud Gateway Ultra**: 98 CHF (new)  
- **UniFi AP AC Lite**: 10 CHF (used, from friend)  
- **UniFi AP AC Pro**: 38 CHF (used)  

**Subtotal for Networking Layer**: 296 CHF  

### Storage Components
Storage (SSDs, HDDs, and microSD) is integrated into the above devices and not purchased separately. No additional standalone costs are reported.

## Software and Services Costs
The project utilizes a stack of open-source and free-tier tools, ensuring no licensing fees:

- **Proxmox VE (Virtual Environment)**: Free (open source)  
- **Proxmox Backup Server**: Free (open source)  
- **Windows Server Datacenter (with Hyper-V)**: Free evaluation/trial or included with hardware (no additional cost)  
- **Veeam Backup & Replication**: Free Community Edition  
- **Home Assistant (on Raspberry Pi)**: Free (open source)  
- **Proton Drive and Proton Mail (with GioTech.ch domain)**: Free tier for core services; domain registration/renewal (~10–20 CHF annually, not included in initial project costs)  

**Subtotal for Software/Services**: 0 CHF  

## Total Project Investment
- **Hardware Total**: 910 CHF (Compute: 614 CHF + Networking: 296 CHF)  
- **Software/Services Total**: 0 CHF  
- **Grand Total**: **910 CHF**  
