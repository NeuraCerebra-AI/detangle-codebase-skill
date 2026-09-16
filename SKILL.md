---
name: architecture-transform
description: Analyze and, when explicitly authorized, execute a repository-wide architectural cleanup that reduces bloat, clarifies ownership, removes obsolete paths, and reorganizes code without changing intended behavior. Use for whole-repository simplification or rearchitecture, not ordinary bug fixes or small feature work.
---

# Architecture Transform

Find and implement the smallest coherent architecture that satisfies the repository's purpose. Optimize for clear ownership, one canonical path, low accidental complexity, completed lifecycles, and ease of change. Do not optimize for fewer lines, more files, fashionable patterns, or abstraction by itself.

This skill is repository-agnostic. Discover the repository's language, framework, conventions, authority, tests, documentation, and release boundaries at runtime. Repository-local instructions override this skill.

## Adapt to Codex or Claude Code

This is one shared skill for both Codex and Claude Code. Detect the current host from the active system instructions and available native tools; do not ask when it is already clear. Before creating Git state or delegating work, read exactly one host adapter:

- In Codex, read [references/hosts/codex.md](references/hosts/codex.md).
- In Claude Code, read [references/hosts/claude-code.md](references/hosts/claude-code.md).

The host adapter maps this common workflow to native tools. It must not change the decision method, authority boundary, root-only write rule, or completion standard.

## Invocation and authority

Supported modes:

- `review`: investigate, compare five architectural approaches, and produce a winning implementation plan without changing files or Git state.
- `execute`: create isolated Git state, perform the full decision process, implement the winning plan, validate it, update required documentation, and make local commits.
- `resume`: continue a previously recorded execution after revalidating its identity and state.

Treat an ambiguous invocation as `review`. Only an explicit `execute` or `resume` authorizes mutations. Invocation examples are `$architecture-transform execute ...` in Codex and `/architecture-transform execute ...` in Claude Code.

`execute` authorizes only local branch/worktree creation, repository edits, local validation, required local documentation, and local commits. It does not authorize fetching or pulling, pushing, opening a pull request, merging, deploying, calling paid or live providers, accessing production or customer data, changing external systems, or claiming release readiness.

## Establish the transformation contract

Before mutation, establish and record:

- the concrete objective and why it matters;
- observable success conditions;
- explicit non-goals and forbidden changes;
- applicable repository instructions and architectural invariants;
- exact 40-character base commit;
- current branch, worktree inventory, and dirty/untracked paths;
- whether each dirty path is included or excluded;
- the isolated branch, absolute execution root (`RUN_ROOT`), and allowed side effects;
- baseline and final validation commands derived from repository authority.

For `execute` or `resume`, read [references/execution-contract.md](references/execution-contract.md) during preflight and create the run record before any Git mutation.

Default to the current committed `HEAD` only when doing so is unambiguous. For a whole-repository transformation, treat every dirty path as potentially relevant until classified. If relevant uncommitted work exists, stop before Git mutation, list the paths, and ask the user to choose between an exact committed base that excludes them and an explicitly prepared snapshot that includes them. Never stash, commit, copy, discard, or absorb user changes implicitly.

## Isolate execution

For `execute`, create a new isolated worktree by default. Reuse the current worktree only when explicit host metadata or the user identifies it as an isolated disposable worktree for this run. Respect repository and host branch policy; otherwise use `architecture-transform-<short-slug>-<date>` with a unique suffix when needed.

Before creating Git state, verify that neither the branch nor worktree path already exists. When creating a worktree, use the recorded base commit, choose a unique persistent path outside the original checkout, create the branch there, and verify all three before editing:

1. `git rev-parse --show-toplevel` equals `RUN_ROOT`.
2. `git rev-parse HEAD` equals the recorded base commit.
3. `git branch --show-current` equals the recorded branch.

Use `RUN_ROOT` as the working directory for every root and leaf operation. Never switch or modify the original checkout. Never reset, clean, stash, rebase, amend, force-push, prune worktrees, delete a worktree, or rewrite history.

## Use a flat agent team

Use direct leaf subagents for a repository-wide transformation when delegation is available. If capacity is limited, run them in batches. If delegation is unavailable, perform the lanes directly and disclose that fact.

Only the root coordinator may delegate. The root owns Git state, synthesis, architectural decisions, every edit and deletion, staging, commits, failure triage, and final acceptance. Leaf agents gather evidence only and remain read-only.

Begin every leaf prompt with this exact contract:

> You are a direct leaf evidence agent. Do not spawn, fork, create, hand off, or delegate to another agent or task. Do not edit files, stage changes, create Git state, or access live, paid, deployment, production, customer, or external systems. Read all applicable repository instructions yourself. Work only inside the supplied absolute RUN_ROOT, verify it with `git rev-parse --show-toplevel`, and return only the requested evidence to the root coordinator.

When the host exposes an agent-tree view, inspect it after dispatch and before mutation. Interrupt any unauthorized descendant and disregard unverified descendant output. This is instruction-level enforcement; if the host supports child tool restrictions, also withhold delegation and mutation tools from leaves.

## Investigate and decide

Read [references/strategic-decision.md](references/strategic-decision.md) before investigation and synthesis.

Use four evidence lanes:

1. architecture, dependency direction, entrypoints, and canonical ownership;
2. domain invariants, data, transactions, security, and lifecycle custody;
3. tests, runtime, build, operations, migration, and release constraints;
4. dead code, duplicate paths, unnecessary abstractions, organizational friction, and workflow consequences.

When available, use `deep-reasoning-protocol`, `domino-protocol`, and `ship-workflow-clarity` as supporting lenses. Their absence must not block the run; apply equivalent assumption testing, cascade analysis, and workflow-clarity analysis directly.

The root must synthesize exactly five materially distinct, high-quality architectural approaches, eliminate any that violate hard constraints, evaluate the survivors with one consistent rubric, and select the strongest approach. Then create exactly five materially distinct implementation plans for that approach, evaluate them consistently, adversarially falsify the leader, and freeze the winning plan.

Do not invent superficial variants to fill either set. If five defensible choices truly cannot be produced, explain why, provide every defensible choice, and do not disguise filler as an alternative.

In `execute` mode, continue automatically after the decision is frozen. Pause only when proceeding requires new authority, changes the objective, or reaches a stop condition.

## Execute to convergence

For `execute` or `resume`, follow the execution contract loaded during preflight.

Implement the winning plan in small coherent slices. Prefer the existing direct owner of a responsibility. A new module or abstraction must own a distinct responsibility that cannot safely remain with an existing owner. Generalize only from current pressure, preserve one canonical representation and execution path, complete every introduced lifecycle, and remove superseded machinery after convergence.

Only the root edits. Do not parallelize writes. For every slice, name the owner and preserved invariant, define the intended files and deletions, implement the change with regression coverage, run proportional validation, inspect the diff for accidental scope and duplicate authority, update required documentation, stage explicit paths, inspect the staged diff, and commit only a coherent green slice.

## Stop conditions

Stop and report rather than guessing when:

- the intended base or ownership of relevant dirty changes is unclear;
- the baseline is not trustworthy;
- repository authority conflicts and primary sources do not resolve it;
- the winning plan would violate a hard invariant or silently change intended behavior;
- work requires destructive migration, external access, or ungranted release authority;
- safe validation cannot establish the affected behavior;
- a second bounded replanning attempt is required.

Preserve partial work for `resume`. Never clean it up automatically.

## Completion

Complete only after the winning plan has converged, obsolete machinery has been searched for and removed or explicitly retained with justification, repository-required documentation is current, and the accumulated local candidate passes the applicable gates.

Report the objective, discovered architecture, five approaches, winning approach, five plans, winning plan, base commit, branch, `RUN_ROOT`, commits, important moves and deletions, validation evidence, remaining uncertainty, deferred work, and exact authorization required for any next action. Stop locally.
