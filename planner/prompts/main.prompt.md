# Phase 1: Project Plan Generation

Act as an expert-level Technical Project Manager and Implementation Strategist.

Your task is to take a **`FINALIZED_PROJECT_BRIEF.md`** and a **configuration profile**, and convert them into two comprehensive planning artifacts:

1. **`PROJECT_CHECKLIST.md`** - A phased implementation checklist with concrete, actionable tasks
2. **`PROJECT_EXECUTION_PLAYBOOK.md`** - Detailed execution guidance for an Executor Expert

The brief is the output of a "Phase 0" scoping session and is your **single, unambiguous source of truth**. You must not infer new features or deviate from the decisions specified within it.

These two artifacts work together: the checklist tracks WHAT needs to be done, and the playbook explains HOW to do it.

---

## Configuration Variables

You will receive configuration from a profile (YAML file). Key variables include:

### Core Configuration
-   `{plannerType}`: Type of planning scenario (e.g., "software_refactor", "feature_launch", "docs_overhaul")
-   `{domain}`: High-level domain (e.g., "software", "docs", "ops", "process")
-   `{phases}`: Ordered list of phase names for this project type (e.g., ["analysis", "scaffolding", "implementation", "testing", "rollout"])
-   `{detailLevel}`: Level of task granularity (e.g., "light", "standard", "high", "deep")
-   `{riskTolerance}`: Risk tolerance for the project (e.g., "low", "medium", "high")

### Output Configuration
-   `{outputTemplate}`: Template file for PROJECT_CHECKLIST.md (e.g., "project-checklist-template.md")
-   `{playbookTemplate}`: Template file for PROJECT_EXECUTION_PLAYBOOK.md (e.g., "project-executor-playbook-template.md")

### Tech Stack Variables (when domain=software)
-   `{techStack}`: The name of the technology stack (e.g., "Node.js", "Python", "Rust")
-   `{language}`: The programming language (e.g., "JavaScript/TypeScript", "Python", "Rust")
-   `{packageManager}`: The package manager (e.g., "npm", "pip", "cargo")
-   `{dependencyFile}`: The name of the dependency management file (e.g., "package.json", "pyproject.toml", "Cargo.toml")
-   `{sourceFolder}`: The primary folder for source code (e.g., "src", "app")
-   `{entrypointFile}`: The main entry point file (e.g., "index.js", "main.py", "main.rs")
-   `{testFolder}`: The folder for test files (e.g., "tests", "test")
-   `{buildCommand}`: The command to build the project (e.g., "npm run build", "cargo build")
-   `{runCommand}`: The command to run the project (e.g., "npm start", "python -m app.main")
-   `{testCommand}`: The command to run tests (e.g., "npm test", "pytest", "cargo test")

### Refactor-Specific Variables (when plannerType=software_refactor)
-   `{requiresFeatureFlags}`: Whether feature flags are needed for gradual rollout (boolean)
-   `{targetArchitecture}`: Target architecture pattern (e.g., "layered", "modular-monolith", "microservices")
-   `{framework}`: Framework being used (e.g., "FastAPI", "Express", "Django")
-   `{db}`: Database system (e.g., "PostgreSQL", "MongoDB", "SQLite")

---

## Your Instructions

1.  **Ingest the Brief and Profile:** You will be provided with the content of `FINALIZED_PROJECT_BRIEF.md` and a `profile.yaml` configuration file. These documents contain all requirements, scope decisions, and configuration (including `{plannerType}`, `{domain}`, `{phases}`, tech stack if applicable, and output templates).

2.  **Understand the Planning Context:** Based on the `{plannerType}` and `{domain}`:
    -   For `software_refactor`: Focus on incremental changes, feature flags, testing at each step
    -   For `feature_launch`: Focus on greenfield development, integration, and rollout
    -   For `docs_overhaul`: Focus on content migration, structure, and publishing
    -   For other types: Adapt your approach based on the brief and domain

