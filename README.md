# CIX PPA Documentation

This repository publishes the official CIX PPA user manual as a MkDocs site deployed through GitHub Pages. The full documentation source now lives under `docs/`, with mirrored English and Simplified Chinese content.

## Quick Start

```bash
sudo apt install curl
curl -fsSL https://archive.cixtech.com/cix-repo-community.sh | sudo sh
```

## Documentation

- [Open the published documentation site](https://claystan404.github.io/cix-ppa/)
- Source content in this repository:
  [English](./docs/en/index.md) | [中文](./docs/zh/index.md) | [Site landing page](./docs/index.md)

## Work On The Docs Locally

On Debian sid, prefer the system packages for local preview:

```bash
sudo apt install mkdocs mkdocs-material python3-jieba
mkdocs serve
```

The GitHub Pages workflow installs the pinned Python dependencies from `requirements-docs.txt` and publishes the generated site from `site/`.
