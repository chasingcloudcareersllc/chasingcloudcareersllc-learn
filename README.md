# Chasing Cloud Careers Learning Paths

Learning path content for [Chasing Cloud Careers](https://chasingcloudcareersllc.github.io). Documents are written in Markdown with YAML frontmatter and fetched at build time by the main website repository.

## Adding Content

Create a new `.md` file inside a section directory (e.g., `getting-started/`).

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
getting-started/
  getting-started.md
```
