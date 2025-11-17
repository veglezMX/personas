# Project Context Cartographer

This folder contains the prompt suite for the **Project Context Cartographer** system, which turns a static codebase into a comprehensive, machine-readable `PROJECT_CONTEXT.md` that downstream AI agents and tools can consume.

The design follows the standard **Task-Specific Expert Prompt** architecture: a pre-scan step proposes configuration, shared rules govern all agents, and the main Cartographer agent produces the final project context document.

## Architecture

This expert prompt package follows the standard modular layout:

### Core Prompts (`prompts/`)

- `prompts/preprocessing.prompt.md` - defines the **Pre-Scan Agent** that runs a lightweight repository reconnaissance and outputs a YAML config block plus candidate profiles and observations.
- `prompts/main.prompt.md` - defines the **Project Context Cartographer** agent that reads the config, explores the repo, and fills an output `PROJECT_CONTEXT.md` template.
- `prompts/shared-rules.md` - **single source of truth** for Shared Rules for Project Context Agents (evidence-first, no secrets, config-driven, deterministic formatting).

### Supporting Components

- `profiles/` - reusable configuration profiles, including:
  - Language-specific profiles (Node.js, Python, Java, Go)
  - Monorepo-aware profiles (`js-monorepo-workspaces`, `polyglot-monorepo`)
  - **Base profiles** (`profile-web-api-base.yaml`) that support profile merging via `baseProfile + overrides` pattern
- `outputs/` - interchangeable `PROJECT_CONTEXT` templates (`standard`, `light`, `deep`, and `monorepo`).
- `variables/` - variable documentation and a baseline `default-config.yaml`.
- `preprocessing/` - pre-scan playbook and command templates.
- `prompts/instruction-playbooks/` - **NEW:** Detailed execution strategies (e.g., `instruction-default.md`) selectable via `instructionPlaybook` config variable
- `postprocessing/` - **NEW:** Validation playbook for quality assurance and optional export to JSON/YAML
- `metadata/` - version metadata, changelog, and compatibility notes.
- `examples/` - example configs and example `PROJECT_CONTEXT` documents for common stacks.

## Intended usage (conceptual quickstart)

1. Run the **Pre-Scan Agent** using `prompts/preprocessing.prompt.md` against a repository to generate:
   - a proposed config block (`config`),
   - candidate profiles,
   - and a brief list of observations.
2. Review and, if needed, adjust the generated config (or map it to a saved profile).
3. Run the **Project Context Cartographer** using `prompts/main.prompt.md`, providing:
   - the refined config block,
   - the target repository,
   - and an appropriate `PROJECT_CONTEXT.md` output template (e.g. `standard` for single services, `monorepo` when analyzing a multi-app monorepo).
4. Use the resulting `PROJECT_CONTEXT.md` as a canonical, machine-readable description of the codebase (or monorepo) for documentation, RAG indexing, and downstream automation.
5. **(Optional)** Run **postprocessing validation** using `postprocessing/postprocessing-playbook.md` to:
   - Validate output quality and template compliance
   - Check for leaked secrets
   - Export to JSON/YAML for downstream tools

---

## New Features (v0.5.0+)

### 1. Output Template Selection via Configuration

Profiles now specify which output template to use via the `outputTemplate` variable:

```yaml
# In profile
outputTemplate: "output-project-context-standard.md"  # or "monorepo", "light", "deep"
```

**Benefits:**
- Profiles automatically select the appropriate template
- No manual template selection needed
- Monorepo profiles auto-use monorepo template

### 2. Instruction Playbooks (Optional Execution Strategies)

New `prompts/instruction-playbooks/` directory contains detailed execution strategies:

- `instruction-default.md` - Default phased execution for most codebases
- Future: `instruction-monorepo.md`, `instruction-polyglot.md`, `instruction-deep-optimized.md`

**Usage:**
```yaml
# In profile
instructionPlaybook: "instruction-default.md"
```

**Benefits:**
- Model-specific optimizations
- Different strategies for different project types
- Keeps main prompt concise while providing detailed guidance

### 3. Postprocessing & Validation

New `postprocessing/` directory provides quality assurance:

**Validation Steps:**
- Template structure compliance
- Content quality checks (no placeholders, evidence-based)
- Security scanning (no leaked secrets)
- Markdown validation
- JSON schema validation

**Optional Exports:**
- JSON export for RAG systems and automation
- YAML export for configuration management
- Summary reports for quick reference

**Usage:**
```bash
# Validate output
python scripts/validate-structure.py PROJECT_CONTEXT.md

# Export to JSON
python scripts/export-to-json.py PROJECT_CONTEXT.md > PROJECT_CONTEXT.json
```

### 4. Profile Merging (baseProfile + overrides)

Profiles can now inherit from base profiles using the `baseProfile` pattern:

**Base Profile Example:**
```yaml
# profile-web-api-base.yaml
profileName: web-api-base
projectType: web_api
# ... common web API settings
```

**Child Profile Example:**
```yaml
# profile-node-express-api.yaml
baseProfile: "profile-web-api-base.yaml"
profileName: node-express-api
primaryLanguage: "Node.js 20"
primaryFramework: "Express 4.19"
# All other values inherited from base
```

**Benefits:**
- Reduce duplication across similar profiles
- Easy to maintain common patterns
- Override only what's different

**Available Base Profiles:**
- `profile-web-api-base.yaml` - Common patterns for all web APIs

---
