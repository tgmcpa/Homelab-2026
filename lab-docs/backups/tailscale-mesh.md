# Remote Backup Strategy & Tailscale Mesh
**Target:** RS815+ Relocation (Son's House)

## Hardware Preparation
- RS815+ Hardware Audit: Perform C2000 Clock Signal flaw check (100-ohm resistor fix).

## Deployment Steps
1. Wipe RS815+ to clear legacy bloat.
2. Re-initialize with Btrfs.
3. Install Tailscale + Hyper Backup Vault.
4. Seed initial backup of irreplaceable data (Photos/Docs) locally.
5. Relocate to remote site.

## Technical Note
* Required: 100-ohm 1/4w resistor soldered to LPC clock header to mitigate C2000 bus failure.
