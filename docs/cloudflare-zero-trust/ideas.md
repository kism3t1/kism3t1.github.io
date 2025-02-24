---
title: Ideas
parent: Cloudflare Zero Trust
nav_order: 1
---
# Cloudflare Zero Trust Design Ideas
---

Policies be based on Lists where possible so updating is simplified by updating a csv file and committing it rather than amending policies

For example:

HTTP_Allow_List would contain 
```
example.com
api.com
```
Then the HTTP_Allow_Policy would reference that list
```
traffic    = "any(http.conn.domains[*] in {\"${cloudflare_zero_trust_list.HTTP_Allow_List.id}\"})"
```