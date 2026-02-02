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
