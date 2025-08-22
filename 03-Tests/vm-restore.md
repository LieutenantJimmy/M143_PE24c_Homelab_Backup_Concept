# Test – Full VM Restore (Proxmox → PBS)
**[⬅ Back to README](/README.md)**

1. Pick `service-ubuntu-test` (or another safe VM).
2. In PBS: verify latest backup snapshot exists.
3. In Proxmox: **Restore** from PBS into a new VM ID (do not overwrite prod).
4. Boot the restored VM; verify services / SSH.
5. Capture screenshots: PBS snapshot list, restore dialog, VM console after boot.
6. Note restore time and compare to RTO.
