# Network Topology & Security

## Current Management Tier
* **Subnet**: 10.0.0.0/24
* **Gateway**: 10.0.0.1
* **DNS**: 1.1.1.1 (Temporary)

## Access Protocols
* **SSH Port**: 51777 (NAS) / 22 (Standard)
* **Identity**: id_ed25519.pub
* **Aliases**: Configured in ~/.ssh/config

## Infrastructure Goals
* **Managed Switch**: Aruba Instant On 1930 24G PoE
* **VLAN Targets**: Trusted/Admin, Servers, Cameras, IoT
* **Hardware Interconnects**: SFP+ DAC Cables for NAS/Z240