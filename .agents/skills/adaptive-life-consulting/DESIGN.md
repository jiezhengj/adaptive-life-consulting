# Adaptive Life Consulting — Design

## Related Documents

Runtime behavior is defined by [`SKILL.md`](SKILL.md) and mirrored in [`SKILL.zh-CN.md`](SKILL.zh-CN.md). Refactor acceptance scenarios live in [`EVALS.md`](EVALS.md). Operational detail is split across [`references/`](references/). Project overview: [`../../../README.md`](../../../README.md).


## 1. Why This Document Exists

`SKILL.md` defines runtime behavior.

This document preserves the reasoning behind that behavior so future maintainers do not accidentally reintroduce failure modes that were already discovered and rejected.

A future refactor may reorganize wording and file structure, but it should not violate the design invariants below without explicitly documenting a protocol change.

---

## 2. Design Objective

The skill is intended to help with life and everyday-life problems that are:

- personally contextual;
- uncertain;
- multi-factor;
- partly dependent on external reality;
- often not reducible to a fixed questionnaire;
- sometimes choices, but sometimes diagnosis, planning, behavior change, or problem discovery.

The system should improve the quality of **belief and action under uncertainty**.

It should not maximize psychological interpretation, conversation length, or user profiling.

---

## 3. Design Invariants

### INV-01 — Solve the Real Problem

Do not optimize the user's initial framing without first checking whether the framing, option set, assumptions, success criteria, or time horizon are themselves wrong.

### INV-02 — Personalize Only Within the Problem Boundary

Do not attempt to fully model the user.

Personal information should be gathered only when it can materially affect the current case.

### INV-03 — Acquire Only Decision-Relevant Information

A question, research action, inspection, inference, or experiment should justify its expected information value.

Interesting information is not enough.

### INV-04 — Route Unknowns to the Best Evidence Source

Use ASK, RESEARCH, INSPECT, INFER, or TEST according to where reliable evidence actually exists.

### INV-05 — Do Not Force Introspection Where Reality Can Answer Better

If the user cannot reliably predict an unfamiliar experience, stop asking hypotheticals and design a real-world test.

### INV-06 — Preserve Epistemic Type

Facts, subjective experiences, values, preferences, behavior, inferences, hypotheses, and recommendations must not silently collapse into one another.

### INV-07 — Observed Behavior Usually Outweighs Abstract Self-Description

When behavior and trait labels disagree, investigate what actually happened.

### INV-08 — Hard Reality Can Dominate Psychological Fit

Health, safety, law, money, time, geography, eligibility, and opportunity structure may override a psychologically attractive option.

### INV-09 — Immediate-Action Confidence Is Not Long-Term-Conclusion Confidence

"This is the best thing to test next" is not equivalent to "this is definitely the best long-term choice."

### INV-10 — Chat Sessions Are Disposable; Cases Are Durable

When persistence is supported, the durable authority is the case state in the workspace, not a specific chat thread.

### INV-11 — Current State May Change; Material History Must Remain Traceable

Current case state is mutable. Historical evidence and conclusions that materially influenced reasoning must not be silently rewritten.

### INV-12 — Retrieval, Not Historical Preload

Past cases should be retrieved selectively when relevant, not injected wholesale as a global user profile.

### INV-13 — Hypotheses Never Silently Become Facts

A hypothesis may be strengthened, downgraded, rejected, or re-tested. It must not become a permanent user trait merely through repetition.

### INV-14 — Distinguish Time Change, Context Difference, and Contradiction

Different values across time or scope may both be true. Do not manufacture contradictions.

### INV-15 — Propagate Only Material Cross-Case Changes

Do not reopen every historical case after every new statement. Reopen only cases whose conclusions materially depended on changed evidence.

### INV-16 — Persistent Personalization Remains User-Inspectable and Correctable

The user should be able to inspect, correct, retire, delete, or prohibit reuse of persisted personal information where the host environment supports it.

### INV-17 — New Case Is Not the Same as Resume

Semantic similarity is insufficient to merge cases. Distinguish `new-case`, `resume`, `reopen`, and `related-new-case`.

### INV-18 — Split Cases Before Scope Drift Becomes Structural

If a subproblem develops its own goal, evidence, experiments, and lifecycle, it may deserve a child case.

### INV-19 — Consider Time Horizon Explicitly

Short-term and long-term optima may differ.

### INV-20 — Choice Problems Should Consider Status Quo and Delay

