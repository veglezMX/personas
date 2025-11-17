# Expert Prompt: Project Planner

## Overview

This expert prompt system is designed to de-risk and plan projects across multiple domains (software, documentation, operations, etc.) through a rigorous two-phase process. It transforms vague ideas and initial project descriptions into concrete, actionable implementation plans with dual outputs: a progress-tracking checklist and detailed execution guidance.

The planner operates in two distinct phases:

1.  **Phase 0 (Preprocessing):** A Senior Systems Analyst determines the project type and domain, then clarifies all ambiguities through conditional questioning (tech stack for software, docs framework for documentation, etc.), forcing clear decisions before planning begins.
2.  **Phase 1 (Main Task):** A Technical Project Manager takes the clarified brief and generates TWO artifacts:
    - **PROJECT_CHECKLIST.md** - A phased implementation checklist tracking WHAT to do
    - **PROJECT_EXECUTION_PLAYBOOK.md** - Detailed execution guidance explaining HOW to do it

This expert is **domain-agnostic**, **plannerType-aware**, and **profile-driven** to support software refactors, feature launches, documentation overhauls, data migrations, process changes, and more.

---

## Key Features

- **Multi-Domain Support:** Works across software, documentation, operations, processes, and more
- **PlannerType-Aware:** Adapts planning approach based on scenario (refactor, feature launch, docs overhaul, etc.)
- **Conditional Questioning:** Asks relevant questions based on domain and planner type
  - Software domain → Tech stack questions
  - Docs domain → Documentation framework and audience questions
  - Refactor type → Architecture and feature flag questions
- **Dual-Output Generation:** Produces both:
  - **Checklist** (WHAT to do) - Progress tracking with phased tasks
  - **Playbook** (HOW to do it) - Execution guidance with Jira mapping
- **Profile-Driven:** Uses base profiles with overrides for maximum flexibility
- **Granular Task Breakdown:** Generates concrete, verifiable tasks appropriate for the detail level
- **Jira-Style Mapping:** Executor playbook includes Epic/Story/Task mapping for team coordination
- **Session-Aware:** Designed for multi-session AI and human collaboration
- **Token-Efficient:** Clean, professional output without unnecessary formatting

---

## Architecture

### Prompts
-   **`prompts/preprocessing.prompt.md`**: Phase 0 ambiguity resolution prompt
    - Determines `plannerType` and `domain`
    - Asks conditional questions based on project type
    - Outputs: `FINALIZED_PROJECT_BRIEF.md` + `profile.yaml`
-   **`prompts/main.prompt.md`**: Phase 1 plan generation prompt
    - Takes finalized brief + profile
    - Generates BOTH checklist and executor playbook
    - Outputs: `PROJECT_CHECKLIST.md` + `PROJECT_EXECUTION_PLAYBOOK.md`
-   **`prompts/shared-rules.md`**: Governance rules applied to all prompts

### Configuration

#### Variables
-   **`variables/variables-reference.md`**: Complete documentation for all configuration variables
-   **`variables/default-config.yaml`**: Expert-wide defaults (plannerType, domain, phases, tech stack, output templates, etc.)

#### Profiles
-   **`profiles/profile-software-refactor.yaml`**: Profile for refactoring projects
-   **`profiles/profile-feature-launch.yaml`**: Profile for new features/products
-   **`profiles/profile-docs-overhaul.yaml`**: Profile for documentation work
-   Profile structure: Base profiles can be referenced with overrides or used standalone

### Outputs (Templates)
-   **`outputs/project-checklist-template.md`**: Template for `PROJECT_CHECKLIST.md` (progress tracking)
-   **`outputs/project-executor-playbook-template.md`**: Template for `PROJECT_EXECUTION_PLAYBOOK.md` (execution guidance)

### Metadata
-   **`metadata/prompt-metadata.json`**: Version tracking, model compatibility, supported planner types/domains
-   **`metadata/changelog.md`**: Version history and migration guides

### Examples
-   **`examples/planner-example-feature-launch.md`**: Complete end-to-end walkthrough showing preprocessing, profile generation, and dual-output generation

