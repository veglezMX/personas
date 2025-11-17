# Pre-Scan Command Templates

This document defines **example shell command templates** that the Pre-Scan Agent can use during Step 0 (repository reconnaissance).

Unlike Prompt 1A, the pre-scan focuses on:
- light directory overviews,
- manifest & language detection,
- project-type and root-directory inference.

These templates are illustrative; the agent should adapt them to the host environment and avoid failing if specific tools are missing.

---

## 1. Top-level overview

List top-level directories and README-style docs:

```bash
# List directories and files up to depth 2
find . -maxdepth 2 -type d 2>/dev/null | sort
find . -maxdepth 2 -type f -iname "README*" 2>/dev/null | sort
```

Optional tree-based overview (when `tree` is available):

```bash
tree -L 2 2>/dev/null || true
```

---

## 2. Manifest & language detection

Search for common language manifests:

```bash
# Node.js / TypeScript
find . -maxdepth 4 -type f \( -name "package.json" -o -name "pnpm-lock.yaml" -o -name "yarn.lock" -o -name "package-lock.json" \) 2>/dev/null

# Python
find . -maxdepth 4 -type f \( -name "pyproject.toml" -o -name "requirements*.txt" -o -name "Pipfile" -o -name "setup.py" \) 2>/dev/null

# Java
find . -maxdepth 5 -type f \( -name "pom.xml" -o -name "build.gradle" -o -name "settings.gradle" \) 2>/dev/null

# Go
find . -maxdepth 4 -type f -name "go.mod" 2>/dev/null

# Ruby
find . -maxdepth 4 -type f \( -name "Gemfile" -o -name "*.gemspec" \) 2>/dev/null

# .NET
find . -maxdepth 5 -type f \( -name "*.csproj" -o -name "*.sln" \) 2>/dev/null
```

Check for version hints:

```bash
find . -maxdepth 3 -type f \( -name ".nvmrc" -o -name ".node-version" -o -name ".python-version" -o -name ".ruby-version" -o -name ".tool-versions" \) 2>/dev/null
```

Detect monorepo tooling:

```bash
# Common monorepo config files
find . -maxdepth 2 -type f \( \
  -name "pnpm-workspace.yaml" -o \
  -name "lerna.json" -o \
  -name "nx.json" -o \
  -name "turbo.json" \
\) 2>/dev/null
```

---

## 3. Project type & root directory hints

Identify common source and test roots:

```bash
# Candidate source roots
find . -maxdepth 2 -type d \( -name "src" -o -name "app" -o -name "lib" -o -name "services" -o -name "cmd" -o -name "pkg" \) 2>/dev/null

# Candidate test roots
find . -maxdepth 3 -type d \( -name "tests" -o -name "__tests__" -o -name "spec" -o -name "e2e" -o -path "*/src/test/*" \) 2>/dev/null
```

Look for API / web-service markers:

```bash
find . -maxdepth 4 -type d \( -name "routes" -o -name "controllers" -o -name "handlers" -o -name "api" \) 2>/dev/null
```

Look for CLI and worker markers:

```bash
# CLI
find . -maxdepth 4 -type d \( -name "cmd" -o -name "bin" \) 2>/dev/null

# Worker / queue hints
rg -i "cron|queue|worker|sidekiq|celery" . 2>/dev/null || true
```

---

## 4. Secondary languages & infrastructure

Detect likely secondary stacks and infra:

```bash
# Frontend-like directories
find . -maxdepth 3 -type d \( -name "frontend" -o -name "web" -o -name "ui" -o -name "client" \) 2>/dev/null

# Terraform / infra-as-code files
find . -maxdepth 6 -type f -name "*.tf" 2>/dev/null

# Kubernetes / Helm
find . -maxdepth 6 -type f \( -name "kustomization.yaml" -o -name "Chart.yaml" -o -name "values.yaml" -o -name "*-deployment.yaml" -o -name "*-service.yaml" \) 2>/dev/null
```

These findings should be summarized as `secondaryLanguages` entries and/or influence `projectType` when infra dominates.

---

## 5. Safety & constraints

- Do **not** read real `.env` or secret files during pre-scan. Only templates such as `.env.example` or `.env.template`.
- Prefer breadth over depth: the goal is to propose a good configuration snapshot, not to fully document the system.
- All conclusions must be evidence-backed; when uncertain, use `"unknown"` or leave values `null` and explain in `observations`.
