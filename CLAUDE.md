# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repository is a **Quartz 4** static site generator that publishes an Obsidian vault as a website. The vault contains TSSR (Technicien Supérieur Systèmes et Réseaux) apprenticeship notes written in French, covering IT infrastructure, networking, scripting, and system administration.

Site title: **TSSR - Gautier**

## Commands

```bash
# Build the site
npx quartz build

# Build and serve with live reload (development)
npx quartz build --serve

# Build from the docs/ directory instead
npm run docs

# Type-check and format-check
npm run check

# Auto-format with Prettier
npm run format

# Run tests
npm run test
```

Node ≥ 22 and npm ≥ 10.9.2 are required.

## Architecture

**Two-layer structure:**

1. **Quartz framework** (`quartz/`) — the SSG engine (upstream, not modified directly). Contains the plugin system, components, processors, and CLI.
2. **Vault content** (`content/`) — the Obsidian notes, published as the site.

**Key config files at root:**
- `quartz.config.ts` — site metadata, theme colors, and the plugin pipeline (transformers → filters → emitters)
- `quartz.layout.ts` — page layout composition using Quartz components (sidebar, TOC, breadcrumbs, etc.)

**Plugin pipeline in `quartz.config.ts`:**
- *Transformers*: parse/transform markdown (frontmatter, syntax highlighting, Obsidian-flavored markdown, LaTeX/KaTeX, link crawling)
- *Filters*: `RemoveDrafts()` — files with `draft: "true"` in frontmatter are excluded from the build
- *Emitters*: produce HTML output (content pages, folder pages, tag pages, RSS, sitemap)

**Content structure under `content/`:**
- `_Procédures/Centre de documents/` — step-by-step IT procedures (Ansible, Bash, Cisco, Docker, Linux, PowerShell, Routage, Windows Server, Zabbix, Fog)
- `Essentiels/` — networking fundamentals, subnet calculations, VLAN, Windows commands
- `index.md` — site homepage
- `Dictionnaire.md`, `🧠 Glossaire.md` — terminology references

## Content conventions

- All notes are written in **French**; keep this when editing content. Technical terms and CLI commands stay in English.
- **Internal links**: `[[Note Name]]` — Quartz resolves these with `markdownLinkResolution: "shortest"`
- **Callouts**: `> [!note]+` blocks (collapsible, Obsidian-compatible)
- **Frontmatter**: YAML `---` blocks. Setting `draft: "true"` excludes a file from the build.
- **Embeds**: `![[filename]]` for images or note transclusion

## Ignoring patterns

The build ignores `private/`, `templates/`, and `.obsidian/` directories (configured in `quartz.config.ts` → `ignorePatterns`).
