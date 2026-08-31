x---
name: product-architecture-teardown
description: Reverse-engineer an AI or software product from user-provided evidence into a layered product architecture and interactive HTML report. Use for product teardown, Agent workflow analysis, or architecture reconstruction; do not use without inspectable product evidence.
metadata:
  short-description: Evidence-led product architecture teardown
---

# Product Architecture Teardown

Produce an evidence-led reconstruction of how a product works from a user's perspective and, where justified, how its architecture is likely organized. The goal is a reusable decision artifact, not a claim to know hidden implementation details.

## Non-negotiable evidence gate

Before analyzing, identify the product and verify that the user supplied at least one inspectable evidence source:

- a navigable product URL with permission to inspect it;
- screenshots or screen recordings that cover the relevant flow;
- source code, API documentation, exported project data, or an equivalent technical artifact;
- a combination of the above.

Do not begin a substantive teardown from a product name, marketing copy, assumptions, or generic knowledge alone. If evidence is absent or too narrow, state exactly what is missing and request the smallest useful addition. Examples: the flow from input to output, parameter panels, task/error states, asset/history views, or code entry points.

Treat the user's own platform access as authoritative. Use only read-only inspection unless the user explicitly authorizes a state-changing action. Never retrieve or expose passwords, cookies, tokens, browser storage, authorization headers, or private personal data. If login, verification, payment, or user takeover is required, hand control back to the user.

## Start by scoping the product

First establish, from evidence, what is being analyzed and what the user wants to learn. Classify the product into one or more modes; this changes the depth and focus of the teardown.

| Product mode | Prioritize |
|---|---|
| AIGC creation tool | Creative intake, style/asset reuse, generation stages, media models, quality checks, editing/export, usage billing |
| General execution Agent | Goal decomposition, planning, tool permissions, approvals, execution state, retries, observability, handoff |
| Conversational companion | Persona/memory boundaries, dialogue loop, safety/escalation, longitudinal preferences, trust and retention |
| Workflow/productivity application | Roles, forms, collaboration, rules, data ownership, integrations, audits, lifecycle states |
| Hybrid | Identify the dominant user loop, then analyze the secondary mode only where evidence shows it participates |

Record the classification as **confirmed**, **inferred**, or **unknown**. Do not force a product into a fixed Agent taxonomy; identify only agents, services, or roles actually supported by evidence.

## Evidence protocol

Create an evidence inventory before conclusions. Give each artifact a stable ID such as `E01`, `E02`, or `C01` (code). For every later claim, preserve the source ID and its observable cue: exact page wording, button text, visible asset, state label, API shape, or code location.

Use these labels everywhere:

- **【Confirmed】**: directly visible in a page, official product material, source code, API contract, or reproducible result.
- **【Inferred】**: follows from multiple confirmed observations but the implementation itself is not visible.
- **【Recommended】**: a proposed design improvement; never present it as current behavior.
- **【Unknown】**: evidence is insufficient or contradictory.

Rules of interpretation:

- An Agent saying “done” is not proof that an asset, task, or state was actually completed. Cross-check the canvas, task status, asset library, history, or output.
- Public “thinking”, “plan”, or “reasoning complete” text is a user-visible summary, not hidden chain-of-thought and not proof of a tool invocation.
- A button, field, or option proves a visible affordance, not necessarily a backend feature.
- If chat, task state, and canvas disagree, record all facts and mark the authoritative source as unknown.
- Do not infer infrastructure vendors, programming languages, databases, or model suppliers from UI behavior alone.

## Four-layer teardown procedure

Work from the user-facing system downward. Keep a running map of which layer owns each fact and which claims remain uncertain.

### 1. User layer — value, interaction, and decisions

Reconstruct one representative end-to-end user journey in chronological order:

1. Entry point, project/container, and visible user/account context.
2. User goal, initial input, uploads, defaults, and constraints.
3. System acknowledgement, planning, forms, and confirmation gates.
4. User choices: continue, modify, choose an alternative, retry, return, cancel, or publish.
5. Visible output, preview/edit/export/share entry points, and user feedback loop.
6. Friction: missing input, confusing defaults, long waits, state conflicts, error messages, incomplete outputs, or cost uncertainty.

For Agentic products, identify who appears to own each phase, when an Agent joins, who triggers it, and what handoff the user can see. For companion products, replace production stages with conversation, memory, safety, and re-engagement states.

### 2. Technical layer — applications, workflow, tools, and operations

Map the visible workbench and application modules first: project, conversation, task panel, canvas, form, asset card, editor, history, preview, export, sharing, billing, and errors. Include only modules evidenced by the product.

Then map the orchestration behavior:

- agent/service roles and their visible boundaries;
- triggers, approvals, handoffs, waiting states, cancellation, retry, and completion conditions;
- tool calls using functional names when official names are unavailable (label them “functional name, not official API”);
- async work, status updates, idempotency/duplicate protection, and error recovery only when evidenced or explicitly recommended.

For every Agent or stage, use an I/O contract:

