# Harness Functionality

This document collects **proposed capabilities**, not an implementation commitment. Some ideas belong in an initial prototype; others are research directions or later enhancements.

## Workflow authoring and execution

- Every task runs as a workflow with **at least one step**.
- Developers can start from blank workflows or templates, including a **full SDLC template** with configurable integration placeholders.
- Support reusable, composable testing, documentation, observability, review and deployment modules.
- Natural-language requests can propose dynamic workflows; developers can review, adjust and approve them before execution.
- Show workflows as a visual DAG with live status, progress, dependencies and artifacts, like a DevOps pipeline.
- Allow controlled amendments during a run when new information is discovered, with validation and approval where needed.
- Separate reusable workflow definitions from one-off execution plans and run-specific amendments.
- Offer conversational editing, a visual node inspector/editor, and direct YAML editing, all based on one canonical typed model.
- **The harness, not the model, serializes final YAML.** The model proposes typed configuration operations; validation and application are handled by software.
- Show diffs, effective configuration, permissions and the concrete execution graph before approval.

## SDLC quality features

### Testing
- Configurable unit, integration and E2E gates at appropriate fidelity.
- Test-impact analysis, failure-path verification, regression checks and selective mutation testing where useful.
- Verifiable test execution results rather than agent-reported claims of success.
- Requirements-based assertions and checks against superficially passing but ineffective test suites.

### Documentation and traceability
- Stable links among business requirements, tickets, technical docs, ADRs, symbols, tests and changes.
- Optional language-specific metadata conventions such as C# attributes referencing centralized document/rule identifiers.
- Compiler-backed code-to-code relationships (for example, Roslyn reference lookup) combined with explicit code-to-business links.
- Distinguish declared, tool-derived, reviewed and inferred relationships; do not treat links as correctness proofs.
- Consider a queryable knowledge graph or indexed relationship store when it demonstrably helps retrieval and impact analysis.
- Avoid duplicate sources of truth: external systems own their authoritative records; the harness indexes or caches references with provenance.

### Observability
- Enforce configurable structured logging, metrics and distributed tracing conventions, potentially based on OpenTelemetry.
- Verify telemetry during integration or E2E tests and injected-failure scenarios.
- Correlate runtime diagnostic evidence with work items, implementation changes and test results where feasible.
- Check for secret leakage, sensitive data exposure and excessive telemetry volume.

## Integrations and models

- MCP as the **exclusive downstream integration protocol** for arbitrary external tools and systems, subject to a stricter harness execution contract.
- Discover tools, validate input schemas and tool bindings, verify output contracts where supplied, and require additional contracts where necessary for guarded transitions.
- Language-specific services, ticketing systems, source control, CI, documentation and other integrations may be made available through MCP servers.
- Local inference through LM Studio initially; eventually allow OpenAI-compatible and other provider implementations.
- Keep mandatory execution policy independent of tool hints, model suggestions or MCP metadata.
- Potential classifier-based routing remains **optional and experimental**, best suited to genuine ambiguous choices or risk categorization, not rules already known deterministically.
- Research MAKER-inspired maximal decomposition, voting and red-flagging for bounded tasks; benchmark against simpler approaches.

## Execution transparency and safety

- Every consequential operation emits structured, traceable workflow events.
- Inspect model activity, tool calls, approvals, configuration revisions, file changes, verification results and artifacts from workflow nodes.
- Support replay or inspection of recorded events; distinguish deterministic replay from rerunning nondeterministic model/tool operations.
- Provide step-specific permissions, bounded execution, timeout and retry behavior, with awareness of non-idempotent side effects.
- Prevent agents from silently weakening the policies governing their own work.
- Resolve secrets at execution time; avoid exposing raw credentials to model prompts and logs.
- Plan for redaction, privacy controls and retention settings.

## Developer experience and distribution

- Local app with a browser-based SPA and local backend, with smooth streaming of AI text and workflow events.
- Offline-capable operation when local models and tools are available.
- Template-based onboarding and a minimal one-step workflow for experimentation.
- Scenario-driven UI demonstration mode with representative successes, failures, approvals, concurrent jobs and reconnections.
- Optional white-label branding: organization name, logo, favicon, theme tokens and limited terminology, without altering execution behavior.
- Future shared/private-infrastructure deployment supporting multiple developers.

## Suggested progression

**Early:** model and MCP integration modules; a minimal agent loop; typed workflow definitions; one-step execution; structured events; YAML generation; basic UI and comprehensive test scenarios.

**Next:** workflow DAG, dynamic plan approval, verification gates, conversational configuration edits, visual inspection, traceability links and local event replay.

**Later/research:** richer code intelligence, graph-backed context, adaptive decomposition, classifier routing, organizational policy rollout, shared inference deployment and extensive white-labeling.