Do not assume the user must pick among the originally listed active options.

### INV-21 — Respect Stakeholder Perspective Boundaries

A user's belief about another person's thoughts is not automatically a fact about that person.

### INV-22 — Decision Quality and Outcome Quality Are Different

Retrospective review must distinguish whether the reasoning was sound at the time from whether the eventual outcome happened to be good or bad.

### INV-23 — Stop When Action Becomes More Informative Than Analysis

Consultation should end or pause when reality can now produce better evidence.

### INV-24 — Data Minimization Applies to Persistence Too

Do not preserve sensitive or irrelevant details merely because storage exists.

### INV-25 — Refactors Require Behavioral Regression, Not Visual Similarity

A shorter skill is not equivalent merely because it "sounds the same." Equivalence is judged through invariants and eval scenarios.

### INV-26 — Single-Question Cadence on Inquiry Turns

On any turn where the agent inquires or gathers information from the user, it must strictly ask exactly one substantive, decision-sensitive question to prevent user fatigue, cognitive overload, and multi-part assumption bias.

### INV-27 — Pure Professional Advisory Tone

The agent must maintain an objective, natural, and grounded consulting tone without roleplaying or referencing internal pedagogical character personas in user output.

### INV-28 — Four-Step Execution Progression & Precondition Gates

Every consultation must proceed through the sequential loop: Problem Form & Premise Deconstruction -> Competing Hypotheses Formulation -> Evidence Routing & Probing -> Actionable Delivery & Test Design. An experiment must never be designed before competing hypotheses exist.

### INV-29 — Anti-Premature Convergence by Action Cost

Low execution or trial cost is not an excuse to skip premise deconstruction and hypothesis discrimination. The agent must not collapse an adaptive consultation into a shallow recommendation simply because testing an option is cheap.

---

## 4. Non-Goals

This skill is not intended to:

- build a complete psychological profile;
- diagnose mental disorders;
- replace medical, legal, financial, or other domain professionals;
- maximize the number of interview questions;
- eliminate uncertainty before action;
- preserve every personal detail;
- make irreversible decisions on behalf of the user;
- make all historical cases consistent by forcing one permanent model of the user;
- treat generic population research as individualized truth;
- maintain a single eternal recommendation.

---

## 5. Core Architecture

The conceptual architecture is:

```text
Conversation / Event Stream
          |
          v
     Current Case State
          |
    +-----+------+----------------+
    |            |                |
    v            v                v
 Evidence     Experiments    Versioned Conclusions
    |
    v
Canonical Cross-Case Memory
    |
 selective retrieval / dependency impact
    v
Historical Cases
```

The important separation is:

- conversation = interaction;
- case state = active working model;
- artifacts = durable evidence and outputs;
- canonical memory = curated cross-case state;
- historical cases = selectively retrieved context.

---

## 6. Why Case State Exists

A long raw transcript contains:

- obsolete hypotheses;
- repeated statements;
- low-value side explorations;
- contradictory intermediate reasoning;
- stale external facts.

Even if the model context can technically hold the transcript, that does not make full-transcript preload the best working-memory design.

A compact case state should preserve the current frontier without pretending the historical transcript never existed.

---

## 7. Why Long-Term Memory Is Not Enough

Generic cross-session memory answers:

> What might be useful to remember about this user later?

Case state answers:

> Where has this specific investigation reached?

Those are different functions.

Case conclusions should not automatically become permanent user traits.

---

## 8. Why Real-World Experiments Are First-Class

Some variables are not reliably introspectable before experience.

Examples:

- whether a real commute becomes intolerable;
- whether a team culture feels acceptable;
- whether an activity's passive phases are boring;
- whether technical interest survives physical fatigue;
- whether a proposed routine is actually executable.

Repeated hypothetical questioning often produces false precision.

Experiments should be designed to discriminate among hypotheses rather than merely create "trial experiences."

---

## 9. Why Interaction Budgets Are Soft

Fixed question counts create two opposite errors:

- asking more after the decision frontier is already stable;
- stopping too early in genuinely complex cases.

Budgets therefore constrain exploration but do not create quotas.

Checkpoint logic is more important than exact question count.

---

## 10. Why Progressive Disclosure Is Preferred

The pre-refactor v3 accumulated many operational details in a single file.

That made the behavior explicit but created:

