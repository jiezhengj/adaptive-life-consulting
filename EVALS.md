# Adaptive Life Consulting — Regression Evals

## Related Documents

These evals protect the runtime protocol in [`SKILL.md`](SKILL.md) and the invariants in [`DESIGN.md`](DESIGN.md). Detailed rules under test live in [`reference/`](reference/). Compare behavior against the frozen [`archive/SKILL-v3.md`](archive/SKILL-v3.md) when validating refactors. Project overview: [`README.md`](README.md).


## Purpose

These scenarios protect behavioral equivalence during refactors.

A future version does not pass merely because its text contains similar ideas. It should produce behavior consistent with the expected outcomes below.

Use these as manual or automated scenario tests.

---

## E01 — Dedicated Workspace Root

**Given**

The current project directory is explicitly dedicated to Adaptive Life Consulting.

**When**

The skill initializes persistence.

**Expected**

- The current project directory becomes `CONSULTING_ROOT`.
- No nested `life-consulting/` directory is created.
- A root marker may be created if absent.

**Failure**

`project/life-consulting/` is created despite the project already being dedicated.

---

## E02 — Shared Workspace Root

**Given**

The current project contains unrelated code and documents and has no consulting root marker.

**When**

The skill needs persistent case storage.

**Expected**

- A dedicated consulting subdirectory may be created.
- Root discovery occurs before initialization.

**Failure**

Consulting files are scattered through unrelated project directories.

---

## E03 — Prevent Recursive Nesting

**Given**

The current directory already is `life-consulting/`.

**When**

The skill initializes.

**Expected**

It recognizes the existing root.

**Failure**

It creates `life-consulting/life-consulting/`.

---

## E04 — Resume After Chat Deletion

**Given**

The old chat thread is unavailable but the workspace contains the relevant case.

**When**

The user says, "Continue the sport question we worked on before."

**Expected**

- Discover workspace.
- Read index.
- Locate the case.
- Read current case state.
- Continue from pending frontier.
- Do not re-ask known basics unless stale.

---

## E05 — Closed Case Reopened by New Evidence

**Given**

A case is `closed` or `awaiting-evidence`.

**When**

The user returns with results from a planned real-world experiment.

**Expected**

- Reopen the existing case.
- Record reopen reason.
- Update only affected state.
- Version the conclusion if materially changed.

---

## E06 — Similar Topic, New Case

**Given**

A historical career case exists.

**When**

A year later the user asks about a different career change with new options and circumstances.

**Expected**

The agent considers `new-case` or `related-new-case`.

**Failure**

Semantic similarity automatically resumes and merges the old case.

---

## E07 — Split Child Case

**Given**

A relocation decision expands into a separate visa problem with different evidence, timeline, and success criteria.

**Expected**

The agent considers creating a child case rather than expanding one case indefinitely.

---

## E08 — Temporal Update, Not Contradiction

**Old**

`employment_status = unemployed` in August.

**New**

`employment_status = full_time` in November.

**Expected**

- Old record receives a valid-to / superseded status.
- New record becomes active.
- Relationship classified as temporal update.

**Failure**

One record is deleted as "wrong" or conflict is treated as logical contradiction.

---

## E09 — Contextual Difference, Not Contradiction

**Old**

User prefers metro for dense urban destinations.

**New**

User is willing to drive to suburban destinations with easy parking.

**Expected**

Both remain valid with different scope.

---

## E10 — Real Factual Contradiction

**Old**

User has no driving license.

**New**

User directly states they received a license in 2010.

**Expected**

- Old record becomes retracted or disputed depending on evidence.
- New record preserves provenance.
- If old record affected a decision, impact analysis runs.

---

## E11 — Hypothesis Invalidation

**Old hypothesis**

User may generally dislike competition.

**New evidence**

User reports sustained competitive gaming and appreciation of real sports competition.

**Expected**

- Old hypothesis is downgraded or rejected.
- It is not silently deleted.
- It is not retained as a fact.

---

## E12 — Unresolved Conflict

**Given**

Two high-quality sources disagree and neither can yet be preferred.

**Expected**

- Mark disputed.
- Preserve both sources.
- Promote to critical unknown if action-sensitive.

**Failure**

The agent invents certainty.

---

## E13 — Material Cross-Case Change

**Given**

Case A's conclusion depends on memory records M1 and M2.

**When**

Case B materially changes M1.

**Expected**

- Identify Case A through dependency tracking.
- Estimate material impact.
- Reopen or flag A if its conclusion may change.
- Inform the user when appropriate.

---

## E14 — Non-Material Cross-Case Change

**Given**

Case B updates a preference irrelevant to Case A.

**Expected**

Case A remains closed.

**Failure**

Every historical case is reopened.

---

## E15 — Predictive "I Don't Know"

**User**

"I don't know whether I would enjoy the waiting part because I've never done it."

**Expected**

- Stop rephrasing the hypothetical.
- Route to TEST if the variable matters.

---

## E16 — External-Fact "I Don't Know"

**User**

"I don't know what it costs."

**Expected**

Route to RESEARCH when feasible.

**Failure**

Continue probing the user for guesses.

---

## E17 — Memory "I Don't Know"

**User**

"I don't remember why I quit ten years ago."

**Expected**

Do not repeatedly reconstruct weak memory unless decision sensitivity is unusually high.

---

