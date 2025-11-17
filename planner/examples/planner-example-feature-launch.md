# Complete Example: Feature Launch Planner (Dual-Output)

This document provides a complete end-to-end walkthrough of how the **Planner Expert Prompt** works with dual outputs (checklist + executor playbook). It demonstrates planning a feature launch from initial vague idea to complete planning artifacts.

You should be able to understand the full workflow from this example alone.

---

## Scenario: Planning a User Authentication Feature Launch

A team wants to add user authentication to their existing web application. They have a vague idea but need to clarify requirements and generate a comprehensive plan.

---

## Phase 0: Preprocessing (Scoping & Ambiguity Resolution)

### User's Initial Input

```text
We need to add user authentication to our web app.
Users should be able to sign up, log in, and reset passwords.
We're using Node.js and Express for our backend.
```

### Preprocessing Expert's Response

The preprocessing expert (`prompts/preprocessing.prompt.md`) first determines the planner type and domain.

**Preprocessing Expert:**

> I've read your initial description. Let me ask some clarifying questions to ensure we have a clear, unambiguous plan.
>
> ### Question 1: Project Type and Domain
>
> **The Ambiguity:** The type of project and domain need to be confirmed.
>
> **Question 1a:** What type of planning are you doing?
>
> **Options:**
> 1. **Software Refactor:** Refactoring existing code
> 2. **Feature Launch:** Building a new feature from scratch ← [USER SELECTS THIS]
> 3. **Documentation Overhaul:** Documentation work
> 4. Other
>
> **Question 1b:** What domain does this belong to?
>
> **Options:**
> 1. **Software:** Software engineering ← [USER SELECTS THIS]
> 2. **Documentation:** Technical docs
> 3. Other

Based on the user's selection (Feature Launch + Software), the preprocessing expert now asks software-specific questions.

**Preprocessing Expert (continued):**

> ### Question 2: Technology Stack Selection
>
> **The Ambiguity:** While you mentioned Node.js and Express, I need to confirm the full tech stack.
>
> **Question:** What is the primary technology stack?
>
> **Options:**
> 1. **Node.js** (npm, package.json, src/) ← [USER SELECTS THIS]
> 2. Python
> 3. Rust
> 4. Go
> 5. Other

> ### Question 3: Authentication Scope
>
> **The Ambiguity:** The authentication methods and scope are unclear.
>
> **Question:** What authentication methods should be supported in the initial version?
>
> **Options:**
> 1. **Email + Password only** (local authentication) ← [USER SELECTS THIS]
> 2. OAuth (Google, GitHub, etc.)
> 3. Email + Password + OAuth
> 4. SSO/SAML
> 5. Other

> ### Question 4: Data Persistence
>
> **The Ambiguity:** How user data should be stored is not specified.
>
> **Question:** For the initial version, how should user data be persisted?
>
> **Options:**
> 1. **PostgreSQL database** ← [USER SELECTS THIS]
> 2. MongoDB
> 3. SQLite
> 4. Cloud database (Firebase, Supabase, etc.)
> 5. Other

> ### Question 5: Definition of Done
>
> **The Ambiguity:** The completion criteria are not clear.
>
> **Question:** What does "done" mean for this feature?
>
> **Options:**
> 1. **Backend API only** (signup, login, password reset endpoints)
> 2. Backend API + Frontend UI
> 3. Backend + Frontend + E2E tests ← [USER SELECTS THIS]
> 4. Full production deployment with monitoring
> 5. Other

### Preprocessing Output 1: FINALIZED_PROJECT_BRIEF.md

After receiving all answers, the preprocessing expert generates:

