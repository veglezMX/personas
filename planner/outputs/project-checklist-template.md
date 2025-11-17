# [Project Name] - Implementation Checklist

**Purpose**: This file tracks implementation progress and serves as a session planner for new AI sessions to understand the current project status, essential context, and next steps.

**Status**: (Phase 0 complete, plan generated)

---

## Quick Start for New Sessions

**Essential Context Documents** (A new session MUST read these to understand the project):
1.  **`FINALIZED_PROJECT_BRIEF.md`** - The **Source of Truth** for all project requirements, scope, and technical decisions. (Read this for "WHY")
2.  **`PROJECT_CHECKLIST.md`** (This file) - The master plan and current project status. (Read this for "WHAT" and "WHERE")
3.  [List any other key files defined in the brief, e.g., `ARCHITECTURE.md`, `API_SPEC.md`]

---

## How to Use This Checklist

**This file is the SOURCE OF TRUTH for project progress.**

-   **For AI Sessions:**
    1.  Read this file FIRST to understand the current status.
    2.  Read the `FINALIZED_PROJECT_BRIEF.md` to understand the full context.
    3.  Perform the requested task for the *next* incomplete checklist item.
    4.  After completing a task, you (the AI) MUST update this file. This includes:
        -   Checking the box `[x]` for the completed task
        -   Adding a summary to the `Implementation Notes` section
        -   Updating the "Status" at the top of this file

---

## Project Configuration

**Tech Stack:** `{techStack}`  
**Language:** `{language}`  
**Package Manager:** `{packageManager}`  
**Dependency File:** `{dependencyFile}`  
**Source Folder:** `{sourceFolder}/`  
**Entrypoint File:** `{sourceFolder}/{entrypointFile}`  
**Test Folder:** `{testFolder}/`  

**Commands:**
-   **Build:** `{buildCommand}`
-   **Run:** `{runCommand}`
-   **Test:** `{testCommand}`

---

## Target Directory Structure

(Based on the architecture defined in the `FINALIZED_PROJECT_BRIEF.md`)

---

## Phase 1 - Setup & Scaffolding

**Goal:** Establish the project structure and initialize all necessary files.

-   [ ] Task 1.1: Create the top-level folder structure as defined above or validate its existence
-   [ ] Task 1.2: Initialize `{dependencyFile}` with core dependencies from the brief
-   [ ] Task 1.3: Create `{sourceFolder}/{entrypointFile}` with a basic "Hello World" or minimal setup
-   [ ] Task 1.4: Create `{testFolder}/` directory and add a placeholder test file
-   [ ] Task 1.5: Create `scripts/setup.sh` to automate dependency installation
-   [ ] Task 1.6: Create `scripts/dev.sh` to automate running the application in development mode
-   [ ] Task 1.7: Verify that `{buildCommand}`, `{runCommand}`, and `{testCommand}` all execute successfully

**Implementation Notes:**
-   **Context Files**: `{dependencyFile}`, `{sourceFolder}/{entrypointFile}`, `scripts/setup.sh`, `scripts/dev.sh`
-   **Key Lines**: `{dependencyFile}:1-15`, `scripts/setup.sh:10-25`
-   **Related Documentation**: [Reference sections in FINALIZED_PROJECT_BRIEF.md if applicable]

---

## Phase 2 - [Logical Phase 2 Name, e.g., "API Skeleton & Data Models"]

**Goal:** [Brief description of this phase's objectives]

-   [ ] Task 2.1: [Concrete, actionable task]
-   [ ] Task 2.2: [Concrete, actionable task]
-   [ ] Task 2.3: [Concrete, actionable task]
-   [ ] Task 2.4: [Concrete, actionable task]

**Implementation Notes:**
-   **Context Files**: [List files created/modified in this phase]
-   **Key Lines**: [Reference specific line ranges for understanding this phase]
-   **Related Documentation**: [Reference sections in FINALIZED_PROJECT_BRIEF.md if applicable]

---

## Phase 3 - [Logical Phase 3 Name, e.g., "Core Business Logic"]

**Goal:** [Brief description of this phase's objectives]

-   [ ] Task 3.1: [Concrete, actionable task]
-   [ ] Task 3.2: [Concrete, actionable task]
-   [ ] Task 3.3: [Concrete, actionable task]
-   [ ] Task 3.4: [Concrete, actionable task]

**Implementation Notes:**
-   **Context Files**: [List files created/modified in this phase]
-   **Key Lines**: [Reference specific line ranges for understanding this phase]
-   **Related Documentation**: [Reference sections in FINALIZED_PROJECT_BRIEF.md if applicable]

---

## Phase 4 - [Logical Phase 4 Name, e.g., "Testing & Validation"]

**Goal:** [Brief description of this phase's objectives]

-   [ ] Task 4.1: [Concrete, actionable task]
-   [ ] Task 4.2: [Concrete, actionable task]
-   [ ] Task 4.3: [Concrete, actionable task]

**Implementation Notes:**
-   **Context Files**: [List files created/modified in this phase]
-   **Key Lines**: [Reference specific line ranges for understanding this phase]
-   **Related Documentation**: [Reference sections in FINALIZED_PROJECT_BRIEF.md if applicable]

---

## Phase 5 - [Logical Phase 5 Name, e.g., "Documentation & Deployment"]

**Goal:** [Brief description of this phase's objectives]

-   [ ] Task 5.1: [Concrete, actionable task]
-   [ ] Task 5.2: [Concrete, actionable task]
-   [ ] Task 5.3: [Concrete, actionable task]

**Implementation Notes:**
-   **Context Files**: [List files created/modified in this phase]
-   **Key Lines**: [Reference specific line ranges for understanding this phase]
-   **Related Documentation**: [Reference sections in FINALIZED_PROJECT_BRIEF.md if applicable]

---

(Continue creating phases and tasks as required to fully implement the `FINALIZED_PROJECT_BRIEF.md`)

---

## Notes & Observations

(Use this section to capture insights, blockers, or decisions made during implementation)

---

## Definition of Done

This project is complete when:
-   All tasks in all phases are checked off
-   All tests pass (`{testCommand}` returns success)
-   The application runs successfully (`{runCommand}` works as expected)
-   Documentation is complete and up-to-date
-   [Any other completion criteria from the `FINALIZED_PROJECT_BRIEF.md`]
