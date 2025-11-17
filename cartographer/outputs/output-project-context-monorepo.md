# MONOREPO PROJECT CONTEXT

> Specialized `PROJECT_CONTEXT` template for monorepos with multiple apps/services and shared packages.

---

## [SECTION:MONOREPO_IDENTITY] Monorepo Identity

- **Repository Name:**  
- **Monorepo Tool:** `Nx` \| `Turborepo` \| `Lerna` \| `pnpm workspaces` \| `Yarn workspaces` \| `Rush` \| `None`  
- **Structure Type:** `apps-and-packages` \| `services` \| `multi-app` \| `mixed`  
- **Package Manager:**  
- **Primary Languages:**  
- **Total Packages (workspaces):**  
- **Total Applications:**  
- **Total Services:**  
- **Total Shared Libraries:**  

---

## [SECTION:MONOREPO_CONFIG] Monorepo Configuration

- **Workspace / Project Configuration Files:** (e.g. `pnpm-workspace.yaml`, `nx.json`, `lerna.json`, root `package.json` workspaces)  
- **Build System / Task Runner:** (Nx, Turbo, custom scripts)  
- **Versioning Strategy:** `independent` \| `fixed` \| `mixed` (describe)  
- **CI/CD Orchestration:** (GitHub Actions, GitLab, etc., and how it targets projects)  

---

## [SECTION:MONOREPO_STRUCTURE] Repository Structure

### 3.1 High-Level Tree

```tree
monorepo-root/
  ├── apps/            # User-facing applications
  ├── services/        # Backend services / microservices
  ├── packages/        # Shared libraries
  ├── libs/            # Additional shared code
  ├── infra/           # Infrastructure as code
  └── tools/           # Internal tooling
```

### 3.2 Package Categorization

| Category          | Count | Purpose                          |
|-------------------|-------|----------------------------------|
| Applications      |       | User-facing apps                 |
| Services          |       | Backend/API services             |
| Shared Libraries  |       | Reusable packages                |
| Tools             |       | Dev tooling / internal CLIs      |
| Infrastructure    |       | IaC and shared deployment config |

---

## [SECTION:PROJECTS] Applications & Services Inventory

For each app/service, provide a short profile.

### 4.1 Applications

| Name              | Path                | Tech Stack                     | Purpose                          |
|-------------------|---------------------|--------------------------------|----------------------------------|
|                   |                     |                                |                                  |

### 4.2 Services / Backends

| Name              | Path                  | Tech Stack                      | Purpose                          |
|-------------------|-----------------------|---------------------------------|----------------------------------|
|                   |                       |                                 |                                  |

---

## [SECTION:SHARED_LIBS] Shared Libraries & Packages

| Name              | Path                    | Language / Framework           | Consumers (apps/services)        |
|-------------------|-------------------------|--------------------------------|----------------------------------|
|                   |                         |                                |                                  |

Brief notes:
- Which packages are “core” and widely reused.
- Any circular or risky dependencies between packages.

---

## [SECTION:INTERSERVICE_DEPS] Inter-Service Dependencies & Communication

### 6.1 Direct Dependencies

| From Service      | To Service / App        | Type (HTTP, gRPC, MQ, lib)    | Notes                            |
|-------------------|-------------------------|--------------------------------|----------------------------------|
|                   |                         |                                |                                  |

### 6.2 Shared Infrastructure & Cross-Cutting Concerns

- Shared databases, queues, or topics used by multiple services.  
- Shared auth, logging, or observability layers.  

---

## [SECTION:CONFIG_TOOLING] Shared Configuration & Tooling

### 7.1 Root-Level Configuration

- Root `package.json` / `pyproject.toml` / other manifests.  
- Central config files (e.g. `config/`, `.eslintrc.*`, `.prettierrc.*`, `tsconfig.base.json`).  

### 7.2 Tooling & Developer Experience

- Internal CLIs under `tools/` or `scripts/`.  
- Code generators, templates, and linting/formatting setups shared across projects.  

---

## [SECTION:INFRA] Infrastructure & Environments

- Terraform / Pulumi / CloudFormation stacks under `infra/` or equivalent.  
- Shared Kubernetes manifests / Helm charts for multiple services.  
- How environments (dev/stage/prod) are represented at the monorepo level.  

---

## [SECTION:RISKS_MONOREPO] Monorepo-Specific Risks & Observations

- Tight coupling between services or shared packages.  
- Hotspots (high-change directories, brittle shared libraries).  
- Areas where monorepo tooling is underused or misconfigured.  

