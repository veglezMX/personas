# [Project Name] - Execution Playbook

**Purpose**: This document provides detailed execution instructions for completing the tasks in `PROJECT_CHECKLIST.md`. It serves as the "minimum plan" that both humans and AI use as the source of truth for how to work through the project.

**Version**: 1.0
**Last Updated**: [Auto-generated timestamp]
**Related Artifacts**: `FINALIZED_PROJECT_BRIEF.md`, `PROJECT_CHECKLIST.md`

---

## Role of the Executor

You are an **Executor Expert** responsible for:
- Turning `PROJECT_CHECKLIST.md` into completed work
- Keeping the checklist updated after each task
- Ensuring quality, testing, and documentation standards are met
- Escalating blockers and ambiguities when they arise

**Key Principle**: The checklist is the SOURCE OF TRUTH for progress. Every completed task must be marked `[x]` with implementation notes added.

---

## Input Artifacts

You MUST read and understand these documents before starting work:

1. **`FINALIZED_PROJECT_BRIEF.md`** - The canonical requirements, scope, and technical decisions ("WHY")
2. **`PROJECT_CHECKLIST.md`** - The phased implementation plan and current status ("WHAT" and "WHERE")
3. **`PROJECT_EXECUTION_PLAYBOOK.md`** (This file) - Detailed execution guidance ("HOW")
4. [List any other key documents from the brief, e.g., `ARCHITECTURE.md`, `API_SPEC.md`]

---

## General Execution Strategy

### Working Through the Checklist

1. **Read First**: Start every session by reading:
   - `PROJECT_CHECKLIST.md` (current status)
   - `FINALIZED_PROJECT_BRIEF.md` (context and requirements)
   - This playbook (execution guidance)

2. **Phase-by-Phase**: Work through phases in order:
   - `[Current Phase Name]` → Complete ALL tasks before moving to the next phase
   - Within a phase, work ticket-by-ticket (see Phase-by-Phase Guidance below)

3. **Task Completion Workflow**: For each task:
   ```
   1. Read the task description from PROJECT_CHECKLIST.md
   2. Execute the task (code, test, document)
   3. Run tests (or equivalent validation for the task type)
   4. Update PROJECT_CHECKLIST.md:
      - Mark task as [x] completed
      - Add implementation notes (files changed, key decisions, line numbers)
   5. Commit changes (if using version control)
   ```

4. **Quality Gates**: Before marking a task complete, ensure:
   - Tests pass (for code tasks)
   - Documentation is updated (for API/user-facing changes)
   - No obvious regressions or broken behavior
   - Implementation notes are added to the checklist

5. **Session Handoffs**: At the end of each work session:
   - Update `PROJECT_CHECKLIST.md` status
   - Add observations or blockers to "Notes & Observations" section
   - Ensure the next session can pick up seamlessly

---

## Jira-Style Ticket Mapping (Optional)

If you want to track work using Jira-style tickets (Epics, Stories, Tasks), here's a suggested mapping:

### Structure
- **Epic** = Phase (e.g., "Phase 1: Scaffolding")
- **Story** = User-visible chunk or logical grouping of related tasks (e.g., "Set up API skeleton")
- **Task** = Individual checklist items (e.g., "Create GET /health endpoint")

### Naming Conventions
- **Epic**: `EPIC: [Phase Name]`
  - Example: `EPIC: Phase 2 - API & Data Models`
- **Story**: `STORY: [Feature/Component]`
  - Example: `STORY: FEAT - Implement User Authentication`
- **Task**: `TASK: [Specific Action]`
  - Example: `TASK: Create User model in {sourceFolder}/models`

### Ticket Flow
1. Create Epic for each phase
2. Group checklist tasks into Stories (2-5 tasks per story typically)
3. Link individual checklist tasks to Stories as sub-tasks
4. Update ticket status as you mark tasks `[x]` in the checklist

**Note**: The `PROJECT_CHECKLIST.md` remains the single source of truth; tickets are just a tracking layer.

---

## Phase-by-Phase Guidance

This section provides execution guidance for each phase defined in your project.

---

### Phase: [Phase 1 Name - e.g., "Analysis"]

