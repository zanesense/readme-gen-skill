---
name: readme-generation
description: "Use this skill whenever the user wants to create, rewrite, improve, audit, or upgrade a README.md file. Trigger on: 'write a README', 'improve my README', 'fix the README', 'add a README', 'make the docs better', 'update the readme', 'my README is missing X', 'generate documentation for this repo', 'README looks bad', or any time the user shares a repo or codebase and asks for documentation help. This skill produces polished, developer-friendly, GitHub-ready README files with badges, quick start, architecture, usage, API docs, deployment, troubleshooting, security, and contribution sections. Always use this skill even for quick README requests — the quality bar matters."
---

# README Generation Skill

Generate a high-quality `README.md` that helps a visitor understand what the project does, why it matters, how to run it, how to use it, how it works internally, and how to contribute.

---

## Step 1: Determine Mode

**Creating from scratch**: user has a repo but no README, or a stub.
**Improving existing**: user has a README that needs polish, missing sections, or a full rewrite.
**Compact mode**: user wants a minimal README (quick start focus only). Skip architecture, API, roadmap, and deployment unless clearly relevant.

When in doubt, ask: "Should this be a full README or a shorter quick-start focused one?"

---

## Step 2: Discover the Repository

Before writing, read the repo. Never invent facts.

### Files to read (in priority order)

| File | What to extract |
| --- | --- |
| `package.json` / `pyproject.toml` / `Cargo.toml` / `go.mod` | Project name, description, scripts, dependencies, version, engines |
| `README.md` (existing) | Preserve accurate facts, tone, and any custom sections |
| `.env.example` | All configuration variables, their names, and whether they have defaults |
| `Dockerfile` / `docker-compose.yml` | Deployment model, service names, exposed ports |
| `next.config.*`, `vite.config.*`, `webpack.config.*` | Framework, build targets |
| `tsconfig.json` / `jsconfig.json` | Target environment, path aliases |
| `.github/workflows/` | CI commands, test scripts, deployment triggers |
| `LICENSE` | License name and year |
| `src/`, `app/`, `lib/`, `server/`, `api/` | App type, entry points, domain structure |
| `docs/` | Any supplemental documentation to reference |
| `public/screenshots/` or `docs/screenshots/` | Screenshot files to link |
| `CONTRIBUTING.md`, `SECURITY.md` | Link to them rather than duplicating |
| `Makefile` | Common developer commands |

### When you cannot access the repo

If the user pastes only a description or partial files:
- Ask for `package.json` and `.env.example` at minimum.
- Mark anything unverified with `TODO`.
- Do not invent package names, URLs, scripts, or environment variable names.

### Project type detection

Identify the project type early. It changes which sections matter most.

| Type | Key signals | Priority sections |
| --- | --- | --- |
| Web app / SaaS | `next`, `react`, `vite`, `app/` directory | Screenshots, quick start, configuration, deployment |
| CLI tool | `bin` in package.json, `argparse`, `cobra`, `clap` | Usage examples, flags, install methods |
| Library / SDK | `main`+`module` exports, `peerDependencies` | API reference, installation, usage snippets |
| Backend API | `express`, `fastapi`, `gin`, routes directory | API reference, deployment, auth |
| Monorepo | `packages/`, `apps/`, `turbo.json`, `nx.json` | Structure, per-package links, workspace setup |
| Data / ML project | notebooks, `requirements.txt`, model files | Dataset, training steps, evaluation |
| DevTool / script | standalone scripts, no framework | Purpose, usage, requirements |

---

## Step 3: Choose README Length

**Full README**: library, serious web app, open-source tool, anything meant for public contributors.

**Compact README**: internal tool, prototype, personal project, or when user explicitly asks for short.

Compact keeps: title, tagline, badges, overview, features, quick start, usage, tech stack, license.
Compact drops: architecture diagrams, roadmap, API reference, security, detailed deployment.

---

## Step 4: Write the README

### Structure Template

Use this structure. Remove irrelevant sections. Add project-specific sections when the repo warrants them.

