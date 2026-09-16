# Execution Contract

Use this protocol from preflight through completion. After the strategic decision is frozen, execution must converge to the selected architecture without turning the repository into a permanent transition state.

## State and checkpoints

Use this state machine:

`preflight -> isolated -> investigated -> decision-frozen -> slice-N -> final-review -> completed`

Read this contract during preflight. Create the run record before branch or worktree creation, then checkpoint every transition. Prefer a repository-approved ignored ledger when one exists. Otherwise store a non-secret local run record beneath the repository's Git common directory, in a `detangle/` directory, so it remains outside commits but is reachable from the worktree. Record:

- run identifier and objective;
- original checkout and `RUN_ROOT`;
- base commit and branch;
- dirty paths that were explicitly excluded;
- current state and frozen decision;
- intended files for the active slice;
- last known-green commit;
- validation commands and receipts;
- open findings, deferrals, and next action.

Never store secrets, provider output, customer data, or large source excerpts in the run record. Retain the record and worktree for `resume`; removal is a separate user action.

For `resume`, reread repository authority and verify the recorded root, branch, base ancestry, current `HEAD`, absence of an in-progress merge/rebase/cherry-pick, and last known-green commit. A clean tracked state may resume. Checkpointed uncommitted work may also resume only when the current changed-path set is a subset of the recorded active slice, every changed path is expected, and the diff matches the recorded purpose; revalidate that slice before continuing or committing it. Stop on unknown paths, unexplained changes, or identity mismatch instead of repairing Git state implicitly.

## Baseline

Before running any discovered validation command, inspect its script, hooks, configuration, environment expectations, and likely side effects. Use only safe local/offline variants, suppress known credentials and live-provider configuration, and refuse commands that may deploy, mutate external state, contact paid/live services, or use production/customer data. If an offline validation route cannot be established, stop and report the gap.

Before edits, run the smallest safe repository-authoritative checks that establish a trustworthy baseline for the surfaces likely to change. Record existing failures separately. A red baseline is not permission to repair unrelated failures.

For a repository-wide transformation, identify the accumulated candidate gate early, but run expensive full gates only when repository policy or changed risk warrants them.

## Slice contract

Each slice must be independently comprehensible and move the repository toward the frozen architecture. Before editing, state:

- the responsibility and direct owner;
- the invariant or behavior preserved;
- the current problem;
- files allowed to change;
- expected moves and deletions;
- proof that the slice is complete;
- focused validation.

Then:

1. inspect all known callers, consumers, tests, configuration, and documentation;
2. add or update behavior-focused regression coverage when warranted;
3. make root-only edits;
4. run focused validation;
5. inspect working diff and dependency direction;
6. remove newly superseded paths immediately when safe;
7. update required living documentation;
8. stage explicit files only, never broad globs;
9. inspect the staged diff and staged file list;
10. commit only a coherent green slice using repository conventions;
11. update the checkpoint.

Do not add a compatibility seam unless an actual external constraint requires it. Every temporary seam must name its owner, authoritative replacement, removal trigger, and validation. Do not leave old and new paths jointly authoritative.

## Failure handling

- Leaf failure: retry with another direct leaf or continue without that evidence; do not broaden scope.
- Evidence conflict: resolve from primary sources and record uncertainty when it remains.
- Focused test failure: repair the active slice or abandon it without disturbing earlier green slices.
- Unexpected cross-cutting impact: pause the slice and re-evaluate its boundary.
- Hard-invariant, security, custody, data-loss, or unauthorized-side-effect risk: halt.
- Plan invalidated by new evidence: return once to strategic synthesis, record why, repeat the falsification check, and freeze one revised plan.
- Second plan invalidation: stop for user direction.

Never use destructive Git recovery. For a bad committed slice, prefer an explicit local revert when appropriate. For unsafe uncommitted work, preserve it and use a fresh recovery worktree from the last known-green commit rather than mass-restoring or deleting files.

## Convergence and subtraction

After the new path is verified, search deliberately for:

- superseded callers and exports;
- duplicate models and representations;
- dead flags, adapters, wrappers, and compatibility code;
- obsolete fixtures and tests;
- stale commands, diagrams, and documentation;
- dependencies made unnecessary by the transformation.

Delete what no longer owns current behavior. Preserve only externally required compatibility, immutable migrations/history, or historical-data readability, with an explicit justification.

## Final review

Run the repository-defined accumulated local gates once on the final semantic candidate, plus only boundary-relevant build, browser, security, migration, or packaging checks.

Use direct read-only leaves for three final reviews:

1. invariant, security, data, and lifecycle custody;
2. architecture, dependency direction, bloat, and subtraction completeness;
3. regression coverage, validation evidence, and documentation accuracy.

The root verifies every material finding from source, fixes confirmed issues, and repeats only checks invalidated by those fixes.

Complete when the repository has one coherent authoritative architecture for the transformed scope, required behavior is preserved, no unintended transition machinery remains, documentation matches reality, the relevant gates pass, and the local branch is reviewable. Do not push, merge, deploy, or claim production readiness.
