# Harness Philosophy

## Purpose

Build a **free and open-source**, local-first AI software engineering harness that helps relatively small, locally hosted language models produce and maintain software to a high engineering standard.

The objective is not to generate the most impressive application with the most powerful model. It is to demonstrate how much dependable engineering can be achieved through well-designed surrounding systems, even when the model is modest (for example, a Gemma E4B-class model running through LM Studio).

## Guiding principles

1. **Engineering quality over spectacle.** A well-tested, documented, observable To Do application is a more meaningful early result than a complicated application lacking engineering discipline.
2. **Intelligence where useful; deterministic software where possible.** Use models for interpretation, planning, generation and investigation. Use ordinary code for contracts, authorization, scheduling, validation, execution and evidence.
3. **Three inseparable quality pillars:**
   - Wide and deep tests at appropriate fidelity: unit, integration and end-to-end, including failures and edge cases.
   - Business and technical documentation, with navigable links between requirements, architectural decisions, code and tests.
   - Mature, full-stack observability: structured logs, metrics, traces and meaningful diagnostic evidence.
4. **Local-first and model-agnostic.** The system should work without mandatory cloud inference, while permitting other providers when configured.
5. **Free and open source as a fundamental commitment.** People and organizations should be able to use, clone, inspect, modify and contribute to the harness without commercial lock-in.
6. **Transparency and traceability by design.** Make workflow definitions, proposed changes, execution activity, tool calls, evidence, and failures inspectable.
7. **Opinionated execution, flexible process.** Enforce essential runtime invariants, but let users choose SDLC templates, policy profiles and integrations.
8. **Avoid unnecessary probabilistic decisions.** Don't ask a classifier or LLM to decide something that configuration, a compiler or a deterministic rule can establish.
9. **Independent evidence matters.** Tests and machine checks are useful, but no single test result, annotation or frontier-model review constitutes proof of correctness.
10. **Learn openly.** Reproducible experiments, documented trade-offs, and honest publication of failures are valuable outcomes in themselves.

## Distinctions worth preserving

- **Deterministic orchestration is not deterministic AI:** workflow transitions, tool permissions and evidence handling can be predictable even when model outputs are probabilistic.
- **Traceability is not correctness:** an attribute linking a method to a business rule expresses intent, not proof of compliant behavior.
- **Self-hosting is not automatically cheaper or greener:** claims depend on hardware, electricity, utilization, task completion rates and operational overhead.
- **A scenario fixture is not an independent oracle:** sharing fixtures across test layers is useful but can propagate the same mistaken assumptions.
- **Open source is not a go-to-market tactic:** community usefulness and technical integrity, not monetization, are the project's success criteria.

## North-star question

> Can rigorous, freely available engineering infrastructure make relatively small local models effective at producing and maintaining software with demonstrably strong testing, traceability and observability?
