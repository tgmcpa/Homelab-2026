# Homelab Stacks Registry (with key metadata)

## PI5 (gatekeeper)

- /opt/stacks/adguard/compose.yaml — AdGuard Home — HOST NET (DNS :53, UI :3000) — deps: none
- /opt/stacks/beszel/compose.yaml — Beszel hub + agent — hub :8090, agent HOST NET — deps: docker.sock (ro), SSH key
- /opt/stacks/dockge/compose.yaml — Dockge — :5001 — deps: docker.sock (rw), /opt/stacks mounted
- /opt/stacks/glances/compose.yaml — Glances — HOST NET (web :61208) — deps: docker.sock (ro)
- /opt/stacks/socket-proxy/compose.yaml — Docker socket proxy — :2375 — deps: docker.sock (ro)

## MINI — LXC 105 (docker-core)

- /opt/stacks/traefik/compose.yaml — Traefik v3 + whoami — PUBLIC :80/:443, admin :8080 — deps: external network `proxy`, Cloudflare token
- /opt/stacks/authentik/compose.yaml — Authentik + Postgres + Redis — :9000/:9443 — deps: `.env` (PG_PASS), external network `proxy`, docker.sock (worker)
- /opt/stacks/homepage/compose.yaml — Homepage — :3000 — deps: external network `proxy`, docker.sock (ro), `/opt/stacks/global.env`, authentik middleware in Traefik
- /opt/stacks/dockge/compose.yaml — Dockge — :5001 — deps: docker.sock (rw), /opt/stacks mounted

### Env files
- /opt/stacks/authentik/.env
- /opt/stacks/global.env

## Z240 — LXC 100 (docker-compute)

- /opt/stacks/plex/docker-compose.yml — Plex — HOST NET — deps: /mnt/nas/Movies, /mnt/nas/tv
- /opt/stacks/radarr/docker-compose.yml — Radarr — :7878 — deps: /opt/stacks/global.env, /mnt/nas/Movies, /mnt/nas/NAS-Downloads
- /opt/stacks/sonarr/docker-compose.yml — Sonarr — :8989 — deps: /opt/stacks/global.env, /mnt/nas/tv, /mnt/nas/NAS-Downloads
- /opt/stacks/overseerr/compose.yml — Overseerr — :5055 — deps: (Traefik labels present but no Traefik here)
- /opt/stacks/docker-proxy/compose.yml — Docker socket proxy — :2375 — deps: docker.sock (ro), privileged
- /opt/stacks/glances/docker-compose.yml — Glances — HOST NET (web :61208) — deps: docker.sock (ro)
- /opt/stacks/beszel-agent/docker-compose.yml — Beszel agent — HOST NET — deps: docker.sock (ro), SSH key
- /opt/stacks/huntarr/docker-compose.yml — Huntarr — (Traefik-routed) — deps: external network `traefik_network`
- /opt/stacks/readarr-audio/docker-compose.yml — Readarr (audio) — (Traefik-routed) — deps: /mnt/nas/Music, /mnt/nas/NAS-Downloads, external network `traefik_network`
- /opt/stacks/readarr-ebook/docker-compose.yml — Readarr (ebook) — (Traefik-routed) — deps: /mnt/nas/Books, /mnt/nas/NAS-Downloads, external network `traefik_network`

### Env files
- /opt/stacks/global.env
