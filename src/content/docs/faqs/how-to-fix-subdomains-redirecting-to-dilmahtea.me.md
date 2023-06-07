---
title: How to Fix Subdomains Redirecting to the Apex Domain (Dilmahtea.me)
---

If you have recently configured a service to run on a subdomain of https://dilmahtea.me and also you have updated the DNS records in Cloudflare but the subdomain is still redirecting to https://dilmahtea.me, then follow the steps below to fix the issue.

- Login to the [Cloudflare Dashboard](https://dash.cloudflare.com/)
- Select the `Dilmah` account
- Select the `dilmahtea.me` website
- Go to the `Rules > Redirect Rules` tab
- Edit the `Forward Traffic from Subdomains to Apex Domain` rule
- Add the subdomain to the `Hostname is not in` condition

Voila! The subdomain should now be accessible.