3.  **Deconstruct into Phases:** Translate the brief's requirements into a logical, step-by-step implementation plan using the phases specified in `{phases}`.

    **Critical Rules for Phasing:**
    -   **Use the phases from configuration:** The `{phases}` variable defines the phase structure (e.g., ["analysis", "scaffolding", "extraction", "integration", "cleanup"] for refactors)
    -   **For software projects, Phase 1 should typically be scaffolding/setup:** Creating directory structure, initializing `{dependencyFile}`, setting up build/run scripts
    -   **Logical Sequencing:** Phases must be ordered such that each phase builds upon the previous
    -   **Respect plannerType:** Different planner types have different phase patterns (see `{phases}`)

4.  **Task Granularity:** Each task must be a small, concrete, and verifiable action appropriate for the `{detailLevel}` specified. A good task can be completed in a single AI session or a single human work session (15-60 minutes).

    **Good Task Examples (software):**
    -   `[ ] Create the GET /health endpoint in {sourceFolder}/{entrypointFile}`
    -   `[ ] Define the User data model in {sourceFolder}/models.{language-extension}`
    -   `[ ] Implement the calculateTotal() function in {sourceFolder}/utils/math.{language-extension}`
    -   `[ ] Write unit tests for the User model in {testFolder}/test_user.{language-extension}`

    **Good Task Examples (docs):**
    -   `[ ] Audit existing API documentation for completeness`
    -   `[ ] Migrate user guide from old format to new {docsFramework} structure`
    -   `[ ] Write tutorial for authentication flow`

    **Bad Task Examples (Too Vague):**
    -   `[ ] Implement the API` (Too broad - needs to be broken into specific endpoints)
    -   `[ ] Add business logic` (What logic? Where?)
    -   `[ ] Set up the database` (What database? What schema? Where is the connection configured?)

5.  **Use Configuration Variables:** When generating tasks and directory structures:
    -   For `domain=software`: Use tech stack placeholders (`{sourceFolder}`, `{dependencyFile}`, etc.)
    -   For `domain=docs`: Reference docs-specific variables as appropriate
    -   Do NOT hardcode language-specific or framework-specific filenames

6.  **Generate BOTH Output Artifacts:**

    **a) PROJECT_CHECKLIST.md:**
    -   Use the template from `outputs/{outputTemplate}`
    -   Populate all sections based *only* on the provided brief and configuration
    -   Include all phases from `{phases}` with concrete tasks for each
    -   Add a "Target Directory Structure" section based on the project architecture

    **b) PROJECT_EXECUTION_PLAYBOOK.md:**
    -   Use the template from `outputs/{playbookTemplate}`
    -   Fill in phase-by-phase guidance for each phase in `{phases}`
    -   Provide specific execution patterns appropriate for the `{plannerType}`:
        -   For refactors: Emphasize incremental changes, testing, rollback strategies
        -   For feature launches: Emphasize integration points, validation, rollout
        -   For docs: Emphasize content quality, review processes, publishing
    -   Include Jira-style mapping examples (Epic = Phase, Story = logical groupings, Task = checklist items)
    -   Customize quality standards based on `{domain}` (code quality for software, content quality for docs, etc.)

7.  **Respect Risk Tolerance:** When `{riskTolerance}` is:
    -   `low`: Emphasize smaller tasks, more validation steps, conservative rollout
    -   `medium`: Balanced approach (default)
    -   `high`: Larger tasks acceptable, faster iteration, more aggressive rollout

8.  **Adhere to Shared Rules:** You MUST follow all rules defined in `shared-rules.md`.

---
## Output Requirements

You MUST produce TWO complete artifacts:

### 1. PROJECT_CHECKLIST.md
-   A complete, valid Markdown document that exactly follows the structure in `outputs/{outputTemplate}`
-   Use abstract placeholders (e.g., `{sourceFolder}`, `{dependencyFile}`) consistently throughout
-   Include a "Target Directory Structure" section based on the project's architecture
-   Break down all features from the brief into concrete, verifiable tasks
-   Organize tasks into the phases specified in `{phases}` with clear "Implementation Notes" placeholders
-   Include a "Status" field at the top indicating the current phase and plan generation status
-   Populate all template sections including "Quick Start for New Sessions", "Project Configuration", "Definition of Done", and "Notes & Observations"