- duplicate rules;
- repeated stop logic;
- repeated persistence logic;
- repeated cross-case truth-maintenance logic;
- high runtime context load;
- greater risk that later patches would contradict earlier text.

The v4 bundle therefore keeps:

- runtime kernel in `SKILL.md`;
- rationale in `DESIGN.md`;
- behavioral regression in `EVALS.md`;
- detailed protocols in `references/`.

The goal is **runtime context slimming**, not deletion of important behavior.

---

## 11. Rejected Designs

### Rejected — Transcript-Only Memory

Reason:

Long-running consultation becomes dependent on one chat thread and accumulates stale reasoning.

### Rejected — Global User-Profile Preload

Reason:

Creates memory contamination, anchoring, and historical determinism.

### Rejected — Everything Goes Into Long-Term Memory

Reason:

Case-local conclusions and tentative hypotheses would become overgeneralized.

### Rejected — Every Turn Becomes a Markdown Transcript

Reason:

Duplicates host history and creates unnecessary storage and retrieval cost.

### Rejected — Fixed Questionnaire Length

Reason:

Question count is not a proxy for information gain.

### Rejected — Personality Matching as Primary Decision Method

Reason:

Traits often have weak discriminative power relative to constraints, behavior, and opportunity structure.

### Rejected — Research After Every User Answer

Reason:

Creates research decoration and false scientific precision without necessarily reducing uncertainty.

### Rejected — Silent Overwrite of Historical Evidence

Reason:

Destroys provenance and makes it impossible to understand why an earlier decision was made.

### Rejected — Reopen Every Historical Case After Every Memory Update

Reason:

Creates excessive propagation and over-association.

### Rejected — "Try Both and See"

Reason:

Underspecified experiments may test the wrong experience and produce low-value evidence.

### Rejected — Flat Declarative Rule Enumeration without Turn Progression

Reason:

Listing rules without an imperative state progression loop allows the LLM to skip critical premise deconstruction and hypothesis formulation steps under conversational pressure.

### Rejected — Low-Cost Trial as an Early Exit from Consultation

Reason:

Equating cheap trial cost with an immediate stop condition causes the agent to bypass deep context, attention curve, and historical dropout exploration for everyday decisions.

### Rejected — In-Band Negative Persona Constraints ("Pink Elephant" Prompting)

Reason:

Negative instructions like "do not roleplay Socrates" inadvertently increase token attention on those personas. Pedagogical metaphors must be purged entirely from the skill runtime and restricted to human documentation.

---

## 12. Design Lessons From the Originating Case

The original development case involved a long adaptive consultation about choosing an adult sport.

Important lessons included:

1. Hidden variables can matter more than obvious option differences.
2. Some behavioral history is highly informative.
3. Local ecosystem research can dominate abstract preference.
4. Generic research citations can create false precision when discriminative value is low.
5. The agent can recognize declining information value yet still continue asking unless a formal stop rule exists.
6. Repeated "I don't know" often means the variable is not introspectable.
7. "Best next experiment" can be much more certain than "best long-term option."
8. Local-source quality must be graded rather than treated uniformly.
9. Long conversations can drift in context even when the final answer remains plausible.
10. Case persistence and structured state are necessary if the work is expected to survive session loss.

These lessons are captured as invariants and evals rather than preserved as domain-specific sport rules.

---

## 13. Change-Control Policy

Future maintainers should treat protocol changes in three categories.

### Refactor

Behavior should remain equivalent.

Requirements:

- preserve all applicable invariants;
- pass regression evals;
- preserve or migrate schemas;
- preserve historical provenance.

### Feature Addition

Adds behavior without intentionally breaking existing semantics.

Requirements:

- add new invariants if necessary;
- add evals;
- document interaction with existing references.

### Breaking Protocol Change

Intentionally changes behavior or storage semantics.

Requirements:

- increment protocol version;
- document migration;
- explicitly identify affected invariants and evals;
- preserve archived previous versions.

---

## 14. Schema Migration Principle

Persistent workspaces may outlive any one version of the skill.

A future schema change must not assume all existing workspaces are new.

Before writing new-format state into an old workspace:

1. read workspace schema version;
2. determine compatibility;
3. migrate explicitly if required;
4. preserve backup or historical source;
5. update marker only after successful migration.

Detailed schemas live in `references/schemas.md`.

---

## 15. Final Design Principle

The system should become more useful over time without becoming more dogmatic about the user.

Accumulated history should improve evidence retrieval, not harden into an unquestioned identity model.
