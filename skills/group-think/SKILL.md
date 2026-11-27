---
name: group-think
description: Consult Greg's Library before starting any work that depends on how a tool, model, box, or interface actually behaves. Use at the start of a task, before reading code or writing scripts, and again whenever a step fails for a reason the library might already explain. Do not use to store agent memory, personality, or project state there.
---

# group-think

Greg's Library at `~/devhome/library` is the shared memory none of us owns. Check it
before working, so the same thing is not learned three times by three agents at three
different costs.

## Why this exists

2026-09-27: asked to drive a local model, I hand-rolled `curl` against the router,
timed out twice at five minutes each, and started building a latency benchmark. The
answer — `hermes -z "<prompt>" -m <model> --worktree` — was in `hermes --help` the
whole time. Greg: *"look at what u and codex did before, read the directions, use the
fucking normal interface."*

Nothing about that was a knowledge problem. It was a **looking** problem. The library
is where the looking goes, once, instead of being repeated.

## The rule

**Before any work that depends on how something behaves — a tool, a model, a box, an
interface, a wire format — read the library first.** Before reading source, before
writing a script, before inventing a mechanism.

Then again at the first unexpected failure. A step that fails for a reason the library
already explains is the exact cost this skill exists to prevent.

## How to consult it

The catalog is small and cheap; read it before grepping.

```fish
bat ~/devhome/library/index.md            # every page, one line each
bat ~/devhome/library/wiki/overview.md    # the current big picture and thesis
```

Then search by topic. Domains are fixed: `hardware/`, `models/`, `software/`,
`hermes/`, `concepts/`, `reading/`.

```fish
rg --ignore-case "laguna|prompt eval|llama-server" ~/devhome/library/wiki
```

`raw/` holds source documents exactly as captured and is never edited. `reading/` has
one summary page per source. `log.md` is the append-only history of what was ingested
and when — useful for judging how stale a claim is.

**Claims are labelled `measured`, `claimed` or `inferred`.** Weigh them accordingly: a
`measured` number came from running the thing on Greg's own hardware; a `claimed` one
came from a vendor or a blog post. Never cite an `inferred` claim as fact, and say
which label you are relying on when it matters.

## What to do when the library does not have it

File the gap; do not quietly absorb it.

- **The library is Greg's, and agents work in it as clerks only when he asks.** Its own
  `AGENTS.md` says so. So the default is: leave a task for the librarian rather than
  writing the page yourself.
- Use the board so the task survives the session:

```fish
hermes kanban create "Document <the thing>" --assignee librarian --body "<what to find out, and why it came up>"
```

- If Greg does ask you to write it, follow `~/devhome/library/AGENTS.md` exactly:
  `raw/` unchanged, a `wiki/` page per topic, `index.md` and `log.md` updated,
  `python3 bin/lint.py` run and clean, then commit. No approval needed there — git is
  the safety net — but tell him what changed and the commit hash.

## What never goes in the library

It is not AI memory and not an agent workspace. Keep out:

- agent memories, character, or journals — those live in
  `/devhome/ai/state/agent-homes/<company>/<instance>/`
- project state and decisions — those live with the project, or in `state/shared/`
- anything that is true only for one agent

The library holds what is true **regardless of who holds it**. That is the same split
as `context/` versus `state/agent-homes/<company>/<instance>/` in the memory design,
and it is why the library can be read by Claude, Codex, Hermes and Greg without any of
them stepping on each other.

## Failure mode to avoid

Consulting the library and then doing it your own way anyway. If a page says how to do
something and you deviate, say why in one line — either the page is stale, in which
case file a task to fix it, or you are about to repeat the mistake it recorded.
