# Variables Reference

This document describes all configuration variables used by the **Project Context Cartographer** suite (Pre-Scan Agent, Prompt 1A, and related command templates).

Variable names in brackets (e.g. `[ProjectType]`) refer to the Version 2 configuration block used inside prompts; the corresponding YAML/JSON config keys are shown in the examples.

---

## Core variables

| Variable                | Type / Allowed Values                                                                                   | Used By                     | Example                                        | Category |
|-------------------------|---------------------------------------------------------------------------------------------------------|-----------------------------|------------------------------------------------|----------|
| `[ProjectType]`         | String enum: `web_api`, `cli_tool`, `library`, `microservice`, `monolith`, `worker`, `data_pipeline`, `infra_only`, `other`, `unknown` | Pre-Scan, 1A, outputs focus | `projectType: web_api`                         | core     |
| `[PrimaryLanguage]`     | String: language + optional runtime version                                                             | Pre-Scan, 1A, command templates | `primaryLanguage: "Node.js 20"`            | core     |
| `[PrimaryFramework]`    | String: main framework(s) or `none` / `unknown`                                                         | Pre-Scan, 1A                | `primaryFramework: "Express 4.19"`             | core     |
| `[PackageManager]`      | String enum: `npm`, `pnpm`, `yarn`, `pip`, `Poetry`, `Maven`, `Gradle`, `Cargo`, `go mod`, `unknown`    | Pre-Scan, 1A, command templates | `packageManager: pnpm`                      | core     |
| `[SourceRootDirs]`      | List of directory names or paths where main code lives (for monorepos, may include `apps/`, `services/`, `packages/`, `libs/`) | Pre-Scan, 1A, file scans    | `sourceRootDirs: ["src", "app"]`               | core     |
| `[TestRootDirs]`        | List of directory names or paths where tests live (for monorepos, often per-project under `apps/` / `services/`) | Pre-Scan, 1A, file scans    | `testRootDirs: ["tests", "src/__tests__"]`     | core     |
| `[CommonIgnorePatterns]`| List of directory names / glob patterns to ignore in all scans                                          | Pre-Scan, 1A, file scans    | `commonIgnorePatterns: ["node_modules","dist"]`| core     |
| `[SecondaryLanguages]`  | List of strings in the format `Language (purpose in path/)`                                             | Pre-Scan, 1A                | `secondaryLanguages: ["Terraform (infra in infra/)"]` | core     |
| `[SpecialConstraints]`  | Free-text notes / constraints                                                                           | Pre-Scan, 1A, outputs notes | `specialConstraints: "redact internal hostnames"` | core     |
| `analysisDepth`         | String enum: `light`, `standard`, `deep`                                                                | Pre-Scan, 1A                | `analysisDepth: standard`                      | core     |
| `outputTemplate`        | String: filename of output template to use from `outputs/`                                              | Pre-Scan, 1A, profiles      | `outputTemplate: "output-project-context-standard.md"` | core     |
| `instructionPlaybook`   | String: filename of instruction playbook from `prompts/instruction-playbooks/` (optional)               | 1A                          | `instructionPlaybook: "instruction-default.md"` | core     |
| `baseProfile`           | String: filename of base profile to inherit from (supports profile merging)                             | Profiles only               | `baseProfile: "profile-web-api-base.yaml"`     | core     |

---

## Depth & sampling variables

| Variable             | Type / Allowed Values                      | Used By          | Example                           | Category |
|----------------------|--------------------------------------------|------------------|-----------------------------------|----------|
| `[ShallowTreeDepth]` | Integer ≥ 1 (default `2`)                  | 1A, tree/overview| `shallowTreeDepth: 2`             | advanced |
| `[DeepTreeDepth]`    | Integer ≥ 1 (default `4`)                  | 1A, deep mapping | `deepTreeDepth: 4`                | advanced |
| `[SampleFileCount]`  | Integer ≥ 1 (default `10`)                 | 1A, sampling     | `sampleFileCount: 10`             | advanced |
| `[SampleLineCount]`  | Integer ≥ 1 (default `50`)                 | 1A, sampling     | `sampleLineCount: 50`             | advanced |

---

## Directory & file pattern variables

