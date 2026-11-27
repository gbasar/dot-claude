---
name: io-debug
description: Trace Codex input and output during a session with compact, honest records of what was read, searched, changed, sent, or received. Use when the user wants to audit context loading, memory behavior, file access, connectors, web access, or other information crossing a system boundary.
---

# I/O Debug

Show what information entered Codex's working context and what Codex changed or sent elsewhere. The goal is an auditable record, especially during bootstrap and memory diagnosis, without turning ordinary reasoning into I/O.

This skill reports actions; it does not authorize new reads, writes, network access, installations, or external side effects.

## What counts

Log an operation when information crosses a boundary:

- Input: full or partial file reads, searches across files, web searches and page fetches, downloads, memory reads, connector reads, and artifact inspection.
- Output: file creation or edits, deletion or relocation, uploads, published artifacts, memory writes, connector writes, messages sent externally, and commits.
- Do not count private reasoning or ordinary chat prose as I/O.

For bootstrap and memory work, identify the exact file or source that supplied each loaded instruction or memory. Report required sources that were missing or skipped as well as those loaded.

## Compact event format

Use one line per operation:

    ⬅️ 📖 context/always/greg.md · local filesystem · 100% · sed (whole file)
    ⬅️ 👀 server.log · workspace · lines 1–120 of 940 · sed -n 1,120p
    ⬅️ 🔍 src/ · workspace · 14 matches in 6 files · rg TODO src/
    ➡️ 📝 config.yaml · workspace · +2 −1 · apply_patch
    ➡️ 🔌 Calendar · external app · created 1 event · connector
    ⬅️ 📖 missing.md · local filesystem · 0% · read · ❌ not found

Use these categories:

- ⬅️ 📖 full read
- ⬅️ 👀 partial read
- ⬅️ 🔍 local search or discovery
- ⬅️ 🔎 web search
- ⬅️ 🌐 web page fetch
- ⬅️ ⬇️ download or inbound transfer
- ⬅️ 🧠 memory read
- ⬅️ 🔌 connector or app read
- ➡️ ✨ new file
- ➡️ 📝 edit
- ➡️ ➕ append
- ➡️ 🗑️ delete or permanent removal
- ➡️ 📦 move, archive, or other recoverable relocation
- ➡️ ⬆️ upload, publish, commit, or outbound transfer
- ➡️ 💾 memory write
- ➡️ 🔌 connector or app write

Name the actual target system: workspace, local filesystem, memory, chat, artifact, web domain, or connector. Show the essential command or tool, shortened only enough to stay readable.

## Report the amount honestly

- A verified whole-file or whole-document read is 100%.
- For a partial read, give the exact line, page, or byte range. Add a percentage only when the total is known cheaply.
- For a search, report matches and files searched or matched. Do not manufacture a percentage from unrelated line counts.
- For an edit, report added and removed lines when available.
- For a newly created file, 100% means the complete new file was written; it does not imply the whole containing directory was examined.
- Log failed operations with 0% and the failure reason.
- If a tool truncated its output, record the operation as partial even when the underlying command scanned the full target.

Do not perform extra reads merely to calculate a percentage. Accuracy matters more than a decorative metric.

## Where to show it

For a short task, put an I/O log near the end of the final answer. For longer work, batch important events at meaningful commentary milestones and include the complete turn log in the final answer, because transient progress updates may be collapsed.

End a reply containing I/O with a running tally:

    📊 I/O tally · in: 4 (📖2 👀1 🔍1) · out: 2 (✨1 📝1) · this turn: +3 in, +1 out

Carry the previous tally forward while it is available in conversation context. If it is missing after compaction or the skill was just enabled, start a new tally and label it “since io-debug started” rather than guessing.

## Coexist with clear scripting

I/O Debug records boundary crossings and provenance. If clear-scripter is also active, let it explain purpose and command flow; do not duplicate that narration in the I/O log.

Keep each event to one line. Group related events, but never collapse distinct targets so far that the user can no longer tell what entered or left the system.
