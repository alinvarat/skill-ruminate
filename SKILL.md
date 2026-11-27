---
name: ruminate
description: Mine past Claude Code and Codex conversations for repeated user corrections, preferences, recurring friction and wished-for skills that are not yet recorded. Use on "ruminate", "mine my history", "ดูประวัติแชตหาสิ่งที่ซ้ำ". Reports first; saves only what the user approves.
---

# Ruminate
Complements `reflect` (this chat) and `meditate` (record audit). Read-only until the user approves.

1. Read `<memory-root>/index.md` and `preferences/user.md` once to know what is already recorded.
2. Locate history: Claude `<claude-projects-dir>/*.jsonl` and Codex `<home>/.codex/sessions/`. Ask for a date range if the user gave none; default to the last 7 days.
3. Extract to the scratch directory:
   `python3 -I "<skills-root>/ruminate/references/extract-conversations.py" "<scratch>/ruminate" --claude-dir "<claude-projects-dir>" --codex-dir "<home>/.codex/sessions" --from <YYYY-MM-DD> --batches <N>`
   Use about one batch per 20 conversations, 2 to 6 batches. If the script fails, report the error and stop; do not hand-parse the history.
4. Spawn one Sonnet subagent per batch in parallel (Haiku if the batch is mostly short chats). Give it the batch manifest path, the already-recorded topics from step 1, and ask for: user corrections (with the user's words and source file), repeated preferences, verified technical gotchas, workflow patterns, frustrations, skills the user wished for. Write findings to `<scratch>/ruminate/findings_N.md`. Never include secrets, tokens or credentials from the chats.
5. Merge. Keep a finding only if it recurred in several conversations, corrects a wrong existing record, or caused repeated wasted work. Discard one-offs.
6. Present a table: finding, evidence (count and sources), proposed destination. Apply approved items through `memory` or the owning skill; skill edits follow `writing-for-agents`, then the owner's standing rule: sync sanitized public copies (no private paths, IDs or personal data) to `agent-skills` and `skill-<name>`.
7. Delete `<scratch>/ruminate` when done. Do not read or alter other people's session files.