**Goal**: [Brief description of this phase's objectives from the checklist]

**General Approach**:
- [How to interpret tasks in this phase]
- [What "done" looks like for this phase]
- [Common pitfalls or things to watch for]

**Task Execution Pattern**:
```
For each task in this phase:
1. [Specific step 1 for this phase type]
2. [Specific step 2 for this phase type]
3. [Validation/testing approach]
4. [Update checklist with findings]
```

**Example Jira Mapping**:
- **Epic**: `EPIC: Phase 1 - [Phase Name]`
- **Stories**: Group tasks like:
  - `STORY: [Logical grouping 1]` → Tasks [1.1, 1.2, 1.3]
  - `STORY: [Logical grouping 2]` → Tasks [1.4, 1.5]

**Key Outputs**:
- [What artifacts or decisions should exist after this phase]

---

### Phase: [Phase 2 Name - e.g., "Scaffolding"]

**Goal**: [Brief description of this phase's objectives from the checklist]

**General Approach**:
- [How to interpret tasks in this phase]
- [What "done" looks like for this phase]
- [Common pitfalls or things to watch for]

**Task Execution Pattern**:
```
For each task in this phase:
1. [Specific step 1 for this phase type]
2. [Specific step 2 for this phase type]
3. [Validation/testing approach]
4. [Update checklist]
```

**Example Jira Mapping**:
- **Epic**: `EPIC: Phase 2 - [Phase Name]`
- **Stories**: Group tasks like:
  - `STORY: [Logical grouping 1]` → Tasks [2.1, 2.2]
  - `STORY: [Logical grouping 2]` → Tasks [2.3, 2.4, 2.5]

**Key Outputs**:
- [What artifacts or decisions should exist after this phase]

---

### Phase: [Phase 3 Name - e.g., "Implementation"]

**Goal**: [Brief description of this phase's objectives from the checklist]

**General Approach**:
- [How to interpret tasks in this phase]
- [What "done" looks like for this phase]
- [Common pitfalls or things to watch for]

**Task Execution Pattern**:
```
For each task in this phase:
1. [Specific step 1 for this phase type]
2. [Specific step 2 for this phase type]
3. [Validation/testing approach]
4. [Update checklist]
```

**Example Jira Mapping**:
- **Epic**: `EPIC: Phase 3 - [Phase Name]`
- **Stories**: Group tasks like:
  - `STORY: [Logical grouping 1]` → Tasks [3.1, 3.2, 3.3]
  - `STORY: [Logical grouping 2]` → Tasks [3.4, 3.5]

**Key Outputs**:
- [What artifacts or decisions should exist after this phase]

---

### Phase: [Phase 4 Name - e.g., "Testing"]

**Goal**: [Brief description of this phase's objectives from the checklist]

**General Approach**:
- [How to interpret tasks in this phase]
- [What "done" looks like for this phase]
- [Common pitfalls or things to watch for]

**Task Execution Pattern**:
```
For each task in this phase:
1. [Specific step 1 for this phase type]
2. [Specific step 2 for this phase type]
3. [Validation/testing approach]
4. [Update checklist]
```

**Example Jira Mapping**:
- **Epic**: `EPIC: Phase 4 - [Phase Name]`
- **Stories**: Group tasks like:
  - `STORY: [Logical grouping 1]` → Tasks [4.1, 4.2]
  - `STORY: [Logical grouping 2]` → Tasks [4.3, 4.4]

**Key Outputs**:
- [What artifacts or decisions should exist after this phase]

---

### Phase: [Phase 5 Name - e.g., "Rollout"]

**Goal**: [Brief description of this phase's objectives from the checklist]

**General Approach**:
- [How to interpret tasks in this phase]
- [What "done" looks like for this phase]
- [Common pitfalls or things to watch for]

**Task Execution Pattern**:
```
For each task in this phase:
1. [Specific step 1 for this phase type]
2. [Specific step 2 for this phase type]
3. [Validation/testing approach]
4. [Update checklist]
```

**Example Jira Mapping**:
- **Epic**: `EPIC: Phase 5 - [Phase Name]`
- **Stories**: Group tasks like:
  - `STORY: [Logical grouping 1]` → Tasks [5.1, 5.2]
  - `STORY: [Logical grouping 2]` → Tasks [5.3]

**Key Outputs**:
- [What artifacts or decisions should exist after this phase]

---

(Continue for all phases defined in the project)

---

## Quality Standards

All completed work must meet these standards:

### Code Quality (for software projects)
- Tests pass (`{testCommand}` returns success)
- No linting errors
- Code follows project style guidelines
- Complex logic has explanatory comments

### Documentation Quality
- API changes have updated documentation
- User-facing changes have release notes or user guide updates
- Implementation notes in checklist reference specific files and line numbers

### Testing Standards
- Unit tests for new business logic
- Integration tests for API endpoints or module boundaries
- Manual testing for UI changes or user flows

### Commit Standards
- Small, focused commits (one task per commit when possible)
- Clear commit messages referencing task numbers
- No secrets or sensitive data in commits

---

## Handling Blockers and Ambiguities

If you encounter blockers or ambiguities:

1. **Document**: Add to "Notes & Observations" in `PROJECT_CHECKLIST.md`
2. **Assess**: Can you work around it or do you need clarification?
3. **Escalate**: If you need clarification, pause and ask for guidance
4. **Don't Guess**: Never make architectural decisions that contradict the brief

---

## Session Handoff Checklist

At the end of each work session, ensure:

- [ ] All completed tasks are marked `[x]` in `PROJECT_CHECKLIST.md`
- [ ] Implementation notes added for completed tasks
- [ ] Tests passing (if code changes were made)
- [ ] "Status" field at top of checklist updated with current phase
- [ ] Any blockers or observations documented
- [ ] Code committed (if using version control)

---

## Definition of Done

The project is complete when:
- All tasks in all phases are checked off in `PROJECT_CHECKLIST.md`
- All tests pass (`{testCommand}` returns success)
- Application runs successfully (`{runCommand}` works as expected)
- Documentation is complete and up-to-date
- [Any other completion criteria from `FINALIZED_PROJECT_BRIEF.md`]

---

## Tips for Success

1. **Read the Brief**: Always refer back to `FINALIZED_PROJECT_BRIEF.md` when in doubt
2. **Small Steps**: Complete tasks one at a time; don't batch multiple unrelated tasks
3. **Test Early**: Run tests after every change, not just at the end of a phase
4. **Document Decisions**: Add notes to the checklist for non-obvious implementation choices
5. **Stay Focused**: Stick to the plan; avoid scope creep or "nice-to-have" additions
6. **Ask Questions**: If something is ambiguous, ask rather than assume

---

**Remember**: This playbook, the checklist, and the brief are your three sources of truth. Follow them systematically and you'll deliver a high-quality, well-tested implementation.
