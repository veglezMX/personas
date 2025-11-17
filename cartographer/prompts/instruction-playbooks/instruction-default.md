# Instruction Playbook: Default Cartographer Execution Strategy

This instruction playbook provides detailed, step-by-step execution guidance for the Project Context Cartographer agent. It supplements the main prompt with concrete execution patterns and best practices.

---

## Purpose

This playbook defines **how** to execute the cartography task using a phased, systematic approach. It is the default execution strategy suitable for most codebases (single repos, microservices, small monorepos).

**When to use this playbook:**
- Single-language or primary-language-dominant codebases
- Standard project types (web APIs, CLI tools, libraries, microservices)
- Analysis depth: light or standard
- Single repository (non-monorepo)

**When to use a different playbook:**
- Large monorepos → use `instruction-monorepo.md` (if available)
- Polyglot codebases with complex secondary languages → use `instruction-polyglot.md` (if available)
- Deep analysis with performance constraints → use `instruction-deep-optimized.md` (if available)

---

## Execution Phases

### Phase 1: Project Identity & Technology Stack

**Objective:** Establish the foundational facts about the repository.

**Execution Steps:**

1. **Read primary manifest files** (in order of priority):
   - `package.json` (Node.js)
   - `pyproject.toml` or `setup.py` (Python)
   - `Cargo.toml` (Rust)
   - `go.mod` (Go)
   - `pom.xml` or `build.gradle` (Java)

2. **Extract core identity:**
   - Project name (from manifest or README)
   - Version (from manifest or git tags)
   - Description (from manifest or README header)
   - Project type (infer from structure + manifest)

3. **Confirm or refine config variables:**
   - Validate `projectType` matches evidence
   - Extract exact versions (runtime, framework)
   - Identify package manager from lockfiles

4. **Detect secondary languages:**
   - Scan for directories matching `secondaryLanguages` config
   - Validate their presence and purpose

**Output:** PROJECT_CONTEXT.md sections:
- `[SECTION:IDENTITY]`
- `[SECTION:TECHNOLOGY_STACK]`

---

### Phase 2: Dependencies & External Integrations

**Objective:** Map all external dependencies and third-party integrations.

**Execution Steps:**

1. **Parse dependency files:**
   - Use appropriate command based on `{packageManager}`:
     - npm/pnpm/yarn → read `package.json` dependencies + devDependencies
     - pip/Poetry → read `pyproject.toml` or `requirements.txt`
     - Maven/Gradle → parse XML/Groovy dependency declarations
     - Cargo → read `Cargo.toml` dependencies
     - go mod → read `go.mod` require statements

2. **Categorize dependencies:**
   - **Core/Framework:** Web frameworks, CLI frameworks, ORMs
   - **Database Clients:** PostgreSQL, MongoDB, Redis clients
   - **External APIs:** SDK clients for third-party services
   - **Infrastructure:** Cloud SDKs, Kubernetes clients
   - **Dev/Build:** Test frameworks, linters, build tools

3. **Detect external integrations:**
   - Search for API clients in code (grep patterns)
   - Check for config files referencing external services
   - Look for environment variable patterns (API keys, endpoints)

4. **Summarize if needed:**
   - If >50 dependencies, group by category
   - List top 10-15 most important by usage
   - Provide counts for each category

**Output:** PROJECT_CONTEXT.md sections:
- `[SECTION:DEPENDENCIES]`
- `[SECTION:EXTERNAL_INTEGRATIONS]`

---

### Phase 3: Codebase Structure & Architecture

**Objective:** Understand the physical and logical organization of code.

**Execution Steps:**

1. **Generate directory tree:**
   - Use `tree` command (if `hasTree: yes`) with appropriate depth
   - For `analysisDepth: light` → depth 2
   - For `analysisDepth: standard` → depth 3
   - For `analysisDepth: deep` → depth 4
   - Respect `commonIgnorePatterns`

2. **Identify architectural patterns:**
   - Look for layered structure (routes/controllers/services/models)
   - Check for domain-driven design (domain/, application/, infrastructure/)
   - Detect modular structure (modules/, features/, components/)
   - Identify special directories (migrations/, scripts/, docs/)

3. **Map key directories:**
   - Use `sourceRootDirs` to locate main code
   - Use `testRootDirs` to locate tests
   - Use pattern variables to find:
     - API routes (`apiDirPatterns`)
     - Data models (`modelDirPatterns`)
     - Database migrations (`migrationDirPatterns`)
     - Infrastructure code (`infraDirPatterns`)

4. **Create architecture diagram (text/ASCII):**
   - Show relationships between major components
   - Indicate data flow direction
   - Mark external boundaries (APIs, databases)

**Output:** PROJECT_CONTEXT.md sections:
- `[SECTION:DIRECTORY_STRUCTURE]`
- `[SECTION:ARCHITECTURE]`

---

### Phase 4: API Surface & Entry Points

