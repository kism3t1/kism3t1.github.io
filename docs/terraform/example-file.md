---
title: Example File
parent: Terraform
nav_order: 2
---

# Code
{: .no_toc }

---

```
terraform {
  required_providers {
    cloudflare = {
      source  = "cloudflare/cloudflare"
      version = "~> 5"
    }
  }
}

provider "cloudflare" {
  api_token = var.cloudflare_api_token
}
```