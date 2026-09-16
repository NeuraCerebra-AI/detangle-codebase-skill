# Strategic Decision Protocol

Use this protocol for the investigation and two-stage five-by-five decision. The goal is a defensible architecture derived from repository evidence, not an aesthetic rewrite.

## Define architectural quality

Treat bloat as unnecessary concepts, authorities, representations, execution paths, configuration states, abstractions, or lifecycles. Raw line count and file count are weak proxies.

Look for:

- duplicate or competing owners of the same invariant;
- parallel old/new implementations and compatibility paths without removal conditions;
- dead code, exports, dependencies, flags, tests, fixtures, and documentation;
- responsibilities placed outside their natural transaction, lifecycle, runtime, or dependency owner;
- circular, inverted, or leaky dependencies;
- giant modules with multiple independent authorities;
- tiny wrapper layers that obscure rather than protect a real boundary;
- speculative interfaces, factories, managers, adapters, and extension points;
- repeated logic without one authoritative owner;
- multiple representations of the same state without a necessary boundary;
- writers without admission, recovery, settlement, retention, cleanup, or failure behavior;
- folder structures and names that conceal the actual system;
- tests that certify implementation ceremony or obsolete behavior instead of meaningful contracts.

Do not assume that improvement means more modules. The correct result may split a mixed owner, combine fragmented non-owners, move behavior to its direct owner, delete an abstraction, or leave a large cohesive module intact.

## Build the evidence dossier

The root defines bounded leaf assignments. Each leaf returns:

1. observed facts with file/symbol locations;
2. current owners and dependency directions;
3. invariants and externally visible behavior at risk;
4. concrete bloat or organization findings;
5. proposed moves and deletions;
6. moves it considered and rejected;
7. validation implications;
8. uncertainty and confidence.

Leaves do not choose the winner. The root reconciles disagreement against primary repository evidence.

Before proposing architecture, produce a compact current-state model covering entrypoints, major modules, dependency flow, state ownership, runtime boundaries, build/test/deploy paths, and known transition seams. Distinguish confirmed facts from inferences.

## Generate five high-level approaches

Create five materially different organizing theses. A difference in naming, phase count, or folder layout is not a distinct approach.

For each approach provide:

- central architectural thesis;
- proposed owners and permitted dependency direction;
- major moves, merges, splits, and deletions;
- canonical representations and execution paths;
- migration and compatibility posture;
- behavior intentionally preserved;
- first-, second-, and third-order consequences;
- user/developer workflow effects;
- principal failure modes;
- reversibility and validation strategy;
- evidence supporting and contradicting it.

## Eliminate and score

Eliminate an approach before scoring if it violates a hard repository constraint, creates two authorities without a bounded transition, depends on unauthorized external changes, cannot preserve required data or behavior, or has no credible validation path.

Score remaining approaches from 1 to 5 on one published rubric:

- invariant and behavior preservation — 25%;
- reduction of accidental complexity and duplicate authority — 20%;
- cohesion, ownership clarity, and dependency direction — 15%;
- lifecycle completeness and operational correctness — 10%;
- migration safety and reversibility — 10%;
- testability and confidence attainable — 10%;
- user/developer workflow clarity — 5%;
- implementation cost and disruption — 5%.

Explain every score with evidence. Scores aid judgment; they do not replace it. Select the approach with the strongest evidence-adjusted outcome, and record why each alternative lost.

## Generate five implementation plans

For the winning approach, produce five materially different execution strategies. Examples of real differences include vertical versus owner-by-owner migration, compatibility bridge versus flag-free replacement, dependency-first versus consumer-first sequencing, and incremental convergence versus one bounded cutover.

Each plan must define:

- ordered coherent slices;
- exact responsibility moved in each slice;
- likely modules or directories affected;
- expected deletions and when they become safe;
- treatment of data, API, configuration, and historical compatibility;
- regression coverage and validation by slice;
- documentation consequences;
- rollback or recovery posture;
- concurrency and integration risk;
- point at which the old path loses authority;
- definition of complete.

Score the plans using invariant safety, convergence to one authority, reversibility, ease of validation, deletion completeness, integration risk, and total disruption.

## Falsify the leader

Give the leading approach and plan, their evidence, and their assumptions to a direct read-only adversarial leaf. Ask it to find:

- hidden behavior changes;
- missed owners or consumers;
- invalid dependency assumptions;
- lifecycle gaps;
- migration traps;
- tests that would give false confidence;
- ways the plan adds more bloat than it removes;
- a credible reason a rejected alternative is superior.

The root resolves every material challenge. Revise scores when evidence changes them. Freeze the final decision with its assumptions, non-goals, hard constraints, slice order, deletion targets, validation plan, and explicit reasons for rejecting alternatives.
