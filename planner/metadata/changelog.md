# Planner Expert: Changelog

This document tracks the evolution of the Planner Expert prompt system.

---

## [2.0.0] - 2025-11-17

### Major Architectural Overhaul

This release represents a complete refactor of the planner expert to support multiple domains, planner types, and dual-output generation.

### Added

#### Core Configuration Layer
- **plannerType support**: Now supports multiple planning scenarios:
  - `software_refactor` - Refactoring existing code
  - `feature_launch` - Launching new features/products
  - `docs_overhaul` - Documentation migrations/rewrites
  - `data_migration` - Data migration between systems
  - `process_change` - Organizational process changes
  - `incident_remediation` - Incident fixes and prevention
  - `experiment` - Experiments and POCs
  - `generic_planner` - Fallback for other scenarios

- **domain support**: Supports multiple domains:
  - `software` - Software engineering projects
  - `docs` - Documentation projects
  - `ops` - Operations and infrastructure
  - `process` - Process design and change management
  - `experiments` - Research and experiments

- **Variables system**: Comprehensive variable reference documentation
  - Core configuration variables (plannerType, domain, phases)
  - Tech stack variables (when domain=software)
  - Domain-specific variables (docs, migrations, etc.)
  - Output configuration (templates, detail level)
  - Risk and execution configuration

- **Default configuration**: `variables/default-config.yaml` provides expert-wide defaults

#### Profiles
- **Profile system**: Three initial profiles for common scenarios:
  - `profile-software-refactor.yaml` - For refactoring projects
  - `profile-feature-launch.yaml` - For new features/products
  - `profile-docs-overhaul.yaml` - For documentation work

- **Profile merging**: Profiles can now reference base profiles with overrides:
  ```yaml
  baseProfile: "profile-software-refactor.yaml"
  overrides:
    techStack: "Python"
    framework: "FastAPI"
  ```

#### Dual-Output Generation
- **PROJECT_EXECUTION_PLAYBOOK.md**: New output artifact providing:
  - Role of the executor
  - General execution strategy
  - Phase-by-phase execution guidance
  - Jira-style ticket mapping (Epic, Story, Task)
  - Quality standards by domain
  - Session handoff checklists

- **Jira-style planning**: Executor playbook includes mapping to Jira tickets:
  - Epic = Phase (e.g., "EPIC: Phase 3 - Implementation")
  - Story = Logical feature grouping
  - Task = Individual checklist item

#### Preprocessing Enhancements
- **Conditional question blocks**: Preprocessing now asks different questions based on:
  - `domain=software` → Tech stack questions
  - `domain=docs` → Documentation framework and audience questions
  - `plannerType=software_refactor` → Architecture and feature flag questions
  - And more...

- **plannerType detection**: First determines what type of planning is needed before asking detailed questions

#### Main Prompt Enhancements
- **plannerType-aware planning**: Adapts planning approach based on scenario:
  - Refactors: Incremental changes, feature flags, rollback strategies
  - Feature launches: Integration points, validation, staged rollout
  - Docs: Content quality, review processes, publishing

- **Phase configuration**: Uses `{phases}` from profile to structure checklist:
  - Software refactor: analysis → scaffolding → extraction → integration → cleanup
  - Feature launch: requirements → scaffolding → implementation → testing → docs → rollout
  - Docs: audit → architecture → migration → authoring → review → deployment

#### Templates
- **project-executor-playbook-template.md**: Complete template for executor playbooks with:
  - Phase-by-phase guidance sections
  - Jira mapping examples
  - Quality standards
  - Handling blockers
  - Session handoff checklist

#### Documentation
- **variables-reference.md**: Complete reference for all configuration variables
- **planner-example-feature-launch.md**: End-to-end example showing:
  - Preprocessing with conditional questions
  - Profile generation with overrides
  - Dual-output generation (checklist + playbook)
  - Jira-style ticket mapping

#### Metadata
- **prompt-metadata.json**: Complete metadata including:
  - Supported planner types and domains
  - Model compatibility
  - Configuration structure
  - Usage notes

