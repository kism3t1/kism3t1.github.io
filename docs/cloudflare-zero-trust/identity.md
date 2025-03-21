---
title: Creating Identity Provider
parent: Cloudflare Zero Trust
nav_order: 4
---
# Creating Identity Provider
---

Example Identity Provider configuration

# Google SAML

```
resource "cloudflare_zero_trust_access_identity_provider" "Google" {
  account_id = var.account_id
  name       = "Google"
  type       = "saml"
  config = {
    acs_url              = "https://tenant.cloudflareaccess.com/cdn-cgi/access/callback"
    email_attribute_name = "email"
    attributes           = ["email", "Region", "Country", "Department", "Role", "Special"]
    issuer_url           = "https://accounts.google.com/o/saml2?idpid=xxxxxxx"
    sso_target_url       = "https://accounts.google.com/o/saml2/idp?idpid=xxxxxxx"
    idp_public_certs     = ["DdAlygAwIBA....DFDDfdfdGfdtdgsDsdd"]
  }
  scim_config = {
    enabled                  = true
    identity_update_behavior = "automatic"
    seat_deprovision         = true
    user_deprovision         = true
  }
}
```

# Okta

```
resource "cloudflare_zero_trust_access_identity_provider" "Okta" {
  account_id = var.account_id
  name       = "Okta"
  type       = "okta"
  config = {
    client_id        = "fghsjh45aebvbjkfsfhgfgf"
    client_secret    = var.okta_secret
    okta_account     = "https://production.okta.com"
    pkce_enabled     = true
    email_claim_name = "email"
    claims           = ["email", "region", "country", "department", "job_role", "special"]
  }
  scim_config = {
    enabled                  = true
    identity_update_behavior = "automatic"
    seat_deprovision         = true
    user_deprovision         = true
  }
}
```