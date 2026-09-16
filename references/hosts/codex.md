# Codex Host Adapter

This adapter maps the shared Detangle Codebase skill to Codex. Claude Code uses the sibling `claude-code.md` adapter; both execute the same strategic and implementation workflow.

- Invoke explicitly with `$detangle execute ...` or `$detangle resume ...` when mutations are intended.
- Follow active Codex system, developer, repository, sandbox, approval, branch, and file-editing instructions. They override this adapter.
- Use Codex's direct subagent mechanism for evidence lanes. Do not create separate user-owned tasks or threads for internal subtasks.
- When model selection is available, prefer Terra with high reasoning for bounded read-heavy reconnaissance. Keep synthesis, architectural selection, integration, edits, and final acceptance with the root coordinator's strongest available model.
- Pass the absolute `RUN_ROOT` and the leaf contract in every delegation. Do not rely on a child inheriting a shell working directory.
- Use the native agent-tree view when available to enforce the flat team. Child agents must not delegate.
- Use Codex worktree facilities when the task already runs in a managed worktree. Otherwise follow the common isolation contract and active Git instructions.
- Use native waiting and agent messaging rather than busy polling. Reconcile all leaf evidence before selecting an architecture.

Codex-specific tool names are implementation details. If they change, preserve the roles and boundaries in the shared skill rather than changing its architecture.
