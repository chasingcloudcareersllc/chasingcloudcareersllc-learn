# Chasing Cloud Careers Learning Paths

Learning path content for [Chasing Cloud Careers](https://chasingcloudcareersllc.github.io). Documents are written in Markdown with YAML frontmatter and fetched at build time by the main website repository.

## Adding Content

Create a new `.md` file inside a section directory (e.g., `foundations/linux/`).

### Required Frontmatter

```yaml
---
title: "Page Title"
description: "A brief description of this page."
sidebar_position: 1
---
```

`sidebar_position` controls the ordering in the sidebar navigation. Lower numbers appear first.

## Structure

```
foundations/
  getting-started/getting-started.md
  introduction-to-computers/introduction-to-computers.md
  os-fundamentals/os-fundamentals.md
  linux/linux.md
  text-editing/text-editing.md
  shell-scripting/shell-scripting.md
  programming/programming.md
  version-control/version-control.md
  networking-fundamentals/networking-fundamentals.md
  cicd/cicd.md
  containers/containers.md
  container-orchestration/container-orchestration.md
  iac/iac.md
```

Each top-level directory is a learning path. Sections within a path are ordered by the website's path configuration.
