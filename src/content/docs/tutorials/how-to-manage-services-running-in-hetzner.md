---
title: How to Manage Services Running in Hetzner
---

We run multiple services in Hetzner, and we use [PM2](https://pm2.keymetrics.io/) to manage them. Below are the services we run in Hetzner:

| Name                 | PM2 ID     | URL                          |
| -------------------- | ---------- | ---------------------------- |
| CMS (Production)     | strapi     | https://cms.dilmahtea.me     |
| CMS (Development)    | strapi-dev | https://dev.cms.dilmahtea.me |
| Dilmahtea.me Scripts | scripts    | https://scripts.dilmahtea.me |

## Useful Commands

- `pm2 start <pm2-id>` - Start or restart a service. Omit the `<pm2-id>` to start/restart all services.
- `pm2 stop all` - Stop all services.
- `pm2 stop <pm2-id>` - Stop a service.
- `pm2 list` - List all services
- `pm2 logs <pm2-id>` - View logs of a service. Omit the `<pm2-id>` to view logs of all services.
- `pm2 flush <pm2-id>` - Flush all logs. Omit the `<pm2-id>` to flush logs of all services.
- `pm2 monit` - Monitor all services

For the full list of commands, please refer to the [PM2 documentation](https://pm2.keymetrics.io/docs/usage/pm2-doc-single-page/).

## Configuration

All the services are configured in the `/home/strapi/ecosystem.config.js` file. Please refer to the [PM2 Configuration File documentation](https://pm2.keymetrics.io/docs/usage/application-declaration/) for more information on how to configure services.
