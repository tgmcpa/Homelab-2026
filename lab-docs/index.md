# Homelab Operations Center

## Project Documentation Protocol
* **Platform**: MkDocs (Material Theme).
* **Workflow**: Live-editing in VS Code with local browser preview (`localhost:8000`).
* **Source of Truth**: The local `Lab-Docs` directory on the HP Laptop.
* **Sync Strategy**: [TBD] - We will determine the sync/backup method once the local baseline is complete.

## Decision History
* **Homepage**: Selected as the canonical landing page for service-status integration.
* **Pi 5 Role**: Designated as the "Gatekeeper" node (Pi-hole, AdGuard, DNS). It is a lightweight infrastructure node, NOT a physical router/firewall replacement.
* **NVIDIA T1000**: Installed in Z240 to offload Tdarr/Frigate tasks.