---

## Usage

### Quick Start

1.  **Prepare Your Input:** Have an initial project idea, README, or brief ready.
2.  **Run Preprocessing (Phase 0):**
    -   Load `prompts/preprocessing.prompt.md`
    -   Provide your initial project description
    -   Answer questions about plannerType and domain
    -   Answer conditional clarifying questions (tech stack if software, docs framework if docs, etc.)
    -   Receive: `FINALIZED_PROJECT_BRIEF.md` + `profile.yaml`
3.  **Run Main Planner (Phase 1):**
    -   Load `prompts/main.prompt.md`
    -   Provide the `FINALIZED_PROJECT_BRIEF.md` and `profile.yaml`
    -   Receive: `PROJECT_CHECKLIST.md` + `PROJECT_EXECUTION_PLAYBOOK.md`
4.  **Execute:** Use the checklist to track progress and the playbook as your execution guide across multiple sessions.

### Example Workflow

```
User Idea (vague)
    ↓
[Preprocessing Prompt]
├─ Determines plannerType (e.g., feature_launch) and domain (e.g., software)
├─ Asks conditional questions (tech stack, scope, constraints)
└─ Outputs:
    ├─ FINALIZED_PROJECT_BRIEF.md
    └─ profile.yaml (references base profile + overrides)
    ↓
[Main Planner Prompt]
├─ Merges: default-config.yaml + base profile + profile overrides
├─ Reads phases from config (e.g., requirements → scaffolding → implementation → testing → docs → rollout)
└─ Outputs:
    ├─ PROJECT_CHECKLIST.md (WHAT to do: phased task list)
    └─ PROJECT_EXECUTION_PLAYBOOK.md (HOW to do it: execution guidance, Jira mapping)
    ↓
[Execution]
└─ Executor follows playbook, updates checklist after each task
```

### Supported Planner Types

- **software_refactor**: Refactoring existing codebases (analysis → extraction → integration → cleanup)
- **feature_launch**: Launching new features or products (requirements → scaffolding → implementation → testing → docs → rollout)
- **docs_overhaul**: Documentation migrations or rewrites (audit → architecture → migration → authoring → review → deployment)
- **data_migration**: Data migrations between systems
- **process_change**: Organizational process changes
- **incident_remediation**: Incident fixes and prevention
- **experiment**: Experiments and proof-of-concepts
- **generic_planner**: Fallback for other scenarios

### Supported Domains

- **software**: Software engineering, code, APIs, systems
- **docs**: Documentation, technical writing, user guides
- **ops**: Infrastructure, deployments, SRE, monitoring
- **process**: Organizational processes, workflows, procedures
- **experiments**: Research, POCs, experiments

---

## Design Principles

1.  **Ambiguity is the Enemy:** Never proceed with unclear requirements. Use conditional questioning to force decisions early based on plannerType and domain.
2.  **Three Sources of Truth:**
    - `FINALIZED_PROJECT_BRIEF.md` - WHY (requirements, scope, decisions)
    - `PROJECT_CHECKLIST.md` - WHAT (phased task list, progress tracking)
    - `PROJECT_EXECUTION_PLAYBOOK.md` - HOW (execution guidance, quality standards)
3.  **Granular Tasks:** Every task must be concrete, verifiable, and completable in a single session. Granularity adapts to `detailLevel` configuration.
4.  **Profile-Driven Behavior:** All variations (tech stack, phases, detail level, risk tolerance) are controlled by configuration, not by rewriting prompts.
5.  **Conditional Intelligence:** Preprocessing asks different questions based on context:
    - Software domain → Tech stack questions
    - Docs domain → Framework and audience questions
    - Refactor type → Architecture and feature flag questions
6.  **Dual Outputs Work Together:** Checklist and playbook are designed to complement each other, with no contradictions and consistent variable usage.
7.  **Token Efficiency:** No emojis, no icons, no fluff. Clean, machine-readable output optimized for AI consumption and human readability.