**Objective:** Document all public interfaces and entry points.

**Execution Steps:**

1. **Locate entry points:**
   - Find main application files using `entryPointPatterns`
   - Identify CLI entry points (bin/, scripts/)
   - Detect background workers/jobs entry points

2. **For Web APIs:**
   - Scan route/controller directories
   - Extract endpoint definitions (GET/POST/PUT/DELETE + paths)
   - Note authentication/authorization patterns
   - Check for API specification files (`apiSpecPatterns`)

3. **For CLI Tools:**
   - Extract commands and subcommands
   - Identify CLI framework (click, cobra, commander, yargs)
   - Document command hierarchy

4. **For Libraries:**
   - Identify exported modules/functions
   - Check for public API documentation
   - Note package entry points

5. **Sample representative files:**
   - Use `sampleFileCount` to limit samples
   - Show `sampleLineCount` lines from each
   - Focus on entry points and core logic

**Output:** PROJECT_CONTEXT.md sections:
- `[SECTION:API_SURFACE]` or `[SECTION:CLI_INTERFACE]`
- `[SECTION:ENTRY_POINTS]`

---

### Phase 5: Data Models & Database Integration

**Objective:** Document data structures and persistence layer.

**Execution Steps:**

1. **Locate data models:**
   - Search directories matching `modelDirPatterns`
   - Identify ORM models (SQLAlchemy, Sequelize, TypeORM, Entity Framework)
   - Find schema definitions (GraphQL schemas, Pydantic models, JSON schemas)

2. **Extract model structure:**
   - List entity names
   - Show key fields and relationships (foreign keys, associations)
   - Note validation rules

3. **Detect database technology:**
   - Check dependencies for database clients
   - Look for migration directories
   - Find connection configuration patterns

4. **Document migrations:**
   - Count migration files
   - Show migration directory structure
   - Note migration tool (Alembic, Flyway, Liquibase, etc.)

5. **Data flow:**
   - Trace data from API → service → model → database
   - Note caching layers (Redis, Memcached)
   - Identify data transformation points

**Output:** PROJECT_CONTEXT.md sections:
- `[SECTION:DATA_MODELS]`
- `[SECTION:DATABASE]`
- `[SECTION:DATA_FLOW]` (if in template)

---

### Phase 6: Configuration & Environment

**Objective:** Document how the application is configured.

**Execution Steps:**

1. **Find configuration files:**
   - Search for patterns in `configFilePatterns`
   - Locate environment templates using `envTemplatePatterns`
   - Check for framework-specific config (Django settings, Spring application.properties)

2. **Extract environment variables:**
   - Parse `.env.example` or `.env.template`
   - Grep for process.env, os.getenv, System.getenv patterns
   - Document:
     - Variable name
     - Required/optional status
     - Default value (if any)
     - Purpose/description

3. **Configuration strategy:**
   - Note configuration approach (env vars, config files, CLI flags)
   - Document configuration precedence
   - Identify secrets management approach

4. **Deployment configurations:**
   - Check for Docker files
   - Look for Kubernetes manifests (`infraConfigPatterns`)
   - Find CI/CD configs (.github/workflows/, .gitlab-ci.yml)

**Output:** PROJECT_CONTEXT.md sections:
- `[SECTION:CONFIGURATION]`
- `[SECTION:ENVIRONMENT_VARIABLES]`
- `[SECTION:DEPLOYMENT]` (if applicable)

---

### Phase 7: Testing & Quality

**Objective:** Document testing approach and quality measures.

**Execution Steps:**

1. **Locate test files:**
   - Use `testRootDirs` and `testFilePatterns`
   - Count tests by type (unit, integration, E2E)

2. **Identify test framework:**
   - Check dependencies and configs
   - Look for test config files (`testConfigPatterns`)

3. **Test coverage:**
   - Check for coverage configs (`coverageConfigPatterns`)
   - Note coverage targets/thresholds
   - Identify coverage tools (Jest, pytest-cov, Jacoco)

4. **Quality tools:**
   - Linters (ESLint, Pylint, Clippy)
   - Formatters (Prettier, Black, gofmt)
   - Type checkers (TypeScript, mypy, Flow)

5. **CI/CD integration:**
   - Note automated test runs
   - Check for quality gates
   - Identify pre-commit hooks

**Output:** PROJECT_CONTEXT.md sections:
- `[SECTION:TESTING]`
- `[SECTION:QUALITY_TOOLS]`

---

### Phase 8: Observability & Operations

**Objective:** Document logging, monitoring, and operational concerns.

**Execution Steps:**

1. **Logging:**
   - Identify logging libraries using `loggingPatterns`
   - Note log levels and formats
   - Check for structured logging

2. **Monitoring & APM:**
   - Search for APM configs (`apmConfigPatterns`)
   - Detect monitoring integrations (New Relic, Datadog, Sentry)
   - Note metrics collection (Prometheus, StatsD)

