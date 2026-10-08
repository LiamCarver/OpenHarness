# Harness Engineering Architecture

## Architecture direction

Use a **TypeScript monorepo** with a React SPA, a Node.js backend, locally persisted state, and replaceable model and MCP capabilities. Start as a local-first modular monolith rather than a distributed system.

A build of the React SPA can be served by the Node backend as static assets; one distribution can therefore contain both frontend and backend. LM Studio and MCP servers may run as separate local processes.

## Proposed modules and boundaries

| Module | Owns | Must not own |
| --- | --- | --- |
| LLM core | Provider-neutral messages, generation, streaming, structured responses, tool-call proposals, cancellation and usage | MCP execution or workflow authorization |
| MCP core | Client connections, discovery, tool schemas, invocation, structured results, errors and lifecycle | LLM prompting or workflow scheduling |
| Workflow core | Workflow definitions, DAG validation, state transitions, execution plans, policies, retries and evidence | Provider-specific model or MCP implementation details |
| Orchestration/adapters | Mapping LLM tool proposals to eligible MCP invocations, invoking step executors and translating results | Hidden authority to override workflow policies |
| Scenario/test tooling | Shared initial states, expected behavior, mock streams, seeded stores and test adapters | Production runtime dependencies |

Each module owns its public contracts. Higher-level orchestration composes them; avoid a single oversized shared-contract package.

## TypeScript package sketch

```text
packages/
  llm/
  mcp/
  workflow/
  orchestration/
  schemas/              # only genuinely cross-cutting definitions
  scenarios/            # development/test-only
apps/
  server/
  web/
tests/
  integration/
  e2e/
  live-model/
```

The structure is illustrative and should remain small initially.

## Runtime and integration contracts

- The LLM core exposes provider-independent requests and event streams; an LM Studio/OpenAI-compatible adapter is a sensible first implementation.
- The MCP core handles standard protocol interactions; a harness-defined **conformance profile** adds the requirements needed for governed execution.
- The workflow engine owns step identifiers, dependencies, permissions, schemas, expected evidence, retries, approvals and state.
- For arbitrary downstream integrations, contributors expose MCP tools rather than adding new harness-specific transports.
- **Internal engine operations** (scheduling, persistence, validation) need not go through MCP.
- Treat model inference as a dedicated internal provider abstraction, not an arbitrary downstream integration.
- MCP provides protocol consistency, not behavioral determinism. Output schemas can be optional; side-effect and idempotency hints must not be treated as guarantees.

## Configuration pipeline

1. User, visual editor or configuration assistant proposes changes.
2. The backend applies **typed configuration commands** to an editable `WorkflowDraft`.
3. Validate schema, graph structure, policy permissions and capability bindings.
4. Compile to an execution plan with concrete tool bindings and effective policies.
5. Show a diff and obtain any necessary approvals.
6. Serialize versioned, canonical YAML through the harness (not model-generated final YAML).
7. Execute a pinned, approved plan and record amendments as new revisions.

Maintain distinct concepts: `WorkflowDefinition`, `WorkflowDraft`, `ExecutionPlan` and `WorkflowRun`. Project YAML in Git remains the configuration source of truth; SQLite stores local execution state and indexes.

## Frontend and streaming

- React + TypeScript + Vite; a DAG library such as React Flow/XYFlow; optional Fastify backend.
- Serve compiled SPA assets and HTTP API through the same local Node application.
- Use Server-Sent Events initially for one-way execution updates, with ordinary HTTP requests for commands and approvals.
- Translate raw model chunks and tool activity into typed harness events such as `agent.text.delta`, `step.started`, `step.failed` and `approval.required`.
- Persist meaningful execution events before broadcasting; sequence per run, support replay/reconnection with cursors, and deduplicate in consumers.
- Run workflows independently of browser connections. Cancellation, client disconnects and backend restart recovery need explicit semantics.
- Consider batching ephemeral text deltas rather than storing every token separately.

## Persistence

- Start with **SQLite** for workflow run state, events and metadata, with an artifact directory for larger test logs, reports and traces.
- Put data in an OS-appropriate user application-data directory, not next to the executable.
- Store authoritative Git-tracked workflow YAML in the project repository; don't silently create a second editable source of truth.
- A later centrally hosted mode may use PostgreSQL and isolated workers, but that introduces authentication, multitenancy and distributed scheduling concerns.

## Shared frontend/backend logic

- Share pure domain algorithms and runtime schemas where environment-neutral.
- Keep filesystem, secrets, process execution, model inference, MCP connections and persistence on the backend.
- Validate all incoming data on the backend even if the frontend uses the same TypeScript types.
- Prefer plain serializable data over class instances with runtime-specific behavior.
- Use explicit package exports and import restrictions to prevent server-only dependencies from reaching browser bundles.
- Test shared code in both Node and browser environments; cover serialization and schema-version migrations.

## High-fidelity test strategy

Use canonical JSON scenario definitions with **initial state, dependency behavior and expected outcomes**, not only database rows. Scenarios drive independent adapters for different fidelity levels:

| Layer | Execution |
| --- | --- |
| Frontend | React + mock API and SSE streams from scenarios, e.g. MSW |
| Backend integration | Real API and workflow engine + seeded temporary SQLite + simulated LLM/MCP dependencies |
| Browser E2E | Real browser, API and engine + controlled test model provider/MCP server |
| Live integration | Real LM Studio and real MCP services, with bounded assertions |

Suggested tools: Vitest, MSW and Playwright. Keep mock implementation and behavioral oracle separable to avoid shared blind spots. Include adverse scenarios: malformed data, interrupted streaming, MCP timeout, duplicate events, partial execution, cancellation, failed approval, unsafe retry and backend restart.

Make scenarios available in a UI demo mode so contributors can inspect complex workflow states without loading a model.

## Security and operational invariants

- All consequential actions go through the workflow engine and its event record.
- Agent steps may propose, but may not independently authorize, tool invocation or weaken their own quality gates.
- Enforce filesystem, process, network and secret permissions using actual isolation, not merely YAML declarations.
- Keep secrets outside model context; resolve them in trusted execution boundaries.
- Version and pin relevant workflow configurations, tool identities/contracts and evidence.
- Avoid automatically retrying non-idempotent operations.
- Preserve provenance, redact sensitive records and define retention rules.

## Initial vertical slice

1. Test LLM core against a mock model service and LM Studio.
2. Test MCP core against a protocol-compatible local test server.
3. Create a tiny agent loop in orchestration that maps model-proposed calls to authorized MCP invocations.
4. Add a one-step workflow with typed configuration, an event log and approval-free local execution.
5. Display streamed text and step progress in React.
6. Seed the same scenario through UI mocks, backend integration tests and full browser E2E.
7. Add the first quality workflow: implement a small To Do change, run tests and record evidence.

## Open decisions

- Exact YAML DSL and internal runtime schema library.
- License and project governance.
- Which MCP tool capabilities require supplemental output contracts.
- Event-store layout, artifact retention and redaction policy.
- Local process isolation on Windows, macOS and Linux.
- Canonical YAML formatting versus preserving human-authored comments and layout.
- How to detect and assess documentation, test and observability quality independently.
