# Mainframe Modernization Using Claude Code
## Migration Execution Activity Plan — Draft v1

**Purpose:** Provide a structured, executable plan for modernizing mainframe workloads using existing Claude Code reverse-engineering (RE) and forward-engineering (FE) workflows, with GCP as the target environment and APP as the data platform.

**Scope:** Environment and data readiness, workflow refinement, reverse engineering, FE planning, code generation, component and job-level testing, cycle-run reconciliation, deployment, and QA handoff.

> **Planning assumptions:** Claude dynamic RE and FE workflows already exist and require tuning; migration progresses by job or migration wave; legacy cycle-run outputs will be available for comparison. Owners, target dates, acceptance thresholds, and migration scope will be finalized during kickoff.

## 1. Execution lifecycle

1. GCP environment and infrastructure readiness
2. APP database and test-data readiness
3. Claude Code workflow readiness and fine-tuning
4. Reverse engineering execution and SME validation
5. Forward engineering planning and approval
6. Forward engineering execution and component validation
7. Job assembly, integration testing, and cycle-run reconciliation
8. Deployment and QA handoff

Phases 1–3 may run in parallel. Phases 4–8 repeat incrementally for individual jobs, components, or migration waves as their dependencies become ready.

## 2. Detailed activity register

### Phase 1 — GCP Environment & Infrastructure Readiness

| ID | Activity | Expected deliverable | Suggested owner | Dependency |
|---|---|---|---|---|
| 1.1 | Set up GCP development and integration environments | Provisioned environments | Cloud / DevOps | None |
| 1.2 | Configure IAM roles, access, networking and required services | Access readiness | Cloud / Security | 1.1 |
| 1.3 | Set up source repositories, CI/CD pipelines and deployment configurations | Build and deployment pipeline | DevOps | 1.1 |
| 1.4 | Validate connectivity among GCP, APP, source repositories and Claude Code execution environments | Connectivity test report | Cloud / Engineering | 1.1–1.3 |
| 1.5 | Establish application logging, execution monitoring and failure diagnostics | Observability setup | DevOps / Engineering | 1.1 |

### Phase 2 — APP Database & Data Readiness

| ID | Activity | Expected deliverable | Suggested owner | Dependency |
|---|---|---|---|---|
| 2.1 | Analyze existing APP tables against legacy data requirements | Table gap analysis | Data Engineering / SME | Scope and source access |
| 2.2 | Identify and create missing tables in APP | Updated table schemas | Data Engineering / DBA | 2.1 |
| 2.3 | Validate structures, mappings, data types, keys and relationships | Schema validation | Data Engineering / SME | 2.2 |
| 2.4 | Build extract/load procedures for new tables using legacy cycle-run data | Repeatable load process | Data Engineering | 2.3; cycle data access |
| 2.5 | Perform initial load into APP | Populated target tables | Data Engineering | 2.4 |
| 2.6 | Reconcile record counts, critical values and relationships with legacy data | Data reconciliation report | Data Engineering / SME | 2.5 |
| 2.7 | Establish repeatable data refresh/reset for cycle testing | Reusable test datasets | Data Engineering / QA | 2.6 |

### Phase 3 — Claude Code Workflow Readiness

| ID | Activity | Expected deliverable | Suggested owner | Dependency |
|---|---|---|---|---|
| 3.1 | Fine-tune existing RE workflows | Updated RE workflows | AI / Modernization Engineering | Existing workflows |
| 3.2 | Fine-tune FE planning and code-generation workflows | Updated FE workflows | AI / Modernization Engineering | Existing workflows |
| 3.3 | Standardize workflow inputs, outputs, artifacts, naming and versioning | Workflow execution conventions | AI / Engineering | 3.1–3.2 |
| 3.4 | Test workflows on representative COBOL, JCL, CICS and large-program cases | Workflow readiness report | AI / SME | 3.1–3.3 |
| 3.5 | Establish run tracking, retries, error handling and human review checkpoints | Workflow monitoring framework | AI / Engineering | 3.3–3.4 |

**Large-program consideration:** For large COBOL programs, use dependency-aware parsing, source indexing and selective context retrieval. An OKF/structured knowledge layer can improve navigation, but does not by itself eliminate model context limits.

### Phase 4 — Reverse Engineering Execution

