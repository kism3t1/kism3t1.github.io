---
title: Managing Lists
parent: Cloudflare Zero Trust
nav_order: 2
---
# Managing Cloudflare Lists via CSV files
---

CSV file would have both url and description headers
```
url,description
https://example.com,This is the main example website
https://api.example.com,API endpoint for example.com
https://www.example.com/admin,Admin interface
```

```
# --- Read the CSV file and decode ---
data "local_file" "http_allow_csv" {
  filename = "csvs/http_allow.csv"
}

locals {
  http_allow_list = csvdecode(data.local_file.http_allow_csv.content)
}

# --- Create the Cloudflare Zero Trust List Resources ---
resource "cloudflare_zero_trust_list" "http_allow" {
  account_id  = var.account_id
  name        = "HTTP Allow List"
  description = "HTTP Allow List from csv http_allow"
  type        = "URL"
  items       = [
    for item in local.http_allow_list : { value = item.url, description = item.description }
  ]
}
```

This would create the Cloudflare Zero Trust List

https://example.com,This is the main example website
https://api.example.com,API endpoint for example.com
https://www.example.com/admin,Admin interface