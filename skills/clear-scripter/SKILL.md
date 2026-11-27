---
name: clear-scripter
description: Make Codex's shell and Python work easy to inspect by exposing purpose, scope, meaningful stages, and results in readable commands or helper scripts. Use when the user wants insight into session operations such as bootstrapping, discovery, diagnostics, transformations, or checks—not merely the final answer.
---

# Clear Scripter

Let the user see what Codex is doing and why while it works. Commands and helper scripts should make operational flow legible enough to audit and, secondarily, learn from without slowing the actual task.

Preserve the requested outcome, repository conventions, and the user's chosen environment. Visibility does not expand permission to inspect, modify, install, or contact anything outside the task's scope.

## Expose the work

Before a nontrivial command or helper, give a short statement of:

- its purpose;
- the scope it will inspect or change; and
- the meaningful result the user should expect.

Write the command so its stages remain visible in the tool call. For bootstrapping, discovery, and diagnosis, use labelled sections or separate commands when that makes it clear which context, files, configuration, or evidence came from where. Report important misses and empty results as well as hits; absence is often part of the diagnosis.

Afterward, summarize the operational result rather than only saying the command succeeded. Mention at most one useful scripting idiom unless the user asks for a tutorial.

Do not narrate every low-level action. A single familiar, read-only command can remain one line when its purpose is already clear from the surrounding update.

## Calibrate the form

- Use a readable multiline command or helper when the work has several transformations, complex quoting, loops, conditions, meaningful side effects, or likely reuse.
- Save a helper as a descriptively named file only when the user benefits from inspecting, editing, or rerunning it after the turn. Otherwise, keep it visible inline.
- Prefer separate commands when combining them would hide partial failure, provenance, or scope.
- Use the current shell unless the user requests another one. If portability matters, target the required environment rather than switching shells for style.
- Use the repository's declared language and tool versions. Check a version or command's availability only when the work depends on it.

## Choose clear tools

Prefer an installed modern tool when it makes the operation clearer, such as rg for text search, fd for file discovery, jq or yq for structured data, and uv or ruff for Python workflows. Do not run a broad tool inventory, introduce dependencies, or propose installations unless the current task would materially benefit.

Use shell for straightforward command orchestration. Use Python when parsing, data structures, error handling, or tests make it clearer than shell. Follow an existing project helper style when one is present.

## Write for a reader

For a nontrivial helper:

1. Start with a short purpose comment when the reason is not already stated in the surrounding update.
2. Put genuine user-adjustable settings near the top. Do not promote every literal to a constant.
3. Use names that describe the value or action: context_files, matching_projects, has_marker, print_summary.
4. Keep one conceptual step per line and separate discovery, validation, action, and reporting when those stages matter.
5. Comment non-obvious intent, safety, or portability—not code that already explains itself.
6. Prefer self-documenting flags when they are supported and no less portable for the target environment.
7. Label output when provenance or a bare value would otherwise be ambiguous.

For saved or multi-step Bash helpers, quote expansions and use arrays or null-delimited input when filenames may contain whitespace. Use set -euo pipefail only when its fail-fast semantics fit the task; explain any deliberately tolerated nonzero status.

For Python helpers, prefer pathlib and small functions when they simplify the code. Add type hints, argparse, dataclasses, or newer syntax only when they improve this helper; avoid scaffolding a throwaway script like a library.

If the user works in Fish or explicitly requests it, use native Fish idioms. Do not switch to Fish solely because it is available.

## Protect signal

Insight comes from structure and provenance, not volume. Avoid giant compound commands, opaque one-liners, full environment dumps, repetitive commentary, or raw output that does not help the user understand the operation.

## Coexist with other skills

This skill explains operational intent and command flow. If an I/O logging skill such as io-debug is also active, let it account for direction, target, amount touched, and running totals; do not duplicate that metadata here.

When another skill defines a workflow, apply these readability rules to its commands and helpers without changing its required sequence, evidence, or stopping conditions.

Do not change shell configuration, install tools, add project dependencies, or replace repository conventions without the user's request and any required authorization.
