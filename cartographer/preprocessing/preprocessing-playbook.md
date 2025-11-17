# Pre-Scan Playbook (Step 0)

This playbook describes how the **Pre-Scan Agent** should explore an unknown repository before running the main Project Context Cartographer.

It is the human-readable companion to `prompts/preprocessing.prompt.md` and focuses on:
- which areas to scan,
- what signals to look for,
- and how to turn those signals into a configuration snapshot and candidate profiles.

---

## 1. Goals

The pre-scan should:

1. Detect the **primary stack** (language, framework, package manager).
2. Propose plausible **projectType**, `sourceRootDirs`, `testRootDirs`, and `commonIgnorePatterns`.
3. Identify obvious **secondary languages / subsystems** (frontends, infra, scripts).
4. Choose sensible defaults for `analysisDepth` (light / standard / deep).
5. Suggest up to **3 candidate profiles** with basic reasoning.

The output format is the YAML document described in `prompts/preprocessing.prompt.md` (top-level keys: `config`, `candidateProfiles`, `observations`).

---

## 2. High-level phases

### Phase A — Top-level overview

Objective: understand the overall shape of the repo.

Recommended actions:
- List top-level files and directories (depth 1–2).
- Capture obvious documentation (`README*`, `docs/`).
- Note very large or obviously generated directories (to add to `commonIgnorePatterns` later).

Signals to look for:
- `src/`, `app/`, `lib/`, `services/`, `cmd/`, `pkg/`.
- Test-like directories: `tests/`, `__tests__/`, `spec/`, `e2e/`.
- Infra-like directories: `infra/`, `terraform/`, `k8s/`, `helm/`, `charts/`.
- Monorepo indicators: `apps/`, `packages/`, `libs/`, multiple service folders at the top level.

Typical commands (conceptual):
- `find . -maxdepth 2 -type d`  
- `find . -maxdepth 2 -type f -iname "README*"`

---

### Phase B — Manifest & language detection

Objective: infer **primaryLanguage**, **packageManager**, and candidate **primaryFramework**.

Look for **language manifests**:
- Node.js / TS: `package.json`, lockfiles (`package-lock.json`, `yarn.lock`, `pnpm-lock.yaml`).
- Python: `pyproject.toml`, `requirements*.txt`, `Pipfile`, `setup.py`.
- Java: `pom.xml`, `build.gradle`, `settings.gradle`.
- Go: `go.mod`, `go.sum`.
- Ruby: `Gemfile`, `*.gemspec`.
- .NET: `*.csproj`, `*.sln`.

Then:
- Pick the dominant manifest(s) to propose `primaryLanguage` and `packageManager`.
- Inspect the manifest for obvious frameworks:
  - Node.js: dependencies such as `express`, `fastify`, `nest`, `koa`, etc.
  - Python: `fastapi`, `django`, `flask`, etc.
  - Java: Spring Boot starter deps, etc.
- Record any version hints (e.g. `.nvmrc`, `.python-version`, `engines` section).

---

### Phase C — Project type & root directories

Objective: guess `projectType`, `sourceRootDirs`, and `testRootDirs`.

Signals:
- **Project type**:
  - `routes/`, `controllers/`, `api/`, HTTP frameworks → `web_api` / `microservice`.
  - `cmd/`, `bin/`, CLI frameworks → `cli_tool`.
  - `lib/`, `pkg/` only (no clear runtime) → `library`.
  - multiple apps + infra or many workspaces under `apps/`, `services/`, `packages/`, `libs/` → treat as a **monorepo** but still choose an appropriate `projectType` (often `monolith` or `other`), and explain the monorepo layout in `specialConstraints` and `observations`.
  - heavy worker/queue usage (`cron`, `queue`, `worker`, Celery, Sidekiq) → `worker` / `data_pipeline`.
- **Source roots**:
  - common candidates: `src`, `app`, `lib`, `services`, `cmd`, `pkg`.
  - for Java: `src/main/java`, `src/main/kotlin`.
  - for Go: `cmd/`, `internal/`, `pkg/`.
- **Test roots**:
  - `tests`, `__tests__`, `spec`, `e2e`, `src/test/java`, `src/test/go`.

The pre-scan agent should:
- Propose an **ordered** list for `sourceRootDirs` and `testRootDirs`.
- Keep `observations` concise but explicit about why these roots were chosen.

---

### Phase D — Secondary languages & infrastructure

Objective: identify polyglot setups and infra-only areas.

Look for:
- Frontend folders: `frontend/`, `web/`, `ui/`, `client/` with JS/TS frameworks.
- Infra: Terraform (`*.tf`), Kubernetes (`kustomization.yaml`, `*-deployment.yaml`), Helm (`Chart.yaml`, `values.yaml`).
- Scripting: `scripts/`, `bin/` with shell, Python, etc.

Map these to `secondaryLanguages` entries in the format:
- `TypeScript (frontend in frontend/)`
- `Terraform (infra in infra/)`
- `Shell (automation in bin/)`

If infra dominates and there is little “runtime” code, consider `projectType: infra_only`.

---

### Phase E — Config assembly & profiles

Objective: consolidate findings into `config` and `candidateProfiles`.

Steps:
1. Fill `config` fields with best-effort values (`"unknown"` or `null` when not sure).
2. Determine `analysisDepth`:
   - `light` for tiny repos or quick scans.
   - `standard` for typical services.
   - `deep` for large, multi-language or heavily-instrumented repos.
3. Suggest up to three `candidateProfiles`:
   - Map stack + project type to profile IDs such as:
     - `node-web-api`
     - `python-fastapi-microservice`
     - `java-spring-monolith`
     - `go-cli`
      - `js-monorepo-workspaces`
      - `polyglot-monorepo`
   - Include a brief `reasoning` string citing concrete evidence.
4. Populate `observations` with 3–10 short bullet points that justify the config and profile choices.
   - If the repo is a monorepo, ensure at least one observation summarizes:
     - monorepo tool (Nx/Turbo/lerna/pnpm workspaces/etc.),
     - how apps/services/packages are laid out,
     - and a note that the monorepo output template may be appropriate.

Always follow the **Shared Rules for Project Context Agents**: evidence-first, no secrets, no speculation.