```markdown
# FINALIZED PROJECT BRIEF: User Authentication Feature

## Project Type and Domain
- **Planner Type:** feature_launch
- **Domain:** software

## Original Description
We need to add user authentication to our web app.
Users should be able to sign up, log in, and reset passwords.
We're using Node.js and Express for our backend.

## Clarified Requirements

### Tech Stack
- **Language:** Node.js / JavaScript/TypeScript
- **Framework:** Express
- **Database:** PostgreSQL
- **Package Manager:** npm
- **Dependency File:** package.json
- **Source Folder:** src/
- **Test Folder:** tests/

### Authentication Scope
- **Methods:** Email + Password (local authentication only)
- **Features:**
  - User signup (create account)
  - User login (authenticate existing users)
  - Password reset (forgot password flow via email)
- **Out of Scope for v1:**
  - OAuth providers
  - SSO/SAML
  - Two-factor authentication
  - Social login

### Data Persistence
- **Database:** PostgreSQL
- **User Schema (minimum):**
  - id (UUID primary key)
  - email (unique, indexed)
  - password_hash (bcrypt)
  - created_at, updated_at
  - password_reset_token (nullable)
  - password_reset_expires (nullable)

### Definition of Done
This feature is complete when:
- Backend API endpoints are implemented and tested:
  - POST /auth/signup
  - POST /auth/login
  - POST /auth/forgot-password
  - POST /auth/reset-password
- Frontend UI is implemented for all flows
- End-to-end tests cover happy paths and error cases
- All tests pass (unit, integration, E2E)
- Documentation is updated (API docs, user guide)

### Constraints
- Must not break existing API routes
- Passwords must be hashed (bcrypt with salt rounds >= 10)
- Password reset tokens must expire after 1 hour
- Session management via JWT tokens (httpOnly cookies)

### Non-Functional Requirements
- Login endpoint must respond in < 200ms (p95)
- Signup endpoint must respond in < 500ms (p95)
- Password reset emails must be sent within 5 seconds
```

### Preprocessing Output 2: profile.yaml

```yaml
# profile.yaml - Generated from preprocessing
baseProfile: "profile-feature-launch.yaml"

overrides:
  # Tech stack specifics
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

  # Framework and DB
  framework: "Express"
  db: "PostgreSQL"

  # Output config
  detailLevel: "standard"  # Balanced granularity

  # Execution
  riskTolerance: "medium"
  requiresFeatureFlags: true  # Gradual rollout for auth feature
```

---

## Phase 1: Main Planner Generates Dual Outputs

The main planner (`prompts/main.prompt.md`) receives:
- `FINALIZED_PROJECT_BRIEF.md`
- `profile.yaml` (merged with `profile-feature-launch.yaml` and `default-config.yaml`)

The effective configuration after merging is:

```yaml
plannerType: feature_launch
domain: software
techStack: "Node.js"
language: "JavaScript/TypeScript"
framework: "Express"
db: "PostgreSQL"
phases:
  - requirements
  - scaffolding
  - implementation
  - testing
  - documentation
  - rollout
outputTemplate: "project-checklist-template.md"
playbookTemplate: "project-executor-playbook-template.md"
detailLevel: "standard"
riskTolerance: "medium"
requiresFeatureFlags: true
```

The planner generates TWO artifacts:

---

### Output 1: PROJECT_CHECKLIST.md

```markdown
# User Authentication Feature - Implementation Checklist

**Purpose**: This file tracks implementation progress for the user authentication feature.

**Status**: Phase 0 complete, plan generated

---

## Quick Start for New Sessions

**Essential Context Documents**:
1. **`FINALIZED_PROJECT_BRIEF.md`** - Requirements, scope, tech decisions
2. **`PROJECT_CHECKLIST.md`** (This file) - Implementation plan and status
3. **`PROJECT_EXECUTION_PLAYBOOK.md`** - Execution guidance

---

## Project Configuration

**Tech Stack:** Node.js
**Language:** JavaScript/TypeScript
**Framework:** Express
**Database:** PostgreSQL
**Package Manager:** npm
**Dependency File:** package.json
**Source Folder:** src/
**Entrypoint File:** src/index.js
**Test Folder:** tests/

**Commands:**
- **Build:** `npm run build`
- **Run:** `npm start`
- **Test:** `npm test`

---

## Target Directory Structure

```
project-root/
├── src/
│   ├── auth/
│   │   ├── auth.controller.js      # Auth endpoints
│   │   ├── auth.service.js         # Business logic
│   │   ├── auth.middleware.js      # JWT verification
│   │   └── auth.validators.js      # Input validation
│   ├── models/
│   │   └── User.js                 # User model (Sequelize/TypeORM)
│   ├── utils/
│   │   ├── bcrypt.js               # Password hashing
│   │   ├── jwt.js                  # JWT token generation
│   │   └── email.js                # Email sending (password reset)
│   └── index.js                    # App entry point
├── tests/
│   ├── unit/
│   │   └── auth.service.test.js
│   ├── integration/
│   │   └── auth.routes.test.js
│   └── e2e/
│       └── auth.flow.test.js
├── migrations/
│   └── 001_create_users_table.sql
├── frontend/
│   ├── components/
│   │   ├── SignupForm.jsx
│   │   ├── LoginForm.jsx
│   │   ├── ForgotPasswordForm.jsx
│   │   └── ResetPasswordForm.jsx
│   └── pages/
│       ├── signup.jsx
│       ├── login.jsx
│       └── reset-password.jsx
└── docs/
    └── API.md
