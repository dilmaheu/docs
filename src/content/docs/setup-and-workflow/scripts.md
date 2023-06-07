---
Title: Dilmahtea.me Scripts Repository
---

This document describes the workflow for the [Dilmahtea.me Scripts](https://github.com/dilmaheu/scripts.dilmahtea.me) project which is hosted at [https://scripts.dilmahtea.me](https://scripts.dilmahtea.me).

## Setup

Clone the repository to your local machine first.

```bash
git clone https://github.com/dilmaheu/scripts.dilmahtea.me/
```

The repository will be cloned into the `scripts.dilmahtea.me` directory. Navigate to it.

```bash
cd scripts
```

Then install the dependencies using `pnpm`.

```bash
pnpm install
```

## Workflow

The project is an **Express** server. The index file is `server.js`. Make changes to the project as needed. Once you've made the changes, just commit and push them directly to the `main` branch. The changes will be automatically deployed to the [Dilmahtea.me Scripts](https://scripts.dilmahtea.me) API server.