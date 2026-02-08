# AGENTS.md - Chasing Cloud Careers Learn Repository Guide

## Overview

This is the learning path content repository for [Chasing Cloud Careers](https://chasingcloudcareersllc.github.io). Content is written in Markdown with YAML frontmatter and consumed by the main website at build time.

## Relationship to Website

- Cloned into `content/learn/` of `chasingcloudcareersllc.github.io` during build
- Pushes to `main` trigger a `repository_dispatch` event (`learn-content-updated`) that rebuilds the website
- Automation lives in `.github/workflows/trigger-site-rebuild.yml`

## Structure

```
foundations/                        # Learning path directory
  _path.md                         # Path metadata (title, description, icon, position)
  getting-started/
    getting-started.md             # Position 1  — icon: compass
  introduction-to-computers/
    introduction-to-computers.md   # Position 2  — icon: monitor
  os-fundamentals/
    os-fundamentals.md             # Position 3  — icon: cpu
  linux/
    linux.md                       # Position 4  — icon: terminal
  text-editing/
    text-editing.md                # Position 5  — icon: pen-tool
  shell-scripting/
    shell-scripting.md             # Position 6  — icon: file-code
  programming/
    programming.md                 # Position 7  — icon: code
  version-control/
    version-control.md             # Position 8  — icon: git-branch
  networking-fundamentals/
    networking-fundamentals.md     # Position 9  — icon: network
  cicd/
    cicd.md                        # Position 10 — icon: refresh-cw
  containers/
    containers.md                  # Position 11 — icon: container
  container-orchestration/
    container-orchestration.md     # Position 12 — icon: ship
  iac/
    iac.md                         # Position 13 — icon: blocks
```

## Path Metadata (`_path.md`)

One per learning path directory. Controls how the path appears on the `/learn/` index.

```yaml
---
title: "Foundations"
description: "Build the universal foundation for any tech career — from how computers work through infrastructure as code."
icon: "blocks"
position: 1
---
```

## Lesson Frontmatter

Each lesson lives in its own subdirectory. The markdown filename must match the directory name (`{section}/{section}.md`).

```yaml
---
title: "Lesson Title"
description: "Brief description shown in sidebar and cards."
position: 1
icon: "compass"
---
```

- **title** (string, required): Lesson display name
- **description** (string, required): Short summary
- **position** (number, required): Sort order within the path (lower = first)
- **icon** (string, required): Icon name from the website's lucide-react registry

## Icon Names

| Icon | Lessons |
|------|---------|
| `blocks` | Foundations (path), Infrastructure as Code |
| `compass` | Getting Started |
| `monitor` | Introduction to Computers |
| `cpu` | OS Fundamentals |
| `terminal` | Linux |
| `pen-tool` | Text Editing |
| `file-code` | Shell Scripting |
| `code` | Programming |
| `git-branch` | Version Control |
| `network` | Networking Fundamentals |
| `refresh-cw` | CI/CD |
| `container` | Containers |
| `ship` | Container Orchestration |

## Content Conventions

- **GFM tables** for reference data and comparisons
- **Code blocks** with language hints (`bash`, `python`, `yaml`, `hcl`, `vim`)
- **Mermaid diagrams** for architecture, workflows, and state machines (fenced with ` ```mermaid `)
- **Blockquote exercises**: `> **Try It**: ...` for hands-on practice prompts
- **Internal cross-references** using absolute paths: `[Linux](/learn/foundations/linux/)`
- **Consistent structure**: Intro → Why It Matters → What You'll Learn → Content Sections → Key Takeaways

## Adding a New Lesson

1. Create a directory under the path (e.g., `foundations/new-topic/`)
2. Create a matching markdown file (`foundations/new-topic/new-topic.md`)
3. Add frontmatter with `title`, `description`, `position` (next number in sequence), and `icon`
4. Push to `main` — the website rebuilds automatically

## Adding a New Path

1. Create a top-level directory (e.g., `devops/`)
2. Add `_path.md` with `title`, `description`, `icon`, and `position`
3. Add lesson subdirectories following the pattern above
