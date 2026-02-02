# Software & Container Registry

## Host: DS1621+ (Synology)
| Container / Service | Status | Port | Notes |
| :--- | :--- | :--- | :--- |
| Plex | Running | Native | Native Package |
| Homepage | Running | 3550 | Healthy |
| Vaultwarden | Running | 8888 | Healthy |
| Radarr / Sonarr | Running | 49166/5 | Healthy |
| Prowlarr | Running | 9696 | Indexer Management |
| Sabnzbd | Running | 8080 | Usenet Downloader |
| Overseerr | Running | - | Requests |
| Tautulli | Running | - | Plex Monitoring |
| Gotify | Running | 6742 | Notifications |
| Uptime-Kuma | Running | 3444 | Redundant monitor |
| Glances / Dozzle | Running | - | System/Log Monitoring |
| Portracker | Running | - | Healthy |
| Transmission-VPN | Running | 9091 | Active |
| Trailarr / Mongo | Running | - | Healthy |
| Linkwarden | Stopped | - | Boot Loop (Audit Priority) |
| Paperless-ngx | Stopped | - | Exited 2 weeks ago |
| Lidarr / Readarr | Stopped | - | Exited |
| NetAlertX / ABS | Created | - | Pending Setup |

## Host: Z240 (Target: Proxmox 8.x)
| Container / Service | Strategy | Target Type | Notes |
| :--- | :--- | :--- | :--- |
| Frigate | Migrate | LXC (GPU Passthrough) | Priority 1 (T1000/Coral) |
| Vaultwarden | Consolidate | LXC | Use DS1621+ as DB backend? |
| Traefik | Rebuild | VM/LXC | Move to Gatekeeper tier |
| Immich | Rebuild | LXC | Requires Hardware Accel |
| Nextcloud | Migrate | VM | Heavy lifting |

## Host: Mac Mini (Ubuntu)
| Container / Service | Type | Status | Port | Notes |
| :--- | :--- | :--- | :--- | :--- |
| Uptime Kuma | Container | Running | 3001 | Healthy |
| Pi-Hole | Container | Running | 53, 80 | Primary DNS |
| Tdarr (Server/Node) | Container | Running | 8265 | Active |
| Portainer | Container | Running | 9443 | Active Management |
| Dockpeek | Container | Running | 3420 | Socket Proxy Active |
| Glances | Container/Svc| Running | 61208 | System Service active |
| Watchtower | Container | Running | 9911 | Healthy |
| Tailscale | Service | Running | - | Mesh VPN |
| Home-Assistant | Container | Stopped | - | Exited 2 months ago |
| Transmission | Container | Stopped | - | Exited 6 weeks ago |

## Host: RS815+ (Synology - Backup)
| Container / Service | Status | Health | Port | Notes |
| :--- | :--- | :--- | :--- | :--- |
| Hyper Backup Vault | Running | Healthy | Native | Target for DS1621+ |
| Active Insight | Running | Healthy | - | |
| Portainer (App/Agent) | Running | Healthy | 9443 (Ext: 49186) | Management |
| Dockpeek (App/Proxy) | Running | Healthy | 3812 / 2375 | Management |
| Portracker | Running | Healthy | 49189 | Port Monitoring |
| Glances | Running | Healthy | 61208 | System Monitoring |
| Watchtower | Running | Healthy | 49153 | Updates |
| Dozzle Agent | Running | Healthy | 7007 | Logs |
| Tdarr | Stopped | - | - | Exited 2 months ago |

## Host: iMac (Proxmox 8.4)
| VM/CT ID | Name | Status | OS | Notes |
| :--- | :--- | :--- | :--- | :--- |
| [TBD] | - | - | - | Potential Home Assistant target |
| [TBD] | - | - | - | Potential Test Lab / Sandbox |

## Host: Pi 5 (Gatekeeper - Pending)
| Service | Status | Health | Notes |
| :--- | :--- | :--- | :--- |
| AdGuard Home | [Plan] | - | Primary DNS |
| Pi-hole | [Plan] | - | Secondary DNS (Redundancy) |
| Unbound | [Plan] | - | Recursive DNS Resolver |