```markdown
# Project Name

> One strong sentence: what it does and who it helps.

<p align="left">
  <!-- Badges -->
</p>

---

## Table of Contents

- [Overview](#overview)
- [Features](#features)
- [How It Works](#how-it-works)
- [Repository Structure](#repository-structure)
- [Tech Stack](#tech-stack)
- [Requirements](#requirements)
- [Installation](#installation)
- [Configuration](#configuration)
- [Usage](#usage)
- [API Reference](#api-reference)
- [Testing](#testing)
- [Deployment](#deployment)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [Security](#security)
- [License](#license)

---

## Overview

Problem, solution, target audience. Three to five sentences max.

## Features

- Specific, concrete feature. Not "powerful" or "easy." What does it actually do?
- Specific, concrete feature.
- Specific, concrete feature.

## How It Works

Mermaid diagram when architecture or data flow is non-obvious. Skip for simple scripts.

## Repository Structure

Key directories only. Annotate purpose.

## Tech Stack

Table: layer, choice, brief reason if non-obvious.

## Requirements

- Runtime version (Node 20+, Python 3.11+, Go 1.22+)
- Package manager
- Required external services or API keys

## Installation

Exact commands from detected package manager and scripts.

## Configuration

Table: variable name, required/optional, description, example value.
Source from .env.example only.

## Usage

CLI: flags table + examples.
Web app: screenshots + workflow description.
Library: code snippets for the main use cases.
API: see API Reference section.

## API Reference

Method, endpoint, purpose, request body, response body, errors.
Use tables for fields. Use code blocks for examples.

## Testing

Exact commands from package.json scripts or Makefile.

## Deployment

Platform-specific steps. Docker, Vercel, Railway, self-hosted.
Only document platforms that have config files in the repo.

## Roadmap

- [ ] Concrete planned improvement
- [ ] Concrete planned improvement

## Contributing

Step-by-step. Fork, branch naming, PR process, code style.
Link to CONTRIBUTING.md if it exists rather than duplicating.

## Security

Honest trust boundary statement. How to report vulnerabilities.
Link to SECURITY.md if it exists.

## License

Name only. Link to LICENSE file.
```

---

## Badges

Use only badges for things that are real and verifiable.

### Badge URL patterns for GitHub-hosted projects

Replace `OWNER` and `REPO` with actual values from the repo.

```html
<!-- Live demo (only if a URL is known) -->
<a href="DEMO_URL"><img alt="Live Demo" src="https://img.shields.io/badge/demo-online-22c55e?style=for-the-badge" /></a>

<!-- License (only if LICENSE file exists) -->
<a href="./LICENSE"><img alt="License" src="https://img.shields.io/github/license/OWNER/REPO?style=for-the-badge" /></a>

<!-- Stars -->
<a href="https://github.com/OWNER/REPO/stargazers"><img alt="Stars" src="https://img.shields.io/github/stars/OWNER/REPO?style=for-the-badge&color=facc15" /></a>

<!-- Last commit -->
<img alt="Last commit" src="https://img.shields.io/github/last-commit/OWNER/REPO?style=for-the-badge" />

<!-- npm version (only if published) -->
<a href="https://npmjs.com/package/PACKAGE_NAME"><img alt="npm" src="https://img.shields.io/npm/v/PACKAGE_NAME?style=for-the-badge&color=cb3837" /></a>

<!-- CI status (only if .github/workflows/ exists) -->
<a href="https://github.com/OWNER/REPO/actions"><img alt="CI" src="https://img.shields.io/github/actions/workflow/status/OWNER/REPO/WORKFLOW_FILE?style=for-the-badge" /></a>

<!-- Language -->
<img alt="TypeScript" src="https://img.shields.io/badge/TypeScript-5.x-3178c6?style=for-the-badge&logo=typescript&logoColor=white" />
```

**Do not add**: badges for things not in the repo, fake test coverage numbers, fake download counts, or placeholder shields that will render as errors.

---

## Section-Specific Guidance

### Tagline

One sentence. Subject + verb + outcome. No adjectives that don't carry information.

Good: `Turn repository context into production-ready documentation.`
Good: `A zero-config CLI for auditing npm dependency licenses.`
Bad: `A powerful and innovative tool for modern developers.`
Bad: `The best way to manage your projects.`

### Features list

Each bullet: one sentence, present tense, describes what the tool does, not what it "supports" abstractly.

Good: `Scans all route handlers and generates an OpenAPI spec in under two seconds.`
Bad: `Powerful API documentation support.`

### How It Works / Architecture

Use Mermaid when there are multiple components, a non-obvious data flow, or a request lifecycle worth showing.

Skip Mermaid for: single-file scripts, simple CRUD apps with no interesting flow, CLI tools where usage examples communicate better.

For app/data flow:
```
flowchart TD
    A[Input] --> B[Processing step]
    B --> C[Output]
```

