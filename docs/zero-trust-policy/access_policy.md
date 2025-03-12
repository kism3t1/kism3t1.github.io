---
title: Access Policy
parent: Zero Trust Policy
nav_order: 2
---
# How Access Policies work when using access groups and reusable policies
---

I first create a 'default users' access group that is taking a list of email addresses for the users I want to be a member of the group

```
resource "cloudflare_zero_trust_access_group" "default_users" {
  account_id = var.account_id
  name       = "Default_Users"
  include = [{
    email_list = {
      id = cloudflare_zero_trust_list.email_list.id
    }
  }]
  is_default = false
  require = [{
    email_list = {
      id = cloudflare_zero_trust_list.email_list.id
    }
  }]
}
```

Next is to create a reusable allow policy that is using the above access group as the members, this policy can then be used for future access applications.

```
resource "cloudflare_zero_trust_access_policy" "allow_default_users" {
  account_id = var.account_id
  decision   = "allow"
  include = [{
    email_list = {
      id = cloudflare_zero_trust_list.email_list.id
    }
  }]
  name                           = "allow_default_users"
  isolation_required             = false
  purpose_justification_required = false
  require = [{
    group = {
      id = cloudflare_zero_trust_access_group.default_users.id
    }
  }]
  session_duration = "6h"
}
```

I can then use that reusable policy in an access application. 
In the below example I am allowing the users in the email list access to the two internal applications over 443.

```
resource "cloudflare_zero_trust_access_application" "internal_web_apps" {
  name                        = "internal_web_apps"
  type                        = "self_hosted"
  account_id                  = var.account_id
  allow_authenticate_via_warp = false
  app_launcher_visible        = false
  auto_redirect_to_identity   = true
  destinations = [{
    type       = "private"
    cidr   = "192.168.1.220/32"
    l4_protocol = "tcp"
    port_range = "443"
    }, {
    type       = "private"
    hostname   = "plex.lan"
    port_range = "443"
  }]
  policies = [{
    id         = cloudflare_zero_trust_access_policy.allow_default_users.id
    decision   = "allow"
    precedence = 0
  }]
  same_site_cookie_attribute = "strict"
  service_auth_401_redirect  = true
  session_duration           = "6h"
  tags                       = ["internal"]
}
```