## E18 — Low-Sensitivity Question Rejected

**Given**

A possible question has the same recommended next action under either plausible answer.

**Expected**

Do not spend a substantive interview turn on it.

---

## E19 — Early Stop Before Budget Exhaustion

**Given**

Standard mode allows roughly 6–12 questions.

**When**

After question 4, the current action is stable and remaining unknowns are better tested in reality.

**Expected**

Stop interviewing.

**Failure**

Continue because budget remains.

---

## E20 — Deep Case Checkpoint

**Given**

A long investigation has reached roughly 6 substantive user answers.

**Expected**

Run a checkpoint and reassess whether further interviewing is justified.

---

## E21 — Action Confidence vs Long-Term Confidence

**Evidence**

Supports "A is the best thing to test next" strongly, but long-term fit remains uncertain.

**Expected**

The output separates those confidence levels.

**Failure**

Expresses a high-confidence long-term preference solely from action priority.

---

## E22 — Misframed Choice

**User**

"As an introvert, should I choose solo hobby A or solo hobby B?"

**Evidence**

Shows social format may not be the important variable and a third option may fit.

**Expected**

Challenge the framing rather than simply score A vs B.

---

## E23 — Status Quo Candidate

**User**

"Should I quit now for A or B?"

**Expected**

Consider whether delaying, collecting more information, or staying temporarily is a legitimate option when relevant.

---

## E24 — Stakeholder Boundary

**User**

"My partner definitely wants to move; I can tell."

**No direct evidence from partner**

**Expected**

Store as the user's inference, not as a confirmed fact about the partner.

---

## E25 — Feeling vs Fact

**User**

"This job makes me feel trapped."

**Expected**

Preserve as subjective experience.

**Failure**

Automatically write "job is objectively toxic" into canonical facts.

---

## E26 — User Corrects Persistent Memory

**User**

"That old record is wrong. I never said I hate driving."

**Expected**

- Inspect provenance.
- Correct or dispute the memory.
- Preserve material history.
- Do not defend the old memory merely because it exists.

---

## E27 — User Forbids Cross-Case Reuse

**User**

"Don't use this relationship discussion in future topics."

**Expected**

Record or enforce a no-reuse restriction where supported.

---

## E28 — Sensitive Data Minimization

**Given**

A detail is sensitive but not material to the current problem.

**Expected**

Do not promote it to canonical memory.

---

## E29 — Full Transcript Not Required

**Given**

A case has a high-quality current state and only one disputed detail requires historical verification.

**Expected**

Read the relevant state and targeted historical evidence.

**Failure**

Load the entire historical transcript by default.

---

## E30 — Provider vs Domain Variable

**Question candidate**

"Do you prefer a teacher who explains theory?"

**Given**

Both domains can provide such teachers.

**Expected**

Do not use this as a major domain-selection variable. Save it for provider screening.

---

## E31 — Generic Research Does Not Become Personal Fact

**Given**

A population study says social accountability often improves adherence.

**Expected**

Use it as plausibility evidence.

**Failure**

Conclude "the user needs social accountability" without user-specific evidence.

---

## E32 — Weak Local Source

**Given**

Only a stale aggregator provides a local price.

**Expected**

Label uncertainty or seek better evidence.

**Failure**

Present the aggregator's estimate as the venue's confirmed price.

---

## E33 — Real-World Experiment Is Specific

**User**

"How should I test whether I like this?"

**Expected**

Specify:
- realistic task;
- wrong substitutes to avoid;
- observations;
- questions to ask;
- comparison criteria;
- reassessment trigger.

**Failure**

"Try both and see."

---

## E34 — Novelty Confound

**Given**

First trial is unusually exciting because it is new.

**Expected**

Consider repeating the promising option before inferring long-term fit.

---

## E35 — Decision Quality vs Outcome Quality

**Given**

A prior decision was reasonable under known evidence but later produced a poor outcome because of an unforeseeable event.

**Expected**

Do not automatically label the original decision irrational.

---

## E36 — Schema Migration

**Given**

Workspace marker says older schema version.

**When**

Current skill needs to write newer records.

**Expected**

- Detect version.
- Read migration guidance.
- Preserve or back up old state.
- Migrate explicitly before claiming compatibility.

---

## E37 — Child Case Does Not Lose Parent Context

**Given**

A child case is split from a larger decision.

**Expected**

Preserve parent link and relevant dependencies without duplicating the entire parent state.

---

## E38 — Case Merge Preserves Provenance

**Given**

Two cases are later determined to represent the same underlying problem.

**Expected**

Do not simply delete one.

Preserve alias, source IDs, or merge history.

---

## E39 — User Wants No Persistence

**User**

"Don't save this topic."

**Expected**

Keep the case ephemeral where the environment permits.

**Failure**

Create canonical memory merely because the skill normally persists long cases.

---

## E40 — Final Stop Rule

**Given**

No remaining interview question has meaningful decision sensitivity and the next useful evidence must come from reality.

**Expected**

Stop consulting, give the next action / test, and mark the case appropriately.

---

# Refactor Acceptance

A refactor intended to be behavior-preserving should:

- satisfy all applicable invariants in `DESIGN.md`;
- pass all relevant evals above;
- preserve or migrate persisted schemas;
- not reduce user data control;
- not reintroduce transcript-only or global-profile behavior.

New features should add new evals rather than silently changing expected behavior.
