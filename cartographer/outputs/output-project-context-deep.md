# PROJECT CONTEXT (Deep)

> Deep `PROJECT_CONTEXT.md` variant with additional traces, infra details, and TODO extraction.

---

## [SECTION:IDENTITY] Project Identity & Stack

<same as standard template, but filled with maximum detail and cross-references>

---

## [SECTION:STRUCTURE] Directory Layout, Metrics & Hotspots

- Condensed tree view of main directories.  
- Detailed file and LOC counts per major module.  
- Identification of **hotspots** (high-change or high-complexity areas) if evidence is available.  

---

## [SECTION:DEPENDENCIES] Dependencies, System Requirements & Risky Areas

- Full dependency lists (grouped by role).  
- Notes on **critical** or **risky** dependencies (e.g. unmaintained, deprecated).  
- System-level requirements and resource expectations.  

---

## [SECTION:ARCHITECTURE] Architecture, Flows & Design Choices

- Architecture style(s) with explicit `Inference:` labels where applicable.  
- Module / layer table with responsibilities and boundaries.  
- High-level explanation of key design decisions that are visible from the code (e.g. synchronous vs async processing, event-driven patterns).  

---

## [SECTION:DATA_MODEL] Data Model & Storage (Deep)

- Detailed entity descriptions including important fields and constraints when visible.  
- Relationship overviews (can be described textually or as simple diagrams).  
- Notes on migrations, versioning, and data retention policies when evident.  

---

## [SECTION:APIS] Public Interfaces (Detailed)

- HTTP / RPC / CLI / library surfaces with:  
  - Purpose, auth requirements, input/output summaries.  
  - Where they live in the code.  
  - Any notable edge cases or constraints inferred from validation logic.  

---

## [SECTION:INTEGRATIONS] External & Internal Integrations (Deep)

- For each integration: endpoints/topics/queues (redacting secrets), retry / timeout patterns, and error handling strategies visible in the code.  
- Grouping by type (payments, storage, messaging, analytics, internal services, etc.).  

---

## [SECTION:CONFIG] Configuration, Environment & Feature Flags

- Detailed overview of configuration files and how they compose.  
- Env var table(s) with purpose and sensitivity notes (without exposing values).  
- Feature flags, modes, or configuration-driven behaviors and where they are implemented.  

---

## [SECTION:WORKFLOWS] Critical Workflows (Deep Traces)

For each selected workflow:

- Entry point.  
- Step-by-step flow including key functions, services, and data transitions.  
- External calls and side effects.  
- Error and retry handling logic.  

---

## [SECTION:OPERATIONS] Deployment, Operations, Observability & Testing

- Build and deployment pipelines, including environments and promotion flows.  
- Operational procedures (start/stop, scaling, backups) when documented in code or scripts.  
- Observability stack (logging, metrics, tracing) and where it is configured.  
- Detailed testing strategy with coverage notes per layer/module (if discoverable).  

---

## [SECTION:RISKS] Known Issues, TODOs & Risk Assessment

- Known issues with impact and evidence links.  
- **TODO / FIXME extraction:** summarized by area/module, pointing to source files (no long code snippets).  
- High-level risk assessment based on visible issues (security, performance, maintainability, operational risk).  