| Variable               | Type / Allowed Values                                         | Used By                 | Example                                                     | Category |
|------------------------|---------------------------------------------------------------|-------------------------|-------------------------------------------------------------|----------|
| `[ApiDirPatterns]`     | Comma/array of directory name patterns for API routes        | Pre-Scan, 1A (web APIs) | `apiDirPatterns: ["routes","controllers","handlers","api"]` | advanced |
| `[ModelDirPatterns]`   | Comma/array of directory name patterns for models/entities   | Pre-Scan, 1A            | `modelDirPatterns: ["models","entities","schemas"]`         | advanced |
| `[MigrationDirPatterns]`| Comma/array of directory name patterns for DB migrations    | Pre-Scan, 1A            | `migrationDirPatterns: ["migrations","db/migrate"]`         | advanced |
| `[EntryPointPatterns]` | Comma/array of filename patterns for application entry points| 1A                      | `entryPointPatterns: ["index.js","server.js","main.go"]`    | advanced |
| `[TestFilePatterns]`   | Comma/array of filename patterns for test files              | 1A                      | `testFilePatterns: ["*.test.js","*_test.py"]`               | advanced |
| `[TestConfigPatterns]` | Comma/array of filename patterns for test config files       | 1A                      | `testConfigPatterns: ["pytest.ini","jest.config.js"]`       | advanced |
| `[ApiSpecPatterns]`    | Comma/array of API spec filename patterns                    | 1A                      | `apiSpecPatterns: ["openapi.yaml","swagger.json"]`          | advanced |

---

## Config & documentation pattern variables

| Variable                 | Type / Allowed Values                                  | Used By      | Example                                                          | Category |
|--------------------------|--------------------------------------------------------|--------------|------------------------------------------------------------------|----------|
| `[EnvTemplatePatterns]`  | Comma/array of env template filename patterns          | 1A           | `envTemplatePatterns: [".env.example",".env.template"]`          | advanced |
| `[ConfigFilePatterns]`   | Comma/array of generic config filename patterns        | 1A           | `configFilePatterns: ["config.*","*settings.*","*.config.js"]`  | advanced |
| `[WorkflowConfigPatterns]`| Comma/array of workflow/pipeline config patterns      | 1A           | `workflowConfigPatterns: ["*dag*.py","*workflow*","*pipeline*"]`| advanced |
| `[CoverageConfigPatterns]`| Comma/array of coverage config filename patterns      | 1A           | `coverageConfigPatterns: [".coveragerc","coverage.xml"]`        | advanced |
| `[ApmConfigPatterns]`    | Comma/array of APM/monitoring config patterns          | 1A           | `apmConfigPatterns: ["newrelic.js","sentry.config.js"]`         | advanced |
| `[KnownIssuesPatterns]`  | Comma/array of known-issues doc filename patterns      | 1A, outputs  | `knownIssuesPatterns: ["KNOWN_ISSUES.md","BUGS.md"]`            | advanced |
| `[ChangelogPatterns]`    | Comma/array of changelog filename patterns             | 1A, outputs  | `changelogPatterns: ["CHANGELOG.md","HISTORY.md"]`              | advanced |

---

## Framework & infrastructure detection variables

| Variable                | Type / Allowed Values                                  | Used By               | Example                                                           | Category |
|-------------------------|--------------------------------------------------------|-----------------------|-------------------------------------------------------------------|----------|
| `[CliFrameworkPatterns]`| Comma/array of CLI framework names to grep for        | Pre-Scan, 1A (CLI)    | `cliFrameworkPatterns: ["cobra","click","commander","yargs"]`    | advanced |
| `[WorkerPatterns]`      | Comma/array of worker/queue keywords to grep for      | Pre-Scan, 1A (workers)| `workerPatterns: ["cron","queue","worker","celery","sidekiq"]`   | advanced |
| `[LoggingPatterns]`     | Comma/array of logging library names to grep for      | 1A                    | `loggingPatterns: ["winston","loguru","log4j","slog","bunyan"]`  | advanced |
| `[InfraConfigPatterns]` | Comma/array of infra-as-code filename patterns        | Pre-Scan, 1A (infra)  | `infraConfigPatterns: ["*.tf","Chart.yaml","*-deployment.yaml"]` | advanced |
| `[InfraDirPatterns]`    | Comma/array of infra-related directory names          | Pre-Scan, 1A (infra)  | `infraDirPatterns: ["infra","terraform","k8s","helm","charts"]`  | advanced |

---

## Tool availability flags

| Variable      | Type / Allowed Values            | Used By     | Example                     | Category |
|---------------|----------------------------------|-------------|-----------------------------|----------|
| `[HasTree]`   | String: `yes` or `no` (or boolean equivalent) | 1A (command templates) | `hasTree: "yes"`             | advanced |
| `[HasJq]`     | String: `yes` or `no` (or boolean equivalent) | 1A (command templates) | `hasJq: "yes"`               | advanced |
| `[HasYq]`     | String: `yes` or `no` (or boolean equivalent) | 1A (command templates) | `hasYq: "yes"`               | advanced |

These flags determine whether the Cartographer may use `tree`, `jq`, or `yq` in its internal command templates or should fall back to simpler alternatives.
