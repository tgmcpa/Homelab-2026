# Project Backlog & Sandbox

## Phase 2/3: High Priority (Infrastructure & Security)
- [ ] **Static Zone**: Complete Tether App Address Reservations (Z240, Mini, HP-Laptop, S24).
- [ ] **Atomic Moves**: Implement Unified Root (/data) structure on all containers.
- [ ] **Storage**: Migrate CIFS to NFS mounts for improved Z240/Proxmox performance.
- [ ] **Gatekeeper**: Dual-run Pi-hole + AdGuard Home with Unbound on Pi 5.
- [ ] **Security**: Hardening router to point to Pi 5 DNS.
- [ ] **Reverse Proxy**: Batch update 46 Synology Reverse Proxies in DSM UI.
- [ ] **Management**: Deploy Dockge on Proxmox for compose-native management.

## Project: The Great Photo Migration (750k+ items)
*Goal: Establish a single repository on DS1621+ with Immich frontend.*
- [ ] **Extraction**: Execute Google Takeout full export.
- [ ] **Metadata**: Use `exiftool` to sync JSON metadata into image headers (Hardening).
- [ ] **Deduplication**: Run `Czkawka` on Z240 against staged takeout.
- [ ] **Ingest**: Staggered 50GB batches; disable ML/Transcoding initially.
- [ ] **Configuration**: Separate DB on SSD; multi-user logic for S24 Ultra & iPhone 17 Pro.

## Project: RS815+ Offsite Migration
*Goal: Physical relocation to Son's house for remote backup.*
- [ ] **Hardware Audit**: Verify 100-ohm resistor fix for C2000 clock flaw.
- [ ] **System**: Wipe legacy bloat and re-initialize with Btrfs.
- [ ] **Software**: Install Tailscale + Hyper Backup Vault.
- [ ] **Seeding**: Perform initial backup of Photos/Docs locally before moving.

## Tdarr Optimization Protocol
- [ ] **Cleanup**: Identify/remove original files from failed legacy deletions.
- [ ] **Logic**: Refine plugins to "Delete source" only after 100% health check.
- [ ] **Watchdog**: Implement folder-size reduction verification script.

## Sandbox: Performance & Monitoring
- [ ] sandbox: Compare Beszel vs. Glances for long-term monitoring strategy.
- [ ] sandbox: Tune Frigate object detection thresholds (see history/notes_archive).
- [ ] sandbox: Evaluate Wireguard vs. Tailscale throughput for remote NAS access.
- [ ] sandbox: Set up old iPad and Fire tablet as dedicated dashboards.
- [ ] sandbox: Local LLM hardware requirements audit vs. existing inventory.
- [ ] sandbox: Research metadata cleanup for ebooks/audiobooks library.
- [ ] sandbox: Fix Readarr search by switching to :nightly or :dev builds.
- [ ] sandbox: Wyze Bridge and Cryze docker evaluation.
- [ ] sandbox: Echo Device integration via Alexa Media Player.

