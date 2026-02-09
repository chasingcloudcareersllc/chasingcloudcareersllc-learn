# Chasing Cloud Careers Learning Paths

Learning path content for [Chasing Cloud Careers](https://chasingcloudcareersllc.github.io). Documents are written in Markdown with YAML frontmatter and fetched at build time by the main website repository.

## Adding Content

Create a new `.md` file inside a section directory (e.g., `foundations/linux/`).

### Required Frontmatter

```yaml
---
title: "Page Title"
description: "A brief description of this page."
position: 1
icon: "compass"
---
```

- **title**: Lesson display name
- **description**: Short summary shown in sidebar and cards
- **position**: Sort order within the path (lower = first)
- **icon**: Icon name from the website's lucide-react registry

## Structure

```
foundations/                          # Learning path (position 1)
  _path.md
  getting-started/getting-started.md
  introduction-to-computers/introduction-to-computers.md
  os-fundamentals/os-fundamentals.md
  linux/linux.md
  text-editing/text-editing.md
  shell-scripting/shell-scripting.md
  programming/programming.md
  version-control/version-control.md
  networking-fundamentals/networking-fundamentals.md
  api-fundamentals/api-fundamentals.md
  security-fundamentals/security-fundamentals.md
  cicd/cicd.md
  containers/containers.md
  container-orchestration/container-orchestration.md
  iac/iac.md

first-principles/                    # Learning path (position 2)
  _path.md
  getting-started/getting-started.md
  how-computers-work/how-computers-work.md
  operating-systems-and-linux/operating-systems-and-linux.md
  text-processing-and-automation/text-processing-and-automation.md
  programming-fundamentals/programming-fundamentals.md
  data-structures-and-algorithms/data-structures-and-algorithms.md
  networking/networking.md
  data-management/data-management.md
  security-and-cryptography/security-and-cryptography.md
  apis-and-integration/apis-and-integration.md
  software-engineering-and-collaboration/software-engineering-and-collaboration.md
  infrastructure-at-scale/infrastructure-at-scale.md
  theory-of-computation/theory-of-computation.md
```

Each top-level directory is a learning path. Paths are auto-discovered by the website via `_path.md` files. Sections within a path are ordered by their `position` frontmatter field.
