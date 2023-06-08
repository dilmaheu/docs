---
title: Dilmahtea.me Workers Repository
---

This document describes the workflow for the [Dilmahtea.me Workers](https://github.com/dilmaheu/dilmahtea.me) project.

## Project Structure

- **.github/workflows/\*.yml**: GitHub Actions workflows
- **.gitignore**: Specifies intentionally untracked files to ignore
- **pnpm-workspace.yaml**: PNPM Workspace configuration file
- **src/utils/\*.js**: Utilities available to all the workers
- **src/workers/\***: Cloudflare Workers projects
- **src/workers/\*/wrangler.toml**: Wrangler configuration file
- **src/workers/\*/src/index.js**: Worker main file
- **src/workers/\*/src/utils/\*.js**: Worker-specific utilities

## Setup

Clone the repository to your local machine first.

```bash
git clone https://github.com/dilmaheu/dilmahtea.me-workers/
```

The repository will be cloned into the `dilmahtea.me-workers` directory. Navigate to it.

```bash
cd dilmahtea.me-workers
```

Then install the dependencies using `pnpm`.

```bash
pnpm install
```

## Workflow

The BMS (Branch Modeling System) of the `dilmahtea.me-workers` repository consists of two types of branches: **Production**, and **Staging**.

To start working on a feature, check out the **Staging** branch first. In this case, `dev` is the **Staging** branch.

```bash
git checkout dev
```

Ensure that the `dev` branch is in sync with the remote.

```bash
git pull --all
```

We don't create **Feature** branches for the `dilmahtea.me-workers` repository. Instead, we work directly on the `dev` branch. It's because we have only two environments for Cloudflare Workers: **Production** and **Development**. So, there's no usefulness in creating **Feature** branches other than keeping the repository clean.

So, we directly work on the `dev` branch. Work on the feature; stage and commit files as needed. If you have to publish the worker to the Development environment, push the changes to the `dev` branch. But if you want to test before committing the changes, use `wrangler` to publish the worker to the development environment manually.

```bash
wrangler publish --env=development
```

Once you've tested the changes, and they're ready to be pushed to Production, push the changes to the `dev` branch. Then open a **PR (Pull Request)** to merge the changes to the `main` (Production) branch. If there are any merge conflicts, resolve them. At least one approving review is required to merge.

Happy Coding :)