| Field | Required content |
|---|---|
| Identity and goal | Visible role and user problem solved |
| Trigger | Event, prerequisite, or user confirmation |
| Inputs | User input, project context, upstream outputs, public knowledge, runtime results |
| Observable judgments | Completeness, routing, confirmation, validation, stop conditions |
| Tools | Functional tool name, preconditions, result, failure behavior, evidence |
| Outputs | User reply, components, structured fields, media assets, state, downstream work |
| Context read/write | Object/field, producer, consumer, update timing, evidence level |
| Completion and exceptions | Verifiable completion rule, failure, retry, interruption, handoff |

Never fabricate tool names or imply a hidden System Prompt. Summarize observable functional rules instead.

### 3. Model layer — model use, routing, evaluation, and guardrails

Identify the exact models and options shown in evidence: model name, quality tier, resolution, duration, language, reference-image ability, voice option, or cost display. If the UI only exposes one model, do not claim dynamic routing exists.

For each media or reasoning capability, record:

- confirmed model/configuration evidence;
- input payload categories and output media;
- observed constraints and error states;
- model selection or routing logic, if visible;
- quality checks users can see;
- unknowns and recommended controls.

Separate model behavior from product policy. Safety, authorization, billing, rate limiting, and privacy checks should be identified as deterministic service responsibilities when proposed; do not assume they live in an Agent prompt.

### 4. Data layer — context, assets, knowledge, and governance

Do not draw “global context” as a single opaque box. Organize data by scope and lifecycle. Use semantic names if real field names are unavailable and label them as inferred or recommended.

At minimum, check whether evidence supports these conceptual groups:

- **User context:** account/UI language, preferences, plan/credits, private assets, permissions.
- **Project configuration:** project identity, goal, style, format, model settings, confirmed parameters.
- **Working content:** source input/script, structured entities, intermediate outputs, prompts, dependencies.
- **Asset context:** images, video, audio, documents, references, source task, version, status, access scope.
- **Workflow state:** active stage, Agent/task status, confirmation, error, interruption, retry, handoff.
- **Billing/evaluation:** balance, estimate, hold, settlement, feedback, validation results, failure reasons.
- **Knowledge/public assets:** templates, styles, domain knowledge, capability registry, policy rules, public libraries.

For each group, show producer, consumer, write trigger, retention/versioning evidence, and any conflict observed. Explicitly distinguish reusable public knowledge, private user assets, and project-temporary data.

## Cross-layer synthesis

After all four layers, connect five flows through the same representative task:

1. User interaction flow.
2. Agent or workflow control flow.
3. Tool/service invocation flow.
4. Data/global-context flow.
5. Asset flow (for example text, image, video, audio, document, or external result).

At each step specify: initiator, handler, inputs read, decision, service/tool call, output, state write, confirmation gate, next owner, and failure branch. Use diagrams only where they make the relationship clearer than a table.

Then derive:

- **As-Is architecture:** only confirmed and clearly inferred components.
- **To-Be architecture:** necessary recommended components to resolve observed risks; keep them visually and semantically distinct.
- **Risk register:** prioritize user-impacting faults such as mismatched UI state, unverified completion, missing dependency invalidation, duplicate billing, orphan tasks, unsafe asset access, stale capability data, and silent quality failure.

## Required HTML delivery

Deliver the final teardown as a self-contained, readable HTML file. Do not use HTML merely as a wrapper around raw notes. It must include:

1. Executive summary and analysis scope.
2. Evidence inventory and evidence gaps.
3. Product classification and why the chosen focus applies.
4. User-layer journey and decision points.
5. Technical-layer modules, Agent/service I/O contracts, workflow/state model, and tool table.
6. Model-layer capability/routing/validation view.
7. Data-layer contexts, knowledge, asset, state, and governance map.
8. End-to-end five-flow table or diagram.
9. Product panoramic architecture diagram, arranged top-to-bottom by user → technical → model → data → governance/infrastructure when evidence supports it.
10. Entity/relationship or producer-consumer view when relationships are material.
11. Clearly separated As-Is and To-Be sections.
12. Risks, evidence traceability, and unanswered validation questions.

Make the HTML interactive only when it helps inspection: evidence-level filters, expandable node details, and a legend are useful defaults for a dense architecture map. Every architecture node and major claim must show its evidence ID and label. Use distinct line styles or colors for confirmed, inferred, recommended, and unknown elements. Avoid fake precision and never hide evidence gaps behind diagrams.

## Suggested output structure

Use this sequence unless the product type calls for a clearer variant:

1. Scope, product classification, and evidence readiness.
2. Evidence table and gaps.
3. User-layer journey map.
4. Technical-layer modules, agents, workflow, and tools.
5. Model-layer model/capability table.
6. Data-layer contexts, asset reuse, knowledge boundaries, and state ownership.
7. Five-flow end-to-end trace.
8. As-Is architecture, To-Be architecture, and principal risks.
9. Traceability table and next validation questions.

## Stop conditions

Stop and ask for evidence instead of continuing when:

- there is no inspectable product artifact;
- a required live page needs login, verification, payment, or user takeover;
- screenshots omit the relevant flow and no code/URL can fill the gap;
- the requested conclusion depends on inaccessible backend internals;
- the user requests a state-changing action without authorizing it.

When evidence is partial, complete the supported portion and label all boundaries, then list the smallest next evidence set needed to resolve the highest-impact unknowns.
