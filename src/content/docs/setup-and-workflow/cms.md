---
title: Strapi CMS
---

This document describes the workflow for the [Strapi CMS](https://cms.dilmahtea.me).

> **Note:** This workflow does not apply to content editing.

## Workflow

We have two environments for the Strapi CMS: **Production** and **Development**. They are hosted at the following URLs:

- **Production:** https://cms.dilmahtea.me
- **Development:** https://dev.cms.dilmahtea.me

Other than editing content, all kinds of content-type structure updates, settings/configuration changes, file changes should be done on the **Development** environment first.

When you're working on a new story, import the content from the **Production** environment to the **Development** environment first. Log in to the **Hetzner VPS** and run the following command to navigate to the development CMS directory.

```bash
cd /home/strapi/strapi/dilmahtea.me/strapi/dev
```

Then run the following command to import the content from the **Production** environment.

```bash
import_prod
```

This command will perform all the steps required to import the content & configurations from the production environment. Once the content is imported, you can start working on the story.

Once the changes are ready to be on production, export the changes from the **Development** environment and import them to the **Production** environment.

Then run the following command to export the changes from the development environment.

```bash
export_dev
```

This command will perform all the steps required to export the changes from the development environment, and push them to GitHub. Once the changes are pushed to GitHub, the deploy action will handle the task of importing the changes to the production environment, rebuilding it, and restarting the Strapi server.

Note that the `export_dev` command doesn't export content changes. So, after the action finishes running, and the production server gets started, you have to update the content manually.

> **Note:** The `import_prod` & `export_dev` commands are aliased to multiple commands. You can find them in the `~/.bashrc` file.