3. **Health checks:**
   - Find health/readiness endpoints
   - Check for graceful shutdown handling

4. **Error handling:**
   - Identify error tracking services
   - Note retry/circuit breaker patterns
   - Document error logging strategy

**Output:** PROJECT_CONTEXT.md sections:
- `[SECTION:LOGGING]`
- `[SECTION:MONITORING]`
- `[SECTION:OBSERVABILITY]` (if in template)

---

### Phase 9: Documentation & Known Issues

**Objective:** Aggregate existing documentation and known problems.

**Execution Steps:**

1. **Find documentation:**
   - Read README.md (summarize key sections)
   - Check CHANGELOG using `changelogPatterns`
   - Look for docs/ directory
   - Find API specs (OpenAPI, GraphQL schema)

2. **Known issues:**
   - Search for files matching `knownIssuesPatterns`
   - Check for TODO/FIXME comments in code
   - Scan issue tracker links in README

3. **Contributing guides:**
   - Look for CONTRIBUTING.md
   - Find developer setup instructions
   - Note coding standards

**Output:** PROJECT_CONTEXT.md sections:
- `[SECTION:DOCUMENTATION]`
- `[SECTION:KNOWN_ISSUES]`

---

### Phase 10: Final Validation & Output

**Objective:** Ensure completeness and correctness before delivery.

**Execution Steps:**

1. **Template compliance check:**
   - Verify all sections from output template are present
   - Ensure section tags are preserved
   - Check heading hierarchy is correct

2. **Evidence validation:**
   - All facts are backed by file paths or code samples
   - Counts are accurate
   - No speculation or assumptions without caveats

3. **Formatting:**
   - Valid Markdown
   - Tables properly formatted
   - Code blocks have language tags
   - Links are valid

4. **Final review checklist:**
   - [ ] All template sections present
   - [ ] Facts are evidence-based
   - [ ] File paths are accurate
   - [ ] Versions are exact (not "latest")
   - [ ] No secrets included
   - [ ] Markdown is valid
   - [ ] Template structure unchanged

**Output:** Complete `PROJECT_CONTEXT.md`

---

## Optimization Tips

### For Light Analysis Depth
- Use shallow tree depth (2 levels)
- Limit file sampling to 5 files per category
- Focus on manifest files and entry points only
- Skip detailed dependency categorization

### For Standard Analysis Depth (Default)
- Use moderate tree depth (3 levels)
- Sample 10 representative files
- Categorize top-level dependencies
- Include basic architecture diagram

### For Deep Analysis Depth
- Use deep tree depth (4 levels)
- Sample 15-20 files across different layers
- Fully categorize all dependencies
- Create detailed architecture diagrams
- Include code complexity metrics (if available)

### Performance Considerations
- Cache directory listings between phases
- Reuse grep results across multiple pattern searches
- For large repos (>10k files), prefer targeted searches over full scans
- Use .gitignore patterns to skip unnecessary files

---

## Tool Usage Patterns

### When `hasTree: yes`
```bash
tree -L 3 -I 'node_modules|dist|build|.git' /path/to/repo
```

### When `hasTree: no`
```bash
find . -maxdepth 3 -type d -not -path '*/node_modules/*' -not -path '*/.git/*'
```

### When `hasJq: yes`
```bash
jq '.dependencies' package.json
```

### When `hasJq: no`
```bash
grep -A 50 '"dependencies"' package.json | sed -n '/}/q;p'
```

### When `hasYq: yes`
```bash
yq eval '.services' docker-compose.yml
```

### When `hasYq: no`
```bash
grep -A 20 '^services:' docker-compose.yml
```

---

## Error Handling

### Missing Configuration
- If `projectType: unknown`, infer from structure and manifest
- If `primaryLanguage: unknown`, detect from file extensions
- If patterns are empty, use built-in defaults for language

### Missing Files
- If README.md missing, use manifest description
- If no tests found, note in TESTING section as "No tests detected"
- If no migrations, note in DATABASE section as "No migrations found"

### Access Restrictions
- If file is unreadable, note path but skip contents
- If directory is restricted, note in KNOWN_ISSUES
- Never attempt to bypass security restrictions

---

## Best Practices

1. **Evidence First:** Always cite file paths, line numbers, or code samples
2. **Be Specific:** Use exact versions, not ranges or "latest"
3. **Stay Factual:** Don't speculate; use "unknown" if data is missing
4. **Respect Config:** Use configured patterns, don't invent your own
5. **Follow Template:** Don't add or remove sections from output template
6. **Security Conscious:** Never include secrets, API keys, or credentials
7. **Consistent Formatting:** Use template's exact heading styles
8. **Complete Sections:** Don't leave placeholder text like "[TODO]"

---

This instruction playbook should be used in conjunction with the main Cartographer prompt and shared rules. Deviations from this playbook should only occur when explicitly directed by configuration or when a specialized playbook is selected.
