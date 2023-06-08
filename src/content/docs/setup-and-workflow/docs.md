---
title: Documentation
---

This document describes the workflow for the [DilmahEU Technical Documentation](https://github.com/dilmaheu/docs) project.

## Preface

All of the code we write must be descriptive enough that a programmer/developer can tell what it does after reading it. However, if the code fails to adequately describe the purpose, proper comments should be added.

However, comments and descriptive code aren't enough to explain the functionalities of a larger concept. In such cases, we should document the concept thoroughly.

## Secrets

### Environment Variables

- **GITHUB_TOKEN**

## Setup

Clone the repository to your local machine first.

```bash
git clone https://github.com/dilmaheu/docs/
```

The repository will be cloned into the `docs` directory. Navigate to it.

```bash
cd docs
```

Then install the dependencies using `pnpm`.

```bash
pnpm install
```

## Workflow

All the documentation files are written in _Markdown_ and stored inside the `src/content/docs` directory. We follow the following folder structure for documentation:

- **src/content/docs/basics\*.md**: Basics
- **src/content/docs/guides/\*.md**: Guides
- **src/content/docs/tutorials/\*.md**: Tutorials
- **src/content/docs/setup-and-workflow/\*.md**: Setup & Workflow

Based on the type of documentation you want to create or update, follow the above folder structure. Also, make sure the index is updated.

Once you've made the changes, just commit and push them directly to the `main` branch. The changes will be automatically deployed to the [DilmahEU Technical Documentation](https://docs.dilmahtea.me) website.
