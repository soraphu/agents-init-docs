## สถาปัตยกรรมแบบแยกชั้นและการจัดโครงสร้างโฟลเดอร์ (Layered Architecture & Code Organization)

```
## Layered Architecture & Code Organization

### Architectural Constraints
- Enforce strict separation of concerns using Layered Architecture.
- Data flow MUST strictly follow the single-direction path: Routes -> Middlewares -> Controllers -> Services -> Repositories -> Models.
- Higher-level layers may depend on lower-level layers, but lower-level layers MUST NEVER import, reference, or depend on higher-level layers.

### Core Layer Responsibilities
- **Routes**: Define API endpoints, HTTP methods, and apply relevant route-level middlewares. Must not contain any business or request-handling logic.
- **Middlewares**: Handle cross-cutting concerns such as authentication, authorization, request logging, rate limiting, and context setup.
- **Controllers**: Serve as the HTTP interface. Responsible strictly for reading/parsing HTTP inputs, delegating business logic to Services, and returning standardized HTTP responses.
- **Services**: Contain core business logic, domain rules, and orchestration logic. Must remain HTTP-agnostic (no direct reliance on request/response objects).
- **Repositories**: Encapsulate all database operations, raw queries, or external API data fetching. Prevent direct ORM/query execution inside Services.
- **Models**: Define database entity schemas, tables, relationships, and data structure representations.

### Reusability & DRY Principles
To strictly follow the Don't Repeat Yourself (DRY) principle, extract reusable code into dedicated modules rather than duplicating logic across layers:
- `dtos/`: Data Transfer Objects defining strict request payloads and response shapes.
- `validators/`: Request validation schemas and input sanction logic.
- `constants/`: Application-wide immutable values, status codes, enum mappings, and error messages.
- `utils/`: Pure, stateless helper functions and common utility methods.
- `errors/`: Custom application error classes and centralized exception handling structures.
- Extend additional specialized folders (e.g., `jobs/`, `events/`, `integrations/`) as required by project scale or stack context.
```

## การจัดการความลับและการป้องกันข้อมูลสำคัญรั่วไหล (Secrets & Sensitive Data Security)

```
## Secrets & Sensitive Data Security

### Environment Variables & Secrets Management
- NEVER hardcode secrets, API keys, private keys, database credentials, tokens, or environment secrets directly inside the codebase.
- Access sensitive configurations exclusively through runtime environment variables or secure secret managers.
- Strictly exclude local environment configuration files (e.g., `.env`, `.env.local`) from version control by declaring them in `.gitignore`. Commit only sanitized template files (e.g., `.env.example`) containing placeholders.

### Payload & Output Exposure Prevention
- MUST NOT include environment secrets, internal keys, or sensitive credentials in public API response payloads, front-end build assets, or client-accessible contexts.
- Sanitize and redact sensitive data from all logs, error stacks, monitoring metrics, and external API requests.

### Repository & Version Control Safeguards
- Enforce pre-commit hooks and secret scanning tools within CI/CD pipelines to detect and block committed secrets automatically.
- Treat any secret pushed to Git as compromised. Revoke and rotate the exposed secret immediately rather than relying solely on scrubbing Git history.
```

## การอัปเดตเอกสารระบบและการตรวจสอบความถูกต้องของโค้ดหลังเสร็จงาน (Documentation Maintenance & Post-Task Verification)

```
## Documentation Maintenance & Post-Task Verification

### Documentation Synchronization
- MUST keep all project documentation strictly synchronized with the current codebase immediately upon completing any task, refactoring, or feature development.
- **API Specifications**: Update API reference files (e.g., `API_ENDPOINTS.md`, Swagger/OpenAPI docs) whenever new endpoints are added, existing routes are updated, or request/response payloads are modified.
- **Architecture & Structure**: Update system architecture and structure guides (e.g., `ARCHITECTURE.md`, `README.md`) whenever files, directories, or core modules are created, deleted, renamed, or reorganized.
- Treat out-of-date documentation as a build-blocking flaw; task completion is strictly contingent upon updated documentation.

### Pre-Commit Quality & Verification Checks
- MUST execute all mandatory verification and validation scripts before marking any task complete or committing changes to version control.
- **Build Checks**: Run project compilation commands (e.g., `npm run build`, `tsc --noEmit`, or equivalent pipeline build tasks) to verify zero build failures or type mismatches.
- **Automated Testing**: Run the test suite (e.g., `npm run test`, `npm run test:unit`) to ensure no regression bugs are introduced and all existing functionality remains intact.
- **Code Quality**: Resolve all linting and code formatting issues (e.g., `npm run lint`) prior to completing the task.
```

## การกำหนดมาตรฐานเวอร์ชันของ Package และ API ในการเขียนโค้ด

```
## Package Versioning & API Alignment

- Write code, syntax, and API integrations that strictly align with the current stable versions of project packages and dependencies.
- Verify method signatures, options, and module imports against the latest documentation to avoid calling deprecated or removed APIs.
- Do not use outdated patterns, legacy syntax, or patterns from older major versions unless explicitly specified by the environment constraints.
- Explicitly check for breaking changes in dependencies before generating or refactoring code snippets.
- Ensure all newly introduced package usage conforms to the active runtime environment and package manager configurations (e.g., package.json, requirements.txt, Cargo.toml).
```

## การตรวจสอบเอกสาร Context ใน Directory docs/ ก่อนเริ่มปฏิบัติงาน

```
## Pre-Task Documentation Inspection

- Always inspect the `docs/` directory for task-relevant context, design specifications, and system guidelines before initiating implementation.
- Ground all code architecture and technical decisions in established documentation to minimize guessing, assumptions, and hallucinated patterns.
- Treat documentation inside `docs/` as the single source of truth for existing workflows, data structures, and domain logic.
- Verify whether the assigned task impacts or updates existing documentation and align the implementation accordingly.
```
