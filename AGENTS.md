# AGENTS.md - Chasing Cloud Careers Learn Repository Guide

## Overview

This is the learning path content repository for [Chasing Cloud Careers](https://chasingcloudcareersllc.github.io). Content is written in Markdown with YAML frontmatter and consumed by the main website at build time.

## Relationship to Website

- Cloned into `content/learn/` of `chasingcloudcareersllc.github.io` during build
- Pushes to `main` trigger a `repository_dispatch` event (`learn-content-updated`) that rebuilds the website
- Automation lives in `.github/workflows/trigger-site-rebuild.yml`

## Structure

```
foundations/                        # Learning path directory (position 1)
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
  api-fundamentals/
    api-fundamentals.md            # Position 10 — icon: plug
  security-fundamentals/
    security-fundamentals.md       # Position 11 — icon: shield
  cicd/
    cicd.md                        # Position 12 — icon: refresh-cw
  containers/
    containers.md                  # Position 13 — icon: container
  container-orchestration/
    container-orchestration.md     # Position 14 — icon: ship
  iac/
    iac.md                         # Position 15 — icon: blocks

first-principles/                   # Learning path directory (position 2)
  _path.md                         # Path metadata (title, description, icon, position)
  getting-started/
    getting-started.md             # Position 1  — icon: compass
  how-computers-work/
    how-computers-work.md          # Position 2  — icon: monitor
  operating-systems-and-linux/
    operating-systems-and-linux.md # Position 3  — icon: cpu
  text-processing-and-automation/
    text-processing-and-automation.md  # Position 4  — icon: pen-tool
  programming-fundamentals/
    programming-fundamentals.md    # Position 5  — icon: code
  data-structures-and-algorithms/
    data-structures-and-algorithms.md  # Position 6  — icon: layers
  networking/
    networking.md                  # Position 7  — icon: network
  data-management/
    data-management.md             # Position 8  — icon: database
  security-and-cryptography/
    security-and-cryptography.md   # Position 9  — icon: shield
  apis-and-integration/
    apis-and-integration.md        # Position 10 — icon: plug
  software-engineering-and-collaboration/
    software-engineering-and-collaboration.md  # Position 11 — icon: git-branch
  infrastructure-at-scale/
    infrastructure-at-scale.md     # Position 12 — icon: blocks
  theory-of-computation/
    theory-of-computation.md       # Position 13 — icon: brain-circuit
```

## Path Metadata (`_path.md`)

One per learning path directory. Controls how the path appears on the `/learn/` index.

```yaml
---
title: "Foundations"
description: "Build the universal foundation for any tech career — 15 sections from how computers work through APIs, security, and infrastructure as code."
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
| `blocks` | Foundations (path), Infrastructure as Code, Infrastructure at Scale |
| `compass` | Getting Started |
| `monitor` | Introduction to Computers, How Computers Work |
| `cpu` | OS Fundamentals, Operating Systems and Linux, First Principles (path) |
| `terminal` | Linux |
| `pen-tool` | Text Editing, Text Processing and Automation |
| `file-code` | Shell Scripting |
| `code` | Programming, Programming Fundamentals |
| `git-branch` | Version Control, Software Engineering and Collaboration |
| `network` | Networking Fundamentals, Networking |
| `refresh-cw` | CI/CD |
| `container` | Containers |
| `ship` | Container Orchestration |
| `plug` | API Fundamentals, APIs and Integration |
| `shield` | Security Fundamentals, Security and Cryptography |
| `layers` | Data Structures and Algorithms |
| `database` | Data Management |
| `brain-circuit` | Theory of Computation |

## Content Conventions

### Foundations Path

- **GFM tables** for reference data and comparisons
- **Code blocks** with language hints (`bash`, `python`, `yaml`, `hcl`, `vim`)
- **Mermaid diagrams** for architecture, workflows, and state machines (fenced with ` ```mermaid `)
- **Blockquote exercises**: `> **Try It**: ...` for hands-on practice prompts
- **Internal cross-references** using absolute paths: `[Linux](/learn/foundations/linux/)`
- **Consistent structure**: Intro → Why It Matters → What You'll Learn → Content Sections → Key Takeaways

### First Principles Path

All Foundations conventions apply, plus:

- **Theory/Practice/Connection pattern**: Each topic section contains `### Theory`, `### Practice`, and `### Connection` subsections that integrate conceptual depth with hands-on work
- **Math (embedded)** callouts: Mathematical concepts taught in context within Theory subsections (e.g., Boolean algebra in digital logic, modular arithmetic in cryptography)
- **Assessment Dimensions**: Each lesson ends with Explain, Build, and Debug subsections before Key Takeaways
- **Exercises section**: Integrative exercises drawn from the syllabus, separate from inline Try It prompts
- **Internal cross-references** using absolute paths: `[Networking](/learn/first-principles/networking/)`
- **Consistent structure**: Intro → Why It Matters → What You'll Learn → Topic Sections (Theory/Practice/Connection) → Exercises → Assessment Dimensions → Key Takeaways → Resources

## Adding a New Lesson

1. Create a directory under the path (e.g., `foundations/new-topic/`)
2. Create a matching markdown file (`foundations/new-topic/new-topic.md`)
3. Add frontmatter with `title`, `description`, `position` (next number in sequence), and `icon`
4. Push to `main` — the website rebuilds automatically

## Adding a New Path

1. Create a top-level directory (e.g., `devops/`)
2. Add `_path.md` with `title`, `description`, `icon`, and `position`
3. Add lesson subdirectories following the pattern above