| ID | Activity | Expected deliverable | Suggested owner | Dependency |
|---|---|---|---|---|
| 4.1 | Identify and baseline jobs/programs in the selected migration wave | Migration inventory | Modernization Lead / SME | Scope agreed |
| 4.2 | Map JCL steps, COBOL programs, CICS interfaces, copybooks, datasets and database dependencies | Dependency inventory | RE Engineering / SME | 4.1 |
| 4.3 | Execute Claude RE workflows for scoped components | RE artifacts | RE Engineering | 3.4; 4.1–4.2 |
| 4.4 | Monitor runs, troubleshoot failures and rerun unsuccessful components | RE execution log | RE Engineering | 4.3 |
| 4.5 | Have mainframe SMEs validate business logic, data flows, dependencies and processing behavior | SME review and findings | Mainframe SME | 4.3–4.4 |
| 4.6 | Apply findings, correct/regenerate artifacts and baseline approved RE outputs | Approved RE baseline | RE Engineering / SME | 4.5 |

### Phase 5 — Forward Engineering Planning

| ID | Activity | Expected deliverable | Suggested owner | Dependency |
|---|---|---|---|---|
| 5.1 | Run Claude FE planning workflows/agents using approved RE artifacts | Draft FE plans | FE Engineering | 4.6; 3.4 |
| 5.2 | Monitor planning workflows and resolve run failures | FE planning log | FE Engineering | 5.1 |
| 5.3 | Review target components, business processing logic and job decomposition | Component design review | Architect / SME | 5.1 |
| 5.4 | Validate source-to-target data mappings and component interfaces | Mapping/interface specifications | Architect / Data Engineering | 5.3 |
| 5.5 | Review and finalize FE plan and component list | Approved FE plans | Architect / SME | 5.3–5.4 |
| 5.6 | Define component acceptance criteria and test scenarios | Component test criteria | Engineering / QA | 5.5 |

### Phase 6 — Forward Engineering & Component Validation

| ID | Activity | Expected deliverable | Suggested owner | Dependency |
|---|---|---|---|---|
| 6.1 | Execute Claude FE code-generation workflows for approved components | Generated target code | FE Engineering | 5.5; 3.4 |
| 6.2 | Monitor generation runs, diagnose failures and rerun as needed | FE execution log | FE Engineering | 6.1 |
| 6.3 | Build/compile and run generated components | Executable components | Engineering | 6.1–6.2; Phase 1 |
| 6.4 | Review business logic, target architecture, error handling and coding standards | Code review findings | Engineering / Architect | 6.3 |
| 6.5 | Execute component unit tests and check outputs against acceptance criteria | Unit test evidence | Engineering / QA | 6.3–6.4; 5.6 |
| 6.6 | Fix defects and regenerate/refine affected components | Validated components | FE Engineering | 6.5 |
| 6.7 | Version validated source, tests and artifacts | Committed, tagged components | Engineering | 6.6 |

### Phase 7 — Job Assembly, Integration Testing & Cycle Runs

| ID | Activity | Expected deliverable | Suggested owner | Dependency |
|---|---|---|---|---|
| 7.1 | Assemble validated components into a complete sequence for each job | Integrated job | Engineering | 6.7 |
| 7.2 | Validate step ordering, dependencies, intermediate datasets and handoffs | Job integration validation | Engineering / SME | 7.1 |
| 7.3 | Execute job-level integration tests | Integration test evidence | Engineering / QA | 7.2; Phase 2 data |
| 7.4 | Execute target-platform cycle runs using representative legacy cycle inputs | Cycle-run outputs | Engineering / QA | 7.3; 2.7 |
| 7.5 | Compare target results with legacy cycle-run baselines | Reconciliation report | QA / SME / Data Engineering | 7.4 |
| 7.6 | Identify root cause of mismatches: RE, data, FE plan, code or orchestration | Defect/root-cause log | Engineering / SME | 7.5 |
| 7.7 | Correct appropriate upstream artifacts, workflows or code and rerun affected tests | Corrected implementation | Relevant workstream | 7.6 |
| 7.8 | Repeat test/reconciliation iterations until acceptance criteria are met | Accepted job/cycle | QA / SME | 7.7 |
| 7.9 | Test multi-job or cross-application dependencies where applicable | End-to-end test results | Engineering / QA | 7.8 and related jobs |

### Phase 8 — Deployment & QA Handoff

| ID | Activity | Expected deliverable | Suggested owner | Dependency |
|---|---|---|---|---|
| 8.1 | Finalize migrated source, job definitions, configuration and deployment artifacts | Release candidate | Engineering / DevOps | 7.8–7.9 |
| 8.2 | Perform readiness checks, defect disposition and acceptance review | Readiness approval | Delivery Lead / QA | 8.1 |
| 8.3 | Deploy to designated QA environment | QA deployment | DevOps | 8.2 |
| 8.4 | Package RE/FE artifacts, traceability, test evidence, reconciliation results and known issues | QA handoff package | Engineering / QA | 8.3 |
| 8.5 | Conduct QA knowledge transfer and formal handoff | QA handoff acknowledgment | Delivery Lead / QA | 8.4 |
| 8.6 | Support QA defect remediation, regression tests and controlled redeployment | QA remediation record | Engineering / DevOps | 8.5 |

