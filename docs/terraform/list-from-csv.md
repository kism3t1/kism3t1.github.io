---
title: Creating a list from a csv
parent: Terraform
nav_order: 2
---

## Creating a Cloudflare list from a csv via a dynamic rule.

```
locals {
	# Load the rule CSV's
    ssl_inspection_list                 = csvdecode(file("csvs/bypass_ssl_inspection.csv"))
	# Iterate over the CSV's and save to a list
	ssl_inspection_list_rule_map = { for c in local.ssl_inspection_list : c.url => c }
}

resource "cloudflare_zero_trust_list" "Bypass_SSL_Inspection" {
  account_id  = var.account_id
  name        = "Bypass_SSL_Inspection"
  type        = "HOSTNAMES"
  description = "Bypass list for URL SSL Inspection."
  dynamic "rule" {
		for_each = local.ssl_inspection_list_rule_map
		content{
        items               = rule.value.url
		description         = rule.value.description
		}
	}
}
```