- **changelog.md**: This file

### Changed

#### Preprocessing (Phase 0)
- **From**: Tech-stack-only focus
- **To**: Multi-domain, plannerType-aware scoping
- **Impact**: Asks relevant questions based on project type and domain

#### Main Prompt (Phase 1)
- **From**: Single output (PROJECT_CHECKLIST.md)
- **To**: Dual outputs (PROJECT_CHECKLIST.md + PROJECT_EXECUTION_PLAYBOOK.md)
- **Impact**: Provides both WHAT (checklist) and HOW (playbook)

#### Configuration
- **From**: Hardcoded tech stack assumptions
- **To**: Profile-driven configuration with variables
- **Impact**: Supports any tech stack, framework, or domain

#### Phasing
- **From**: Fixed phases (scaffolding → implementation → testing → etc.)
- **To**: Configurable phases via `{phases}` variable
- **Impact**: Different planner types can have different phase structures

### Improved

- **Modularity**: Clear separation between:
  - Defaults (default-config.yaml)
  - Base profiles (profile-*.yaml)
  - Instance-specific overrides (profile.yaml from preprocessing)

- **Documentation**: Comprehensive documentation for:
  - All variables
  - Profile structure
  - Prompt assembly
  - Usage patterns

- **Examples**: Complete end-to-end example showing full workflow

- **Consistency**: All prompts and templates reference documented variables

### Migration Guide (1.x → 2.0)

If upgrading from version 1.x:

1. **Update preprocessing calls**: Preprocessing now determines plannerType and domain first
2. **Use new profile format**: Profiles now include plannerType and domain
3. **Expect dual outputs**: Main planner generates both checklist and playbook
4. **Reference variables**: Use variables-reference.md for configuration options

---

## [1.0.0] - 2024-11-16

### Initial Release

- **Two-phase design**:
  - Phase 0 (Preprocessing): Ambiguity resolution
  - Phase 1 (Main): Checklist generation

- **Tech stack support**:
  - Node.js
  - Python
  - Rust
  - Go

- **Single output**: PROJECT_CHECKLIST.md

- **Templates**: project-checklist-template.md

- **Features**:
  - Multiple-choice clarifying questions
  - Tech stack selection
  - Fixed phasing (scaffolding → implementation → testing → deployment)
  - Abstract placeholders ({sourceFolder}, {dependencyFile}, etc.)

---

## Versioning Strategy

This project follows [Semantic Versioning](https://semver.org/):

- **MAJOR** version when incompatible API changes (e.g., prompt structure, output format)
- **MINOR** version when functionality is added in a backward-compatible manner
- **PATCH** version when backward-compatible bug fixes

### What Triggers Version Bumps

- **MAJOR**:
  - Changes to prompt assembly process
  - Changes to output artifact structure (checklist/playbook templates)
  - Removal of supported planner types or domains
  - Breaking changes to profile format

- **MINOR**:
  - Addition of new planner types or domains
  - Addition of new configuration variables
  - Addition of new base profiles
  - New features in preprocessing or main prompts

- **PATCH**:
  - Bug fixes in prompt logic
  - Documentation improvements
  - Template formatting fixes
  - Variable reference clarifications

---

## Future Roadmap

### Under Consideration

- **Additional planner types**:
  - `security_audit` - Security assessment planning
  - `performance_optimization` - Performance improvement planning
  - `tech_debt_remediation` - Technical debt cleanup planning

- **Additional domains**:
  - `design` - UI/UX design projects
  - `data` - Data engineering and analytics

- **Advanced features**:
  - Dependency graphs between tasks
  - Effort estimation per task
  - Risk assessment per phase
  - Automated postprocessing validation

- **Output formats**:
  - JSON export of checklist
  - Jira/Linear import format
  - Gantt chart generation

### Not Planned

- Language-specific code generation (keep abstract)
- Automated execution (planner only plans, doesn't execute)
- Integration with specific project management tools (keep tool-agnostic)
