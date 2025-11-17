# PROJECT CONTEXT

> Standard, comprehensive `PROJECT_CONTEXT.md` structure for the Project Context Cartographer.

---

## [SECTION:IDENTITY] Project Identity & Stack

- **Project Name:**  
- **Short Description:**  
- **Primary Repository Location / URL:**  
- **Primary Project Type:**  
- **Primary Language & Runtime:**  
- **Primary Framework(s):**  
- **Package Manager / Build Tool:**  
- **Primary Entry Point(s):**  
- **Runtime Targets (environments):**  
  - Development:  
  - Staging:  
  - Production:  

### 1.1 Primary Responsibilities

- Core responsibility 1  
- Core responsibility 2  

### 1.2 High-Level Capabilities

- Capability 1  
- Capability 2  

---

## [SECTION:STRUCTURE] Directory Layout & Key Statistics

### 2.1 Directory Overview

> Focus on `sourceRootDirs` and `testRootDirs` from config; omit noisy dirs from `commonIgnorePatterns`.

```text
<insert condensed tree view of main directories here>
```

### 2.2 Key Paths

- **Source Roots:**  
- **Test Roots:**  
- **Configuration & Manifests:**  
- **Infrastructure / Deployment:**  
- **Documentation:**  

### 2.3 Repository Statistics

| Metric                              | Value |
|-------------------------------------|-------|
| Total files (excluding ignores)     |       |
| Source files (primary language)     |       |
| Test files                          |       |
| Config / manifest files             |       |
| Approx LOC (primary language)       |       |
| Approx LOC (other languages)        |       |

---

## [SECTION:DEPENDENCIES] Dependencies & System Requirements

### 3.1 Runtime & Platform Requirements

- **Runtime(s):**  
- **Minimum Version(s):**  
- **Supported OS / Architectures:**  

### 3.2 Application Dependencies (Production)

| Name        | Version | Type / Role                  | Notes |
|-------------|---------|------------------------------|-------|
|             |         | Framework / Core runtime     |       |
|             |         | Database driver / ORM        |       |
|             |         | HTTP / RPC client/server     |       |

### 3.3 Development & Tooling Dependencies

| Name        | Version | Category (test, lint, build) | Notes |
|-------------|---------|------------------------------|-------|
|             |         |                              |       |

### 3.4 External Runtime Requirements

- Required system packages / tools (e.g. `curl`, `bash`, `make`)  
- Required services (e.g. Docker, Kubernetes cluster, message broker)  

---

## [SECTION:ARCHITECTURE] Architecture & Components

### 4.1 Overall Architecture Style

- **Architecture Style (Inference if needed):**  
  - Inference:  

### 4.2 Modules / Layers

| Layer / Module        | Purpose                               | Key Directories / Files               |
|-----------------------|---------------------------------------|---------------------------------------|
| Presentation / API    |                                       |                                       |
| Domain / Services     |                                       |                                       |
| Data Access / Repos   |                                       |                                       |
| Cross-Cutting / Infra |                                       |                                       |

### 4.3 Core Components

- Component name – location – short responsibility  
- Component name – location – short responsibility  

---

## [SECTION:DATA_MODEL] Data Model & Storage

### 5.1 Storage Technologies

- **Primary Database:**  
- **Other Stores (cache, search, object storage):**  

### 5.2 Key Entities / Tables

| Entity / Table | Storage Type | Location (file / path)     | Notes / Relationships           |
|----------------|-------------|----------------------------|---------------------------------|
|                |             |                            |                                 |

### 5.3 Relationships (High-Level)

- Relationship 1 (e.g. `User 1–* Order`)  
- Relationship 2  

---

## [SECTION:APIS] Public Interfaces & API Surface

> Adapt based on `projectType` (web API, CLI, library, worker, etc.).

### 6.1 HTTP / RPC APIs (if applicable)

| Method | Path / Operation        | Handler / Location          | Auth? | Description                    |
|--------|-------------------------|-----------------------------|-------|--------------------------------|
|        |                         |                             |       |                                |

### 6.2 CLI Commands (if applicable)

| Command              | Arguments / Options          | Description                    | Entry Point / File       |
|----------------------|-----------------------------|--------------------------------|--------------------------|
|                      |                             |                                |                          |

### 6.3 Library Surface (if applicable)

| Module / Package     | Public API                  | Purpose                        |
|----------------------|-----------------------------|--------------------------------|
|                      |                             |                                |

---

## [SECTION:INTEGRATIONS] External Integrations & Services

### 7.1 Third-Party Services

| Service / Provider   | Type (REST, SDK, message)   | Purpose                        | Where Configured                     |
|----------------------|-----------------------------|--------------------------------|--------------------------------------|
|                      |                             |                                |                                      |

### 7.2 Internal Services & Messaging

| Integration          | Protocol (HTTP, gRPC, MQ)   | Direction (in/out)             | Notes                                |
|----------------------|-----------------------------|--------------------------------|--------------------------------------|
|                      |                             |                                |                                      |

---

## [SECTION:CONFIG] Configuration & Environment

### 8.1 Configuration Files

- Config file path – format – purpose  

### 8.2 Environment Variables (Templates Only)

| Name                 | Required? | Default / Example (non-secret) | Purpose                         |
|----------------------|----------:|--------------------------------|---------------------------------|
|                      |           |                                |                                 |

### 8.3 Feature Flags & Modes

- Flag / mode – location – behavior impact  

---

## [SECTION:WORKFLOWS] Critical Workflows

> 2–5 most important flows; each should be evidence-backed.

### 9.1 Workflow: <Name>

**Goal:**  

**Entry Point:**  

**Step-by-step Flow:**
1.  
2.  
3.  

**Data Involved:**  

**External Calls:**  

**Error Handling:**  

### 9.2 Workflow: <Name>

*(Repeat structure as needed)*  

---

## [SECTION:OPERATIONS] Deployment, Operations & Testing

### 10.1 Build & Run (Development)

- Commands to install dependencies  
- Commands to run dev server / worker / CLI  

### 10.2 Build & Run (Production)

- Build commands / pipelines  
- Deployment artifacts (Docker images, K8s manifests, etc.)  

### 10.3 CI / CD Pipelines

- CI provider(s) and main workflow files  
- Key jobs (lint, test, build, deploy)  

### 10.4 Testing Strategy

| Test Type      | Tools / Frameworks     | Location / Pattern            | Notes          |
|----------------|------------------------|-------------------------------|----------------|
| Unit           |                        |                               |                |
| Integration    |                        |                               |                |
| End-to-end     |                        |                               |                |

---

## [SECTION:RISKS] Known Issues, Gaps & TODOs

### 11.1 Known Issues

- Issue – impact – evidence location  

### 11.2 Gaps in Documentation / Context

- Area – reason not documented (e.g. no files found)  

### 11.3 Future Improvements

- Potential improvement – rationale – related files / modules  