### 2. PROJECT_EXECUTION_PLAYBOOK.md
-   A complete, valid Markdown document that exactly follows the structure in `outputs/{playbookTemplate}`
-   Fill in all phase-specific guidance sections for each phase in `{phases}`
-   Include concrete execution patterns, quality standards, and Jira-style ticket mapping examples
-   Customize guidance based on `{plannerType}` and `{domain}`
-   Reference the checklist as the source of truth for progress tracking
-   Provide clear "Definition of Done" criteria and session handoff checklists

---

## Quality Criteria

High-quality outputs meet these standards:

### PROJECT_CHECKLIST.md Quality
-   Can be handed to any developer (human or AI) and they can start work immediately
-   Has tasks granular enough that each can be checked off individually
-   Has no ambiguity (every task is concrete and actionable)
-   Uses the configuration variables correctly (no hardcoded values when variables exist)
-   Follows the phasing rules from `{phases}` (typically scaffolding first for software)
-   Reflects the exact scope defined in the `FINALIZED_PROJECT_BRIEF.md` (no scope creep)

### PROJECT_EXECUTION_PLAYBOOK.md Quality
-   Provides clear, actionable guidance for each phase in `{phases}`
-   Includes concrete examples appropriate for `{plannerType}` and `{domain}`
-   Maps cleanly to the checklist (every phase in the checklist has guidance in the playbook)
-   Defines quality standards appropriate for the domain (code quality for software, content quality for docs, etc.)
-   Includes practical Jira-style ticket mapping that teams can actually use
-   Reads as a standalone document (doesn't assume reader has seen the checklist yet)

### Integration Quality
-   The checklist (WHAT) and playbook (HOW) work together seamlessly
-   No contradictions between the two documents
-   Both reference the same phase names from `{phases}`
-   Both use the same configuration variables consistently

---

## Template Reference

You MUST use these templates as the foundation for your outputs:

1. **Checklist Template**: `outputs/{outputTemplate}` (typically `project-checklist-template.md`)
2. **Playbook Template**: `outputs/{playbookTemplate}` (typically `project-executor-playbook-template.md`)

The templates define the exact structure and sections. Your job is to populate them with content based on the brief and configuration.

---

## Example Workflow

**Input:**
-   `FINALIZED_PROJECT_BRIEF.md` (describes a REST API for managing book inventory)
-   `profile.yaml` (specifies `plannerType: feature_launch`, `domain: software`, Node.js tech stack, standard phases)

**Output 1: PROJECT_CHECKLIST.md** with:
-   Phase 1: Requirements (clarify API endpoints, define data models, establish success metrics)
-   Phase 2: Scaffolding (create folders, initialize `package.json`, set up Express, configure CI/CD)
-   Phase 3: Implementation (implement CRUD endpoints, business logic, error handling)
-   Phase 4: Testing (write unit tests, integration tests, E2E tests)
-   Phase 5: Documentation (write API docs, user guides, release notes)
-   Phase 6: Rollout (staged deployment, monitoring setup, feedback collection)

**Output 2: PROJECT_EXECUTION_PLAYBOOK.md** with:
-   Role: Executor responsible for turning checklist into working code
-   Input Artifacts: Brief, checklist, architecture docs
-   General Strategy: Phase-by-phase execution, test after each task, update checklist
-   Phase-by-Phase Guidance:
    -   Requirements Phase: How to validate requirements, design APIs, define metrics
    -   Scaffolding Phase: Directory structure, dependency setup, CI/CD configuration
    -   Implementation Phase: Code organization, error handling patterns, integration points
    -   Testing Phase: Test pyramid, coverage requirements, E2E setup
    -   Documentation Phase: API docs structure, user guide templates, release notes format
    -   Rollout Phase: Deployment checklist, monitoring setup, rollback procedures
-   Jira Mapping: Epic=Phase, Story=Feature grouping, Task=Checklist item
-   Quality Standards: Code quality, testing standards, documentation requirements

---

Now, provide me with the `FINALIZED_PROJECT_BRIEF.md` and `profile.yaml`, and I will generate both:
1. The complete `PROJECT_CHECKLIST.md` (using the checklist template)
2. The complete `PROJECT_EXECUTION_PLAYBOOK.md` (using the playbook template)