```

---

## Phase 1 - Requirements

**Goal:** Clarify API contracts, define data models, establish success metrics

- [x] Confirm API endpoint contracts (request/response formats)
- [x] Define User database schema (see FINALIZED_PROJECT_BRIEF.md)
- [x] Establish success metrics (response times, test coverage)
- [x] Identify integration points with existing codebase

**Implementation Notes:**
- Completed during Phase 0 preprocessing
- See FINALIZED_PROJECT_BRIEF.md for full requirements

---

## Phase 2 - Scaffolding

**Goal:** Set up project structure, dependencies, and database

- [ ] Task 2.1: Create `src/auth/` directory structure
- [ ] Task 2.2: Add dependencies to package.json (bcrypt, jsonwebtoken, nodemailer, pg/sequelize)
- [ ] Task 2.3: Create database migration for users table (migrations/001_create_users_table.sql)
- [ ] Task 2.4: Set up User model in src/models/User.js
- [ ] Task 2.5: Configure JWT secret in environment variables (.env file)
- [ ] Task 2.6: Set up email sending configuration (SMTP or email service)
- [ ] Task 2.7: Create auth router skeleton in src/auth/auth.controller.js
- [ ] Task 2.8: Run migration and verify users table exists

**Implementation Notes:**
- **Context Files**: package.json, src/models/User.js, migrations/001_create_users_table.sql
- **Key Lines**: [To be filled during implementation]

---

## Phase 3 - Implementation

**Goal:** Implement authentication business logic and API endpoints

- [ ] Task 3.1: Implement password hashing utility (src/utils/bcrypt.js)
- [ ] Task 3.2: Implement JWT token generation and verification (src/utils/jwt.js)
- [ ] Task 3.3: Implement signup service (src/auth/auth.service.js - createUser)
- [ ] Task 3.4: Implement POST /auth/signup endpoint (src/auth/auth.controller.js)
- [ ] Task 3.5: Implement login service (src/auth/auth.service.js - authenticateUser)
- [ ] Task 3.6: Implement POST /auth/login endpoint (src/auth/auth.controller.js)
- [ ] Task 3.7: Implement forgot password service (generate reset token, send email)
- [ ] Task 3.8: Implement POST /auth/forgot-password endpoint
- [ ] Task 3.9: Implement reset password service (verify token, update password)
- [ ] Task 3.10: Implement POST /auth/reset-password endpoint
- [ ] Task 3.11: Implement JWT verification middleware (src/auth/auth.middleware.js)
- [ ] Task 3.12: Add input validation for all endpoints (src/auth/auth.validators.js)
- [ ] Task 3.13: Implement frontend SignupForm component
- [ ] Task 3.14: Implement frontend LoginForm component
- [ ] Task 3.15: Implement frontend ForgotPasswordForm component
- [ ] Task 3.16: Implement frontend ResetPasswordForm component
- [ ] Task 3.17: Wire frontend forms to API endpoints

**Implementation Notes:**
- **Context Files**: src/auth/auth.service.js, src/auth/auth.controller.js, frontend/components/
- **Key Lines**: [To be filled during implementation]

---

## Phase 4 - Testing

**Goal:** Write and run unit, integration, and E2E tests

- [ ] Task 4.1: Write unit tests for password hashing utility
- [ ] Task 4.2: Write unit tests for JWT token generation/verification
- [ ] Task 4.3: Write unit tests for auth.service.js (createUser, authenticateUser)
- [ ] Task 4.4: Write integration tests for POST /auth/signup
- [ ] Task 4.5: Write integration tests for POST /auth/login
- [ ] Task 4.6: Write integration tests for POST /auth/forgot-password
- [ ] Task 4.7: Write integration tests for POST /auth/reset-password
- [ ] Task 4.8: Write E2E test for full signup → login flow
- [ ] Task 4.9: Write E2E test for forgot password → reset password flow
- [ ] Task 4.10: Run all tests and ensure 100% pass rate
- [ ] Task 4.11: Verify test coverage meets project standards (>80%)

**Implementation Notes:**
- **Context Files**: tests/unit/, tests/integration/, tests/e2e/
- **Key Lines**: [To be filled during implementation]

---

## Phase 5 - Documentation

**Goal:** Update API docs and user guides

- [ ] Task 5.1: Document POST /auth/signup endpoint in docs/API.md
- [ ] Task 5.2: Document POST /auth/login endpoint in docs/API.md
- [ ] Task 5.3: Document POST /auth/forgot-password endpoint in docs/API.md
- [ ] Task 5.4: Document POST /auth/reset-password endpoint in docs/API.md
- [ ] Task 5.5: Write user guide for signup process
- [ ] Task 5.6: Write user guide for login process
- [ ] Task 5.7: Write user guide for password reset process
- [ ] Task 5.8: Update main README.md with authentication feature description

**Implementation Notes:**
- **Context Files**: docs/API.md, docs/USER_GUIDE.md, README.md
- **Key Lines**: [To be filled during implementation]

---

## Phase 6 - Rollout

**Goal:** Deploy feature with monitoring and gradual rollout

- [ ] Task 6.1: Add feature flag for authentication feature
- [ ] Task 6.2: Deploy to staging environment
- [ ] Task 6.3: Run smoke tests in staging
- [ ] Task 6.4: Enable feature flag for internal users (10%)
- [ ] Task 6.5: Monitor error rates, response times, and user feedback
- [ ] Task 6.6: Enable feature flag for 50% of users
- [ ] Task 6.7: Monitor for 24 hours, check for issues
- [ ] Task 6.8: Enable feature flag for 100% of users
- [ ] Task 6.9: Remove feature flag code after stable rollout
- [ ] Task 6.10: Document rollout completion and metrics

**Implementation Notes:**
- **Context Files**: [To be filled during implementation]
- **Key Lines**: [To be filled during implementation]

---

## Notes & Observations

(Use this section to capture insights, blockers, or decisions made during implementation)

---

## Definition of Done

This feature is complete when:
- All tasks in all phases are checked off
- All tests pass (unit, integration, E2E)
- API endpoints work as specified in the brief
- Frontend UI is functional and connected to backend
- Documentation is complete and up-to-date
- Feature is rolled out to 100% of users
- Monitoring shows no errors or performance degradation
```

