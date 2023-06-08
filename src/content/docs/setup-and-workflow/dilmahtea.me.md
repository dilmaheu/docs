---
title: Dilmahtea.me Repository
---

This document describes the workflow for the [Dilmahtea.me](https://github.com/dilmaheu/dilmahtea.me) project.

## Project Structure

- **.gitignore**: Specifies intentionally untracked files to ignore
- **.prettierrc**: Prettier configuration file
- **astro-imagetools.config.mjs**: `astro-imagetools` configuration file
- **astro.config.mjs**: Astro configuration file
- **tailwind.config.mjs**: Tailwind CSS configuration file
- **tsconfig.json**: TypeScript configuration file, _import aliases_ are defined here
- **window.d.ts**: TypeScript declaration file for the extended `window` object
- **.github/workflows/\*.yml**: GitHub Actions workflows
- **public**: Static files
- **public/fonts**: Font files
- **public/scripts/\*.js**: Static client-side script files
- **src**: Source files
- **src/pages/\*\*.astro**: Astro pages
- **src/pages/db/\*\*.ts**: Static file routes; used to generate client-side accessible data
- **src/components/\*.astro**: Astro components
- **src/components/solid/\*.tsx**: Solid JS components
- **src/components/solid/\*.astro**: Astro wrappers for Solid components, used to prune the props before passing them to the Solid components
- **src/components/pages/\*.astro**: Astro components for pages
- **src/components/scripts/\*.astro**: Astro components for generating client-side scripts
- **src/layouts/\*.astro**: Astro layouts
- **src/queries/\*.graphql**: GraphQL queries for fetching data from the CMS
- **src/styles/\*.scss**: Global stylesheets & SASS modules
- **src/stores/\*.(ts|js)**: Server-side stores; used to create easily accessible data sources
- **src/stores/signals/\*.ts**: Solid JS signals; used for sharing data between components
- **src/utils/\*.(ts|js)**: Server-side utilities
- **src/utils/client/\*.ts**: Client-side utilities
- **src/postbuild-integration**: Custom Astro integration code for running post-build tasks
- **src/postbuild-integration/tasks/\*.js**: Post-build tasks

## Setup

Clone the repository to your local machine first.

```bash
git clone https://github.com/dilmaheu/dilmahtea.me/
```

The repository will be cloned into the `dilmahtea.me` directory. Navigate to it.

```bash
cd dilmahtea.me
```

Then install the dependencies using `pnpm`.

```bash
pnpm install
```

## Workflow

The BMS (Branch Modeling System) of the `dilmahtea.me` repository consists of three types of branches: **Production**, **Staging**, and **Feature**. Feature branches are created from the Staging branch.

To start working on a feature, check out the **Staging** branch first. In our case, `dev` is the **Staging** branch.

```bash
git checkout dev
```

Ensure that the `dev` branch is in sync with the remote.

```bash
git pull --all
```

Next, create a **Feature** branch from it. Always prefix the branch name with `feature/`, `bugfix/`, `update/`, `research/`, etc. depending on the type of the story. Assume you want to work on a new feature, for example: Menu.

```bash
git checkout -b feature/menu dev
```

You're now on the `feature/menu` branch. Work on the feature; stage and commit files as needed. When you're finished, push the branch to the remote repository.

```bash
git push origin feature/menu
```

Then open a **PR (Pull Request)** to merge the changes to the `dev` (Staging) branch. If there are any merge conflicts, resolve them. At least one approving review is required to merge.

Once merged, the changes will coexist with other merged changes in the `dev` branch. They will be kept in the Staging environment for testing before being pushed to Production (the `main` branch).

Once the changes have been deployed to Production, delete the `feature/menu` branch to keep the repository clean.

```bash
git branch -d feature/menu

git push origin -d feature/menu
```

Happy Coding :)
