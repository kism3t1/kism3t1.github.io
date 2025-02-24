---
title: Example Policy
parent: Zero Trust Policy
nav_order: 1
---

```
resource "cloudflare_zero_trust_gateway_policy" "HTTP_Allow" {
  description = "Allow List of URL's"
  account_id = var.account_id
  action     = "off"
  enabled    = true
  filters    = ["http"]
  name       = "HTTP_Allow"
  precedence = 620
  traffic    = "any(http.conn.domains[*] in {\"${cloudflare_zero_trust_list.HTTP_Allow.id}\"})"
}
```