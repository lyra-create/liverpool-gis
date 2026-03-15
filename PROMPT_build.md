0a. Study `specs/*` using up to 10 parallel subagents to learn the application specifications.
0b. Study `IMPLEMENTATION_PLAN.md` to understand the current task list and priorities.
0c. For reference, application source is in `src/`. Shared utilities are in `src/lib/` if present.

1. Choose the single most important *incomplete* task from `IMPLEMENTATION_PLAN.md`. Before making changes, search the codebase to confirm the feature isn't already implemented — don't assume it's missing. Implement the task completely. No placeholders. No stubs. If functionality is missing, it is your job to add it per the specifications.

2. After implementing, run the validation commands from `AGENTS.md` (build, typecheck, tests, lint). Use only 1 subagent for build/test execution (backpressure). If they fail, fix the issues before committing. Ultrathink on failures.

3. When validation passes: `git add -A && git commit -m "feat: [concise description of changes]"`. Update `IMPLEMENTATION_PLAN.md` to mark the completed task done (use `[x]` prefix or strikethrough). Note any discoveries or follow-up bugs found.

4. Output "TASK COMPLETE: [task name]" on the final line. If there are no incomplete tasks remaining in `IMPLEMENTATION_PLAN.md`, write "ALL TASKS COMPLETE" to the top of that file and output "ALL TASKS COMPLETE" as the final line instead.

99999. When authoring documentation or tests, capture the *why* — tests and implementation importance.
999999. Single sources of truth. No migrations, no adapters. Consolidate to `src/lib/` where possible.
9999999. For any bugs discovered unrelated to your current task, document them in `IMPLEMENTATION_PLAN.md` using a subagent — do not fix them now.
99999999. Keep `IMPLEMENTATION_PLAN.md` updated with discoveries and mark completed items done. Remove completed items periodically when the file becomes large.
999999999. Keep `AGENTS.md` operational only (build/run/test commands, codebase patterns). No status or progress notes.
9999999999. If you find inconsistencies in `specs/*`, use extended reasoning to update the spec file, then document the plan change in `IMPLEMENTATION_PLAN.md`.
99999999999. Implement completely. Placeholders and stubs waste future loops redoing the same work.