## 3. Dependencies and parallel workstreams

- **Workstream A — GCP infrastructure:** Phase 1 can begin immediately and should establish integration-test environment readiness.
- **Workstream B — APP data readiness:** Phase 2 proceeds alongside Phase 1, subject to required connectivity and access; data loads and reconciliation must precede dependent cycle tests.
- **Workstream C — Claude workflows:** Phase 3 runs in parallel using representative source assets and samples.
- **Job execution pipeline:** For each migration wave: **Scope & dependencies → RE → SME approval → FE plan → design approval → FE generation → unit testing → job assembly → integration testing → cycle run → reconciliation → QA**.
- **Incremental processing:** An individual job may move forward once its specific prerequisites are satisfied; do not require all applications to finish RE before any FE begins.

## 4. Stage gates and exit criteria

| Gate | Checkpoint | Required evidence |
|---|---|---|
| G1 | Environment/data readiness | Required GCP services, connectivity, App schemas and test data ready for the selected job/wave |
| G2 | RE sign-off | SME-approved RE artifacts, business logic and dependency mapping |
| G3 | FE plan approval | Approved component decomposition, interfaces and source-target mappings |
| G4 | Component validation | Code reviews completed, components executable and unit tests passed |
| G5 | Integration/cycle acceptance | Job integration complete, outputs reconciled with legacy baseline, residual defects accepted per agreed criteria |
| G6 | QA handoff | Release candidate deployed to QA, evidence package complete and team acknowledgment received |

**To define at kickoff:** Numerical reconciliation tolerances, test coverage thresholds, severity-based defect exit thresholds, performance targets, security requirements, and gate approvers.

## 5. Operational controls and measurements

### Job-level progress tracking

Track each job from inventory to QA using its current stage, owner, target/actual dates, blockers, artifact links, defect count and sign-offs. Track shared dependencies separately so one infrastructure task does not appear as repeated job work.

### Claude workflow effectiveness

Measure RE/FE first-pass completion, execution failures, retry counts, SME correction rate, generated-code build success, test pass rate, iteration counts and manual review effort. Use observations to target workflow tuning.

### Traceability and changes

Maintain a link from **legacy job/program and source version → RE artifact → approved FE plan → generated target code → tests → reconciliation output → release version**. When defects are found, log their source and determine which upstream assets require regeneration and review.

### Pilot then scale

Begin with a representative pilot job that includes real dependencies and data handling. Validate the entire RE-to-QA path before expanding the size or concurrency of migration waves.

## 6. Suggested ownership by workstream

| Workstream | Proposed accountable team |
|---|---|
| GCP / IAM / CI-CD / monitoring | Cloud and DevOps |
| APP schema, loads and reconciliation | Data Engineering / DBA |
| Claude workflows and orchestration | AI / Modernization Engineering |
| RE review and business validation | Mainframe SMEs |
| FE planning and architecture | Modernization Architects / SMEs |
| Generated code, component tests and assembly | Engineering |
| Integration and cycle-run validation | Engineering / QA / Mainframe SMEs |
| Deployment and QA transition | DevOps / Release / QA |

## 7. Execution tracker template

Use one record per activity **per relevant job or wave**. Shared environment/workflow readiness activities may have a single project-level record.

| Field | Example / guidance |
|---|---|
| Activity ID | `7.5` |
| Migration wave | Wave 1 |
| Job / component | Job identifier |
| Activity | Reconcile cycle-run results |
| Owner | QA lead |
| Dependencies | 7.4, baseline available |
| Planned start / finish | To be assigned |
| Actual start / finish | Track during execution |
| Status | Not Started / In Progress / Blocked / Complete |
| Deliverable link | Reconciliation report |
| Defect / blocker reference | Issue tracker ID |
| Sign-off | Reviewer, date and decision |

## 8. Kickoff decisions to finalize

1. Prioritized migration inventory, pilot job and wave boundaries.
2. Exact target technology design, APP responsibilities and system interfaces.
3. Source-of-truth legacy cycle-run datasets and expected output comparison methodology.
4. Claude workflow deployment configuration, model permissions, code review policies and source-data security controls.
5. Named owners, effort estimates, milestone dates and dependency tracking approach.
6. Agreed quantitative gate criteria and SME/QA sign-off responsibilities.
7. QA environment promotion path and responsibilities after handoff.

---

**Planning note:** This plan intentionally covers migration execution through deployment to and handoff to QA. Production cutover, parallel production operation, rollback and post-go-live support should be added as a separate release/cutover plan if those are included in the project scope.
