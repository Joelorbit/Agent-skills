---
name: handoff
description: >-
  Agent-to-agent (or session-to-session) handoff standard. Produces a complete
  context transfer so the receiving agent continues work without re-discovery.
  Activate when ending a session, switching agents, or delegating mid-task work.
---

# Handoff Standard

## 1. When to Hand Off

Hand off when: a session is ending, context budget is exhausted, another agent
takes over, or a task crosses into a different skill domain. Never end a session
with work half-understood in your head.

## 2. Handoff Document Structure

Every handoff must capture six sections, in order:

| Section | Content |
| :--- | :--- |
| **Goal** | The one-line outcome the work is driving toward |
| **State** | Current truth: what exists, what works, what is broken, with file:line refs |
| **Decisions** | Choices already made and why (including rejected alternatives) |
| **Open Questions** | Unresolved ambiguities the next agent must settle first |
| **Next Steps** | Ordered, concrete actions with acceptance criteria |
| **Risks** | Known traps, half-applied changes, and rollback notes |

## 3. Rules

- **No re-discovery:** the handoff must contain everything needed to continue —
  command names, exact paths, ENV dependencies, test commands.
- **Precise refs:** cite `path:line` for every changed or suspected file. Not "the
  auth code" but `src/services/auth.ts:42`.
- **Failures documented:** never hide a failed experiment. Log what was tried,
  what broke, and why it was abandoned.
- **Verify commands listed:** include the exact commands to run state checks
  (`npm test`, `ruff check`, `docker compose ps`, etc.).
- **Keep it current:** update the handoff as you go. Never write it from memory
  at session end.
- **Length discipline:** as long as necessary, as short as possible. Tables and
  lists over prose.

## 4. Receiving a Handoff

- Read the full document before touching anything.
- Re-run the listed verify commands to confirm the stated state.
- Treat "Open Questions" as blockers: resolve them (or explicitly defer) before
  starting Next Steps.
- If state contradicts the handoff, trust reality and update the document.