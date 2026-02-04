# Homelab Session Log — Control Plane & Traefik Security  
**Date:** 2026-02-01  

---

## 🧠 Architecture Clarified

**Control Plane:** LXC 105 (10.0.0.245)  
Runs:
- Traefik (reverse proxy / ingress)
- Authentik (SSO)
- Homepage
- Dockge

**Compute Node:** LXC 100 (10.0.0.246)  
Runs Radarr, Sonarr, Overseerr, and other application stacks.

**Note:**  
Z240 host (10.0.0.20) is *not* serving application ports.

---

## 🌐 DNS / Ingress

AdGuard wildcard DNS confirmed:

```
*.tgmcpa.net → 10.0.0.245
```

Removed redundant DNS rewrites:
- overseerr.tgmcpa.net  
- dockge-z240.tgmcpa.net  
- dockge-pi5.tgmcpa.net  

Ingress is now clearly centralized to **Traefik on LXC 105**.

---

## 🔍 Network Validation (Control Plane → Compute Node)

From LXC 105:

```
curl http://10.0.0.246:7878 → 200 (Radarr)
curl http://10.0.0.246:8989 → 200 (Sonarr)
curl http://10.0.0.246:5055 → 307 (Overseerr)
```

Confirms Traefik can reach backend services over the LAN.

---

## 🔐 Cloudflare API Token Security Fix

**Problem:**  
Cloudflare DNS API token was stored directly in Traefik compose file and exposed in plain text.

**Actions Taken:**

1. Created new Cloudflare API Token  
   - Template: **Edit zone DNS**  
   - Scope: **tgmcpa.net only**

2. Saved token to:

   ```
   /opt/stacks/traefik/.env
   ```

   ```
   CF_DNS_API_TOKEN=NEW_TOKEN_HERE
   ```

3. Updated Traefik compose file  
   **Removed** inline token from `environment:` section  
   **Added:**

   ```yaml
   env_file:
     - /opt/stacks/traefik/.env
   ```

---

## 🚨 Traefik Certificate Resolver Error

**Error:**
```
Router uses a nonexistent certificate resolver
certificateResolver=myresolver routerName=authentik@docker
```

**Cause:**  
Authentik container label referenced old resolver name `myresolver`.

**Fix:**

Edited:

```
/opt/stacks/authentik/compose.yaml
```

Changed:

```
traefik.http.routers.authentik.tls.certresolver=myresolver
```

To:

```
traefik.http.routers.authentik.tls.certresolver=cloudflare
```

Redeployed Authentik stack and restarted Traefik.

**Result:**  
No further resolver errors in Traefik logs.

---

## ✅ End State

- Control plane networking verified  
- DNS clean and centralized  
- Cloudflare token secured  
- Traefik resolver configuration corrected  
- System stable  

---

## ▶️ Next Session Starting Point

Publish first backend app (**Radarr**) through Traefik + Authentik

## 📅 Homelab Progress Log — 2026-02-03

**Status:** Infrastructure definition phase complete. Preparing to begin control-plane and observability rollout.

---

### 🌐 Current Network State
- **Router:** TP-Link BE9300 using Main / Guest / IoT SSID segmentation  
  *(Temporary solution until VLAN-capable router upgrade)*
- **Switch:** Aruba 1930 24-port PoE *(VLAN capable, not yet fully utilized)*

---

### 🖥 Active Nodes and Roles

| Host | IP | Role |
|------|----|------|
| **Pi5 – gatekeeper** | 10.0.0.10 | DNS, AdGuard, Unbound, utility services |
| **Mini – pve-mini** | 10.0.0.21 | Control-plane node (Traefik, Authentik, Homepage) |
| **Z240 – pve-z240** | 10.0.0.20 | Core Docker workloads and services (includes Vaultwarden) |
| **iMac – imac** | 10.0.0.22 | Proxmox node, currently idle |
| **NAS** | 10.0.0.30 | Storage and download clients |

---

### 🔎 Docker Discovery Status
Docker socket proxy is confirmed running on:
- **Pi5**
- **Z240**

This allows **Homepage** to discover containers across hosts.

---

### 🎯 Next Implementation Phase
**Goal:** Establish unified ingress and observability before expanding services

Planned order of work:

1. Validate and standardize **Traefik routing**
2. Restore **Vaultwarden** access through Traefik
3. Configure **Homepage** to discover all Docker hosts
4. Deploy observability stack:
   - **Gotify** — notifications
   - **Uptime Kuma** — service monitoring
   - **Dozzle** — container log viewer
   - **NetAlertX** — network presence alerts

---

### 🧱 Planned but Not Yet Activated

| Service | Target Platform | Purpose |
|---------|----------------|---------|
| **Immich** | Dedicated LXC | Photo management + AI indexing |
| **Frigate** | Dedicated LXC w/ GPU + Coral passthrough | NVR + object detection |
| **Home Assistant OS** | VM on iMac | Home automation hub |

These systems will be provisioned in stages after observability is operational.

---

### 📌 Operational Discipline
After each major milestone:
- Traefik operational
- Vaultwarden accessible
- Homepage discovery working
- Observability stack online  

➡ Update this log and push changes to Git.

## 📅 Homelab Progress Log — 2026-02-03 (Traefik + KOP Session)

**Status:** Traefik multi-host automation architecture established. Routing pipeline nearly operational.

---

### 🌐 Control Plane Architecture Changes

**Objective:** Eliminate per-service Traefik file edits and enable automatic routing across multiple Docker hosts.

**Implemented:**
- Redis deployed on **Mini LXC 105 (10.0.0.245)** as shared config store for Traefik
- Traefik updated to use **Redis provider** in addition to Docker provider
- `traefik-kop` agent deployed on **Z240 LXC 100 (10.0.0.246)**
  - KOP now:
    - Reads Docker labels locally on Z240
    - Publishes service/router definitions into Redis
- Connectivity verified between:
  - Z240 → Redis (10.0.0.245:6379)
  - Z240 → Docker Socket Proxy (10.0.0.246:2375)

**Result:**  
Traefik is now receiving dynamic configuration for services running on Z240 without manual file edits.

---

### 📦 Services Now Publishing to Traefik via KOP
- **Vaultwarden**
- **Overseerr**

KOP logs confirm both services are being published into Redis.

---

### ⚠️ Current Issue (Stopping Point)

Attempting to access:


returns **HTTP 404** from Traefik.

This indicates Traefik does not yet have a fully valid HTTPS router for Vaultwarden.

---

### 🔎 Diagnosis in Progress

Vaultwarden already has:
- `traefik.enable=true`
- `traefik.http.routers.vaultwarden.rule=Host(...)`
- `traefik.http.services.vaultwarden.loadbalancer.server.port=80`

But it **may still be missing** required TLS labels:

We need to verify whether those labels were successfully applied to the running container and then confirm Traefik sees the router via the dashboard/API.

---

### ▶️ Next Session Starting Point

1. **Verify active Vaultwarden labels**
   ```bash
   docker inspect vaultwarden --format '{{range $k,$v := .Config.Labels}}{{println $k "=" $v}}{{end}}' | grep traefik

## 📅 Homelab Progress Log — 2026-02-03 to 2026-02-04 (Vaultwarden First Live Route, KOP Fix)

**Goal:** Finish first live route (Vaultwarden) via Traefik + Traefik-KOP + Redis, without manual per-service Traefik file edits.

### ✅ What worked (final state)
- **Vaultwarden is live:** `https://vaultwarden.tgmcpa.net` loads successfully.
- **Traefik source of truth for Vaultwarden is Redis**:
  - Router: `vaultwarden@redis`
  - Service target: `http://10.0.0.246:11001`
- **Traefik Docker provider reverted to local-only** to prevent duplicates:
  - Traefik Docker provider endpoint is `unix:///var/run/docker.sock`
  - Duplicate `vaultwarden@docker` entries (pointing to `172.x`) are gone.
- **Manual file-provider Vaultwarden route disabled** (it was used briefly to prove end-to-end routing, then removed once Redis/KOP path worked).

### 🔧 Root cause & fix
**Root cause:** Traefik-KOP was publishing container bridge IPs (`172.x.x.x`) into Redis, which are not reachable cross-host from Traefik on the Mini.  
**Fix:** Configure Traefik-KOP to publish the node/LAN IP by setting **BIND_IP**.

### 🧩 Traefik-KOP configuration pattern (for future KOP instances)
In each host’s `traefik-kop` compose file, set:

```yaml
environment:
  DOCKER_HOST: tcp://<LOCAL_DOCKER_SOCKET_PROXY_IP>:2375
  REDIS_ADDR: 10.0.0.245:6379
  REDIS_ROOT_KEY: traefik
  CONSTRAINTS: "Label(`traefik.enable`,`true`)"
  BIND_IP: <THIS_NODE_LAN_IP>   # e.g. 10.0.0.246