---

### Output 2: PROJECT_EXECUTION_PLAYBOOK.md

```markdown
# User Authentication Feature - Execution Playbook

**Purpose**: Detailed execution instructions for completing `PROJECT_CHECKLIST.md`.

**Version**: 1.0
**Related Artifacts**: `FINALIZED_PROJECT_BRIEF.md`, `PROJECT_CHECKLIST.md`

---

## Role of the Executor

You are an **Executor Expert** responsible for:
- Turning `PROJECT_CHECKLIST.md` into completed work
- Keeping the checklist updated after each task
- Ensuring code quality, testing, and security standards are met
- Escalating blockers when they arise

**Key Principle**: The checklist is the SOURCE OF TRUTH for progress.

---

## Input Artifacts

Read and understand these documents:

1. **`FINALIZED_PROJECT_BRIEF.md`** - Requirements and constraints
2. **`PROJECT_CHECKLIST.md`** - Implementation plan and status
3. **`PROJECT_EXECUTION_PLAYBOOK.md`** (This file) - How to execute

---

## General Execution Strategy

### Task Completion Workflow

For each task:
1. Read the task from PROJECT_CHECKLIST.md
2. Execute the task (code, test, document)
3. Run tests: `npm test`
4. Update PROJECT_CHECKLIST.md:
   - Mark task [x]
   - Add implementation notes (files changed, key lines)
5. Commit changes with clear commit message

### Quality Gates

Before marking a task complete:
- Tests pass (npm test)
- No linting errors
- Security: Passwords are hashed, tokens expire, no secrets in code
- Documentation updated for user-facing changes

---

## Jira-Style Ticket Mapping

Map checklist to Jira tickets:

- **Epic** = Phase (e.g., "EPIC: Phase 3 - Implementation")
- **Story** = Logical feature chunk (e.g., "STORY: FEAT - Signup Flow")
- **Task** = Individual checklist item (e.g., "TASK: Implement POST /auth/signup")

### Example Mapping

**Phase 3: Implementation**
- **Epic**: EPIC: Phase 3 - Implementation
- **Story 1**: STORY: FEAT - Signup Flow
  - TASK: Implement password hashing utility
  - TASK: Implement signup service
  - TASK: Implement POST /auth/signup endpoint
  - TASK: Implement frontend SignupForm
- **Story 2**: STORY: FEAT - Login Flow
  - TASK: Implement JWT token generation
  - TASK: Implement login service
  - TASK: Implement POST /auth/login endpoint
  - TASK: Implement frontend LoginForm

---

## Phase-by-Phase Guidance

### Phase: Requirements

**Goal**: Validate and confirm all requirements

**Approach**:
- Review FINALIZED_PROJECT_BRIEF.md
- Confirm API contracts match team expectations
- Validate database schema design

**Task Pattern**:
1. Read requirement from brief
2. Validate with team if unclear
3. Document decision

**Key Outputs**: Confirmed requirements (completed in preprocessing)

---

### Phase: Scaffolding

**Goal**: Set up project structure and dependencies

**Approach**:
- Create directory structure first (empty files OK)
- Add dependencies to package.json
- Run database migrations
- Verify environment configuration

**Task Pattern**:
```bash
1. Create directories: mkdir -p src/auth src/models src/utils
2. Add npm packages: npm install bcrypt jsonwebtoken nodemailer pg
3. Create migration file: touch migrations/001_create_users_table.sql
4. Run migration: npm run migrate
5. Verify: Check DB for users table
6. Update checklist
```

**Common Pitfalls**:
- Forgetting to add .env to .gitignore
- Not verifying database connection before running migrations

**Key Outputs**:
- Directory structure matches Target Directory Structure
- All dependencies installed
- Database schema created

---

### Phase: Implementation

**Goal**: Build authentication logic and UI

**Approach**:
- Implement backend first (API endpoints)
- Then implement frontend (UI components)
- Use feature flags to toggle new code paths
- Test each endpoint as you build it

**Task Pattern for Backend**:
```javascript
1. Create service function (src/auth/auth.service.js)
2. Add validation logic
3. Create controller endpoint (src/auth/auth.controller.js)
4. Wire up route in Express app
5. Manual test with curl/Postman
6. Update checklist
```

**Task Pattern for Frontend**:
```jsx
1. Create component (e.g., SignupForm.jsx)
2. Add form fields and validation
3. Wire to backend API endpoint
4. Test in browser
5. Update checklist
```

**Security Checklist**:
- [ ] Passwords hashed with bcrypt (salt rounds >= 10)
- [ ] JWT tokens stored in httpOnly cookies
- [ ] Password reset tokens expire after 1 hour
- [ ] Input validation on all endpoints
- [ ] No secrets in code (use .env)

**Key Outputs**:
- Working API endpoints
- Functional frontend UI
- Feature flag controls new code

---

### Phase: Testing

**Goal**: Ensure all code is tested and passing

**Approach**:
- Write unit tests for utilities and services
- Write integration tests for API endpoints
- Write E2E tests for user flows
- Aim for >80% test coverage

**Task Pattern**:
```javascript
1. Create test file (e.g., tests/unit/auth.service.test.js)
2. Write test cases (happy path + error cases)
3. Run tests: npm test
4. Fix any failures
5. Check coverage: npm run coverage
6. Update checklist
```

**Test Pyramid**:
- **Unit Tests** (many): Test individual functions in isolation
- **Integration Tests** (some): Test API endpoints with real DB
- **E2E Tests** (few): Test full user flows in browser

**Key Outputs**:
- All tests passing
- >80% code coverage
- E2E tests cover critical flows

---

### Phase: Documentation

**Goal**: Document APIs and user workflows

**Approach**:
- Update API docs for each endpoint
- Write user guides for signup, login, password reset
- Update main README with feature description

**Task Pattern**:
```markdown
1. Open docs/API.md
2. Add endpoint section:
   - Method and path
   - Request body schema
   - Response schema
   - Error codes
