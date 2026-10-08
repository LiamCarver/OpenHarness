# Harness Vision

## Problem statement

AI-assisted software development increasingly relies on costly proprietary frontier models and informal, partly opaque agent workflows. Generating code faster does not guarantee that the result is appropriately tested, understandable, traceable to business intent, maintainable or observable in operation.

Smaller open-weight models are improving, yet conventional agent designs often expect the model to manage too much: finding context, choosing tools, maintaining workflow state, remembering standards and deciding when work is complete.

For organizations, inconsistent AI usage may bring financial cost, computational waste, fragile software and reduced accountability. These are motivating concerns to investigate, **not yet established outcomes for every organization**.

## Vision statement

Create a **free and open-source, model-agnostic, local-first software engineering harness** that combines declarative workflows, deterministic execution controls, code intelligence, traceable engineering knowledge and robust verification to maximize the useful work of modest local language models.

The harness should be valuable whether operated by an individual on a workstation or, eventually, shared across developers on privately operated GPU infrastructure.

## What success looks like

An early demonstration could be a small To Do app built and subsequently maintained by a small local model, with:

- Strong unit, integration and E2E tests that detect meaningful regressions.
- Technical and business documentation linked through the code and tests.
- Logs, metrics and traces that help diagnose real failures.
- A visible, approved, replayable workflow recording the decisions, tools, artifacts and verification evidence used to produce it.
- No requirement for paid frontier-model inference during normal operation.

This would be more compelling than generating a bigger application without those standards.

## Longer-term impact sought

- Make high-standard AI-assisted engineering more accessible to individuals, communities and organizations.
- Reduce unnecessary dependence on proprietary AI tools and providers.
- Enable local or shared private inference where economically and operationally appropriate.
- Encourage stable, maintainable software through executable engineering disciplines.
- Potentially reduce cost and environmental impact, measured empirically rather than assumed.
- Allow open-source contributors to extend models, workflows, integrations, verification techniques and language-specific code intelligence.

## Development and evaluation strategy

Use frontier models as **development and independent evaluation aids** while designing the harness for operation with a small local model. Keep evaluator findings separate from benchmark runs; do not feed hidden answers into the model being evaluated.

Compare:

1. Baseline agent + fixed small model.
2. Successive versions of the harness + the same fixed small model.
3. The same harness + stronger models, to distinguish harness benefits from model benefits.

Measure task completion, human intervention, regressions, verification quality, documentation traceability, useful observability, time, inference and infrastructure costs, and ideally energy per accepted change. Keep held-out tasks and test ongoing maintenance, not only initial code generation.

## Intended audience and governance

The project is **not being conceived as a product for sale or a startup**. It is intended for anyone who wants to adopt or improve disciplined AI engineering, including individual engineers, open-source communities, platform teams and large organizations.

Licensing, community governance and contribution policies remain future choices; they should support broad inspection, reuse, modification and self-hosting.

## Known uncertainties

- How far can orchestration compensate for the reasoning limitations of small local models?
- Can comprehensive engineering standards be maintained through repeated changes, rather than just satisfied once?
- When do additional steps create excessive bureaucracy or computation?
- Under what workloads are private inference and smaller models economically or environmentally preferable?
- How well do successful results generalize across repositories, languages and tasks?
