---
title: How to Start a New Service on the Hetzner VPS
---

## Setup

First, create a new directory for the service in the `/home/strapi` directory. It's recommended to create a new repository for the service too. Once the service is ready to be deployed, move on to the next step.

## Register a PM2 Service

Next, we have to update the `/home/strapi/ecosystem.config.js` file to register the new service. Add a new entry to the `apps` array. The updated file should look something like this:

```js
module.exports = {
  apps: [
    // other apps ...
    {
      name: "new-service",
      script: "/home/strapi/new-service-directory/server.js",
    },
  ],
};
```

Please refer to the [PM2 Configuration File documentation](https://pm2.keymetrics.io/docs/usage/application-declaration/) for more information.

## Save the Updated Configuration

Then run the following command to save the update configs to make the new service start automatically on startup:

```bash
pm2 save
```

## Update the Caddyfile

We use [Caddy](https://caddyserver.com/) as the reverse proxy for all the services. We have to add a new entry to the `/etc/caddy/Caddyfile` file to prxy the new service. Run the following command to open the file in the nano editor:

```bash
sudo nano /etc/caddy/Caddyfile
```

Then add a new entry to the file. The updated file should look something like this:

```conf
# other services ...
new-service.dilmahtea.me {
  reverse_proxy 127.0.0.1:1234
}
```

Please refer to the [Caddyfile documentation](https://caddyserver.com/docs/caddyfile) for more information.

## Restart Caddy

Finally, restart Caddy to apply the changes:

```bash
caddy reload --config /etc/caddy/Caddyfile
```

## Update the DNS Records (Optional)

If the service is configured to run on a new subdomain, then we have to update the DNS records in Cloudflare. Follow the steps below to update the DNS records:

- Login to the [Cloudflare Dashboard](https://dash.cloudflare.com/)
- Select the `Dilmah` account
- Select the `dilmahtea.me` website
- Go to the `DNS` tab
- Add a new `A` record with the following details:
  - Name: `subdomain`
  - IPv4 address: `65.108.211.110`
  - TTL: `Auto`
  - Proxy status: `DNS only`

Here, replace the `subdomain` with the actual subdomain name.

> `65.108.211.110` is the IP address of the Hetzner VPS.

## Update the Redirect Rule (Optional)

We have a **Redirect Rule** configured in Cloudflare to redirect all subdomain traffic to the apex domain (https://dilmahtea.me). If the service is configured to run on a new subdomain, then we have to update the redirect rule in Cloudflare to exclude the new subdomain. Read the [How to Fix Subdomains Redirecting to the Apex Domain (Dilmahtea.me)](/docs/faqs/how-to-fix-subdomains-redirecting-to-dilmahtea.me) FAQ guide to learn more.

## Done

That's it! You have successfully started a new service on the Hetzner VPS. Congratulations! 🎉