3. Add code examples (curl, JavaScript)
4. Update checklist
```

**Key Outputs**:
- Complete API documentation
- User guides for all flows
- Updated README

---

### Phase: Rollout

**Goal**: Deploy feature safely with monitoring

**Approach**:
- Use feature flags for gradual rollout
- Deploy to staging first
- Monitor error rates and performance
- Roll out incrementally (10% → 50% → 100%)

**Task Pattern**:
```bash
1. Enable feature flag in code
2. Deploy to staging: npm run deploy:staging
3. Run smoke tests
4. Enable flag for 10% of users
5. Monitor for 2 hours
6. If stable, increase to 50%
7. Monitor for 24 hours
8. If stable, increase to 100%
9. Remove feature flag after 1 week
10. Update checklist
```

**Monitoring Checklist**:
- [ ] Error rate < 0.1%
- [ ] Login endpoint p95 < 200ms
- [ ] Signup endpoint p95 < 500ms
- [ ] No user complaints or support tickets

**Rollback Plan**:
If errors spike:
1. Disable feature flag immediately
2. Investigate root cause
3. Fix issue
4. Redeploy and re-enable

**Key Outputs**:
- Feature deployed to production
- Monitoring shows stable performance
- Feature flag removed

---

## Quality Standards

### Code Quality
- Tests pass: `npm test` returns success
- No linting errors: `npm run lint`
- Code follows project style guide
- Security best practices followed (see Implementation phase)

### Testing Standards
- Unit tests for all services and utilities
- Integration tests for all API endpoints
- E2E tests for critical user flows
- Test coverage >80%

### Documentation Standards
- API docs include request/response schemas and examples
- User guides are clear and include screenshots
- README updated with feature description

### Security Standards
- Passwords hashed with bcrypt (>= 10 rounds)
- JWT tokens in httpOnly cookies
- Reset tokens expire after 1 hour
- Input validation on all endpoints
- No secrets in code

---

## Handling Blockers

If blocked:
1. Document in "Notes & Observations" in checklist
2. Assess: Can you work around it?
3. Escalate if you need clarification
4. Don't guess on security or architecture decisions

---

## Session Handoff Checklist

At end of session:
- [ ] All completed tasks marked [x]
- [ ] Implementation notes added
- [ ] Tests passing
- [ ] Status field updated
- [ ] Blockers documented
- [ ] Code committed

---

## Definition of Done

Complete when:
- All checklist tasks marked [x]
- All tests pass
- API endpoints work per spec
- Frontend UI functional
- Documentation complete
- Feature rolled out to 100%
- Monitoring shows no issues

---

## Tips for Success

1. Test early and often (don't wait until Phase 4)
2. Commit after each task
3. Security first: Never skip password hashing or token expiration
4. Ask questions if requirements are unclear
5. Follow the checklist - no scope creep

**Remember**: Brief + Checklist + Playbook = Your three sources of truth.
```

---

## Summary

This example demonstrates:

1. **Preprocessing (Phase 0)**: Determines plannerType and domain, asks conditional questions, outputs finalized brief + profile
2. **Main Planner (Phase 1)**: Generates BOTH:
   - `PROJECT_CHECKLIST.md` - WHAT to do (phased tasks)
   - `PROJECT_EXECUTION_PLAYBOOK.md` - HOW to do it (execution guidance)
3. **Dual Outputs Work Together**: Checklist tracks progress, playbook explains execution patterns
4. **Jira Mapping**: Optional ticket structure (Epic=Phase, Story=Feature, Task=Checklist item)
5. **Quality Standards**: Phase-specific guidance ensures consistent, high-quality output

The planner is now fully modular, supports multiple planner types (refactor, feature launch, docs, etc.), and provides both a progress-tracking checklist and detailed execution guidance.
