# Claude Code Host Adapter

This adapter maps the shared Detangle Codebase skill to Claude Code. Codex uses the sibling `codex.md` adapter; both execute the same strategic and implementation workflow.

- Invoke explicitly with `/detangle execute ...` or `/detangle resume ...` when mutations are intended.
- Follow active Claude Code system, `CLAUDE.md`, repository, permission, sandbox, Git, and tool instructions. They override this adapter.
- Use direct Claude Code subagents for evidence lanes. Prefer read-only Explore-style agents when available; do not use agent teams or create nested workers for this workflow.
- Keep synthesis, architectural selection, integration, every edit, Git mutation, commit, and final acceptance in the parent Claude Code session.
- Pass the absolute `RUN_ROOT` and the leaf contract in every subagent request. Require each leaf to verify its repository root before inspection.
- When Claude Code exposes tool restrictions for a child, omit delegation and mutation tools. The textual leaf contract remains mandatory.
- Use Claude Code's native worktree support when the session already has an isolated checkout. Otherwise follow the common isolation contract and active Git instructions.
- Wait for every requested leaf result and reconcile the evidence before selecting an architecture.

Claude Code-specific tool names are implementation details. If they change, preserve the roles and boundaries in the shared skill rather than changing its architecture.
