---
title: Creating Tunnels and Routes
parent: Cloudflare Zero Trust
nav_order: 3
---
# Creating Tunnels and Routes
---

Example cloudflared Tunnel and Route

```
resource "cloudflare_zero_trust_tunnel_cloudflared" "Internal_Tunnel_01" {
  account_id = var.account_id
  name       = "Internal_Tunnel_01"
  config_src = "cloudflare"
}

resource "cloudflare_zero_trust_tunnel_cloudflared_route" "Internal_Tunnel_01_Route" {
  account_id = var.account_id
  network    = "192.168.1.0/24"
  tunnel_id  = cloudflare_zero_trust_tunnel_cloudflared.Internal_Tunnel_01.id
  comment    = "Internal subnet route"
}
```