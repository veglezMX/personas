# Planner Expert: Variables Reference

This document defines all configuration variables that can be used in:
- `variables/default-config.yaml` (expert-wide defaults)
- `profiles/profile-*.yaml` (scenario-specific configurations)
- `prompts/preprocessing.prompt.md` (Phase 0 - scoping)
- `prompts/main.prompt.md` (Phase 1 - plan generation)

Variables are organized into categories for clarity.

---

## Core Planner Configuration

### `plannerType`
- **Type:** String (enum)
- **Allowed Values:**
  - `software_refactor` - Refactoring existing software systems
  - `feature_launch` - Launching new features or products
  - `data_migration` - Migrating data between systems
  - `docs_overhaul` - Overhauling documentation systems
  - `process_change` - Changing organizational processes
  - `incident_remediation` - Remediating incidents and preventing recurrence
  - `experiment` - Running experiments or proof-of-concepts
  - `generic_planner` - General-purpose planning (fallback)
- **Where Used:** preprocessing, main, profiles
- **Example:** `plannerType: software_refactor`
- **Classification:** Core
- **Description:** Determines the type of planning scenario and drives conditional question blocks in preprocessing.

### `domain`
- **Type:** String (enum)
- **Allowed Values:**
  - `software` - Software engineering projects
  - `docs` - Documentation projects
  - `ops` - Operations and infrastructure
  - `experiments` - Research and experiments
  - `process` - Process design and change management
- **Where Used:** preprocessing, main, profiles
- **Example:** `domain: software`
- **Classification:** Core
- **Description:** High-level domain category. When `domain=software`, tech-stack questions are asked during preprocessing.

### `phases`
- **Type:** Array of strings
- **Allowed Values:** Any ordered list of phase names (e.g., `["analysis", "scaffolding", "implementation", "testing", "rollout"]`)
- **Where Used:** main, outputs (checklist template)
- **Example:**
  ```yaml
  phases:
    - analysis
    - scaffolding
    - implementation
    - testing
    - rollout
  ```
- **Classification:** Core
- **Description:** Ordered list of phase names that structure the project checklist. Phase 1 should typically be scaffolding/setup for software projects.

---

## Tech Stack Configuration (Software Domain Only)

These variables are only used when `domain=software`.

### `techStack`
- **Type:** String
- **Allowed Values:** Any tech stack name (e.g., "Node.js", "Python", "Rust", "Go", "Java", etc.)
- **Where Used:** preprocessing (tech stack selection), main, profiles
- **Example:** `techStack: "Node.js"`
- **Classification:** Core (software domain)
- **Description:** Human-readable name of the technology stack.

### `language`
- **Type:** String
- **Allowed Values:** Any programming language (e.g., "JavaScript/TypeScript", "Python", "Rust", "Go", "Java", etc.)
- **Where Used:** preprocessing, main, outputs
- **Example:** `language: "JavaScript/TypeScript"`
- **Classification:** Core (software domain)
- **Description:** Programming language(s) used in the project.

### `packageManager`
- **Type:** String
- **Allowed Values:** Any package manager (e.g., "npm", "pip", "cargo", "go mod", "maven", etc.)
- **Where Used:** preprocessing, main, outputs
- **Example:** `packageManager: "npm"`
- **Classification:** Core (software domain)
- **Description:** Package/dependency manager for the tech stack.

### `dependencyFile`
- **Type:** String
- **Allowed Values:** Any dependency file name (e.g., "package.json", "pyproject.toml", "Cargo.toml", "go.mod", etc.)
- **Where Used:** preprocessing, main, outputs
- **Example:** `dependencyFile: "package.json"`
- **Classification:** Core (software domain)
- **Description:** Name of the file that defines project dependencies.

### `sourceFolder`
- **Type:** String
- **Allowed Values:** Any folder path (e.g., "src", "app", "lib", etc.)
- **Where Used:** preprocessing, main, outputs
- **Example:** `sourceFolder: "src"`
- **Classification:** Core (software domain)
- **Description:** Primary folder for source code.

### `entrypointFile`
- **Type:** String
- **Allowed Values:** Any file name (e.g., "index.js", "main.py", "main.rs", "main.go", etc.)
- **Where Used:** preprocessing, main, outputs
- **Example:** `entrypointFile: "index.js"`
- **Classification:** Core (software domain)
- **Description:** Main entry point file for the application.

### `testFolder`
- **Type:** String
- **Allowed Values:** Any folder path (e.g., "tests", "test", "__tests__", etc.)
- **Where Used:** preprocessing, main, outputs
- **Example:** `testFolder: "tests"`
- **Classification:** Core (software domain)
- **Description:** Folder containing test files.

### `buildCommand`
- **Type:** String
- **Allowed Values:** Any shell command (e.g., "npm run build", "cargo build", "make", etc.)
- **Where Used:** preprocessing, main, outputs
- **Example:** `buildCommand: "npm run build"`
- **Classification:** Core (software domain)
- **Description:** Command to build the project.

### `runCommand`
- **Type:** String
- **Allowed Values:** Any shell command (e.g., "npm start", "python -m app.main", "cargo run", etc.)
- **Where Used:** preprocessing, main, outputs
- **Example:** `runCommand: "npm start"`
- **Classification:** Core (software domain)
- **Description:** Command to run the application.

### `testCommand`
- **Type:** String
- **Allowed Values:** Any shell command (e.g., "npm test", "pytest", "cargo test", etc.)
- **Where Used:** preprocessing, main, outputs
- **Example:** `testCommand: "npm test"`
- **Classification:** Core (software domain)
- **Description:** Command to run tests.

---

## Output Configuration

### `outputTemplate`
- **Type:** String (path to template file)
- **Allowed Values:** Any template file in `outputs/` (e.g., "project-checklist-template.md")
- **Where Used:** main, profiles
- **Example:** `outputTemplate: "project-checklist-template.md"`
- **Classification:** Core
- **Description:** Template file to use for generating the PROJECT_CHECKLIST.md output.

### `playbookTemplate`
- **Type:** String (path to template file)
- **Allowed Values:** Any template file in `outputs/` (e.g., "project-executor-playbook-template.md")
- **Where Used:** main, profiles
- **Example:** `playbookTemplate: "project-executor-playbook-template.md"`
- **Classification:** Core
- **Description:** Template file to use for generating the PROJECT_EXECUTION_PLAYBOOK.md output (Jira-style executor instructions).

### `detailLevel`
- **Type:** String (enum)
- **Allowed Values:**
  - `light` - Minimal detail, high-level tasks only
  - `standard` - Balanced detail (default)
  - `high` - Very detailed, granular tasks
  - `deep` - Comprehensive, exhaustive detail
- **Where Used:** main, profiles
- **Example:** `detailLevel: "standard"`
- **Classification:** Core
- **Description:** Level of detail/granularity for generated tasks and checklists.

---

## Execution & Risk Configuration

### `riskTolerance`
- **Type:** String (enum)
- **Allowed Values:**
  - `low` - Very conservative, favor safety over speed
  - `medium` - Balanced approach (default)
  - `high` - More aggressive, favor speed over caution
- **Where Used:** preprocessing, main, profiles
- **Example:** `riskTolerance: "medium"`
- **Classification:** Advanced
- **Description:** Risk tolerance for the project. Affects how changes are staged and rolled out.

### `requiresFeatureFlags`
- **Type:** Boolean
- **Allowed Values:** `true`, `false`
- **Where Used:** main, profiles (especially for refactors)
- **Example:** `requiresFeatureFlags: true`
- **Classification:** Advanced (software domain)
- **Description:** Whether the plan should include feature flag mechanisms for gradual rollout.

### `targetArchitecture`
- **Type:** String
- **Allowed Values:** Any architecture pattern (e.g., "layered", "modular-monolith", "microservices", "event-driven", etc.)
- **Where Used:** preprocessing, main, profiles (especially for refactors)
- **Example:** `targetArchitecture: "layered-modular"`
- **Classification:** Advanced (software domain)
- **Description:** Target architecture pattern for refactors or new projects.

---

## Profile Metadata

### `profileName`
- **Type:** String
- **Allowed Values:** Any descriptive name
- **Where Used:** profiles (metadata only, not used in prompts)
- **Example:** `profileName: "software-refactor-python"`
- **Classification:** Metadata
- **Description:** Human-readable name for the profile.

### `description`
- **Type:** String
- **Allowed Values:** Any description
- **Where Used:** profiles (metadata only)
- **Example:** `description: "Profile for refactoring Python FastAPI services"`
- **Classification:** Metadata
- **Description:** Description of what this profile is for.

---

## Domain-Specific Variables

### Software Refactors
- `framework` (string): Framework being used (e.g., "FastAPI", "Express", "Django")
- `db` (string): Database system (e.g., "PostgreSQL", "MongoDB", "SQLite")

### Data Migrations
- `sourceSystem` (string): Source system for migration
- `targetSystem` (string): Target system for migration
- `dataVolume` (string): Estimated data volume (e.g., "small", "medium", "large")
- `migrationType` (string): Type of migration (e.g., "one-time", "continuous", "staged")

### Documentation Projects
- `docsFramework` (string): Documentation framework (e.g., "MkDocs", "Docusaurus", "Sphinx")
- `targetAudience` (string): Primary audience (e.g., "developers", "end-users", "both")
- `docScope` (string): Scope of docs (e.g., "API reference", "user guides", "tutorials", "comprehensive")

---

## Variable Usage Matrix

| Variable | preprocessing | main | outputs | profiles | Classification |
|----------|---------------|------|---------|----------|----------------|
| `plannerType` | ✓ | ✓ | - | ✓ | Core |
| `domain` | ✓ | ✓ | - | ✓ | Core |
| `phases` | - | ✓ | ✓ | ✓ | Core |
| `techStack` | ✓ | ✓ | ✓ | ✓ | Core (software) |
| `language` | ✓ | ✓ | ✓ | ✓ | Core (software) |
| `packageManager` | ✓ | ✓ | ✓ | ✓ | Core (software) |
| `dependencyFile` | ✓ | ✓ | ✓ | ✓ | Core (software) |
| `sourceFolder` | ✓ | ✓ | ✓ | ✓ | Core (software) |
| `entrypointFile` | ✓ | ✓ | ✓ | ✓ | Core (software) |
| `testFolder` | ✓ | ✓ | ✓ | ✓ | Core (software) |
| `buildCommand` | ✓ | ✓ | ✓ | ✓ | Core (software) |
| `runCommand` | ✓ | ✓ | ✓ | ✓ | Core (software) |
| `testCommand` | ✓ | ✓ | ✓ | ✓ | Core (software) |
| `outputTemplate` | - | ✓ | - | ✓ | Core |
| `playbookTemplate` | - | ✓ | - | ✓ | Core |
| `detailLevel` | ✓ | ✓ | - | ✓ | Core |
| `riskTolerance` | ✓ | ✓ | - | ✓ | Advanced |
| `requiresFeatureFlags` | - | ✓ | - | ✓ | Advanced |
| `targetArchitecture` | ✓ | ✓ | - | ✓ | Advanced |

---

## Best Practices

1. **Defaults First:** Define sensible defaults in `variables/default-config.yaml` for all core variables.
2. **Profile Overrides:** Profiles should only override variables that differ from defaults for that scenario.
3. **Domain Conditional:** Only set tech-stack variables when `domain=software`.
4. **Consistency:** Ensure variables referenced in prompts match the documented names exactly.
5. **Templates:** Always specify both `outputTemplate` and `playbookTemplate` in profiles to ensure dual-output generation.

---

## Example: Minimal Profile

```yaml
# Minimal profile for a software refactor
profileName: "software-refactor-minimal"
description: "Minimal software refactor profile"

# Core config
plannerType: software_refactor
domain: software

# Tech stack (since domain=software)
techStack: "Node.js"
language: "JavaScript/TypeScript"
packageManager: "npm"
dependencyFile: "package.json"
sourceFolder: "src"
entrypointFile: "index.js"
testFolder: "tests"
buildCommand: "npm run build"
runCommand: "npm start"
testCommand: "npm test"

# Output config
outputTemplate: "project-checklist-template.md"
playbookTemplate: "project-executor-playbook-template.md"
detailLevel: "standard"

# Phases
phases:
  - analysis
  - scaffolding
  - extraction
  - integration
  - cleanup
```

---

## Version History

- **v1.0.0** (Initial) - First comprehensive variables reference for modular planner expert system