For request lifecycle:
```
sequenceDiagram
    participant U as User
    participant A as API
    participant D as Database
    U->>A: POST /items
    A->>D: Insert record
    D-->>A: ID
    A-->>U: 201 Created
```

Keep labels short. Do not describe implementation details in the diagram.

### Repository Structure

Show shape, not every file. Aim for 8 to 15 lines. Annotate purpose.

```text
project/
├── app/            # Next.js routes and layouts
├── components/     # Shared UI components
├── lib/            # Core logic and utilities
├── scripts/        # One-off automation
├── public/         # Static assets
└── tests/          # Unit and integration tests
```

For monorepos, show top level and one level into `packages/` or `apps/`.

### Configuration table

Source every variable from `.env.example` or source code `process.env` / `os.environ` usage.

| Variable | Required | Default | Description |
| --- | --- | --- | --- |
| `DATABASE_URL` | Yes | none | PostgreSQL connection string |
| `PORT` | No | `3000` | HTTP server port |

Never invent variable names.

### API Reference

Document only if the project exposes a public API.

For each endpoint:
- Method and path
- One-line purpose
- Request body fields (table)
- Response shape (code block)
- Error codes (table)

```http
POST /api/analyze

Body:
{ "url": "https://..." }

Response 200:
{ "score": 82, "issues": [...] }

Errors:
400 – Missing or invalid url
429 – Rate limit exceeded
```

### Screenshots

Link only if files exist in the repo.

```markdown
![Main dashboard](public/screenshots/dashboard.png)
```

If screenshots would add value but are missing:
```markdown
> TODO: Add screenshots showing the main workflow and result view.
```

### Security section

State only what is actually true about the project.

Honest claims: read-only, local-only, no persistence, input validated, secrets masked.
Do not claim: perfect security, full compliance, no false positives, complete coverage.

For vulnerability reporting, standard template:

```markdown
To report a security vulnerability, please open a [GitHub Security Advisory](https://github.com/OWNER/REPO/security/advisories/new) rather than a public issue.
```

---

## Writing Style

**Avoid:**
- Em dashes. Use commas, colons, or new sentences.
- "Powerful", "innovative", "cutting-edge", "seamless", "robust", "intuitive"
- Passive voice where active is cleaner
- Repeating the project name in every sentence
- Bullet points for everything, including things that read better as prose

**Prefer:**
- Short, declarative sentences
- Tables for structured data (config, API fields, tech stack)
- Code blocks for everything that should be copy-pasted
- Concrete numbers and examples over vague claims
- Present tense for features ("generates", not "can generate")

---

## Improving an Existing README

When rewriting rather than creating:

1. Extract all accurate facts from the existing README first. Do not discard them.
2. Identify what is missing, wrong, or generic.
3. Keep the project's existing branding and voice if it's intentional.
4. Fix broken image paths or mark them TODO.
5. Remove duplicated sections, filler copy, and unsupported claims.
6. Upgrade structure without erasing the original maintainer's intent.

---

## TODO Conventions

Use `TODO` when something should exist but is missing or unknown:

```markdown
> TODO: Add a `LICENSE` file before first public release.
> TODO: Confirm the correct demo URL.
> TODO: Add screenshots for the main workflow.
```

Keep TODOs actionable and limited. Do not litter the README with TODOs for things that are simply optional.

---

## Pre-Delivery Checklist

Before returning the README, verify every item:

**Accuracy**
- [ ] Title matches the repository name
- [ ] All package names, script names, and command names come from actual files
- [ ] Environment variables come from `.env.example` or source code, not invented
- [ ] Screenshot paths point to files that exist (or are marked TODO)
- [ ] Badge URLs contain real `OWNER/REPO` values, not placeholders
- [ ] License name matches the `LICENSE` file

**Quality**
- [ ] Tagline is one sentence, specific, no empty adjectives
- [ ] Each feature bullet describes a concrete behavior
- [ ] Code blocks are copy-pasteable and use the correct language tag
- [ ] Mermaid diagrams use valid syntax
- [ ] Table of contents links match actual heading anchors

**Completeness**
- [ ] Every section referenced in the table of contents exists
- [ ] Configuration section covers all variables in `.env.example`
- [ ] Install and run commands match detected package manager and scripts

**Style**
- [ ] No em dashes
- [ ] No fake or unverifiable badges
- [ ] No generic marketing copy
- [ ] No invented demo URLs, test coverage numbers, or CI links
