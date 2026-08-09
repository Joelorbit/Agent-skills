---
name: skill-creator
description: >-
  Standard for authoring new agent skills (SKILL.md). Covers structure, frontmatter,
  writing rules, and verification. Activate when creating a new skill for
  this pack or a project.
---

# Skill Creation Standard

## 1. Structure

```
skill-name/
├── SKILL.md              # Required — instructions + YAML frontmatter
├── scripts/              # Optional — helper scripts the agent can execute
├── references/           # Optional — deep docs, checklists, examples
└── assets/               # Optional — templates, images, data
```

## 2. Frontmatter Contract

```yaml
---
name: skill-name              # kebab-case, matches folder name
description: >-
  What the skill does and WHEN to activate it. Write concrete trigger
  conditions ("Activate when building server-side features").
---
```

- `name` and `description` are mandatory. A vague description guarantees the
  agent never activates the skill.
- Frontmatter must be valid YAML. Use `>-` folded style for multi-line text.

## 3. Writing Rules

- **Actionable, not philosophical:** every line tells the agent what to do.
  "Never trust the client" without follow-up is useless; add "fulfill orders
  only from signed webhooks."
- **Concrete thresholds:** give numbers — 44x44px touch targets, 5s/10s
  timeouts, WCAG 4.5:1 contrast. Vague guidance gets ignored.
- **Name the anti-pattern:** state what NOT to do explicitly (e.g., "No
  hardcoded breakpoints", "Avoid: god classes").
- **Tables and lists over prose:** agents scan; structured content survives.
- **One concern per skill:** a skill on "everything" is a skill on nothing.
- **Reference, don't duplicate:** shared checklists live in the skill or
  `references/`; cross-reference them.
- **Keep it tight:** 40-80 lines is a good target. Trimming is part of writing.

## 4. Verification Checklist

Before committing a skill:

- [ ] `name` matches the folder name, kebab-case
- [ ] `description` states both capability AND activation trigger
- [ ] Frontmatter parses as YAML
- [ ] Rules are actionable, with concrete thresholds where applicable
- [ ] Anti-patterns explicitly named
- [ ] Registered in the pack's `AGENTS.md` skill table
- [ ] No secrets, credentials, or private URLs

## 5. Testing a Skill

- Run the skill's own instructions against a real task in a scratch repo.
- If the skill references scripts, execute them from a clone (not the author's
  working tree).
- Iterate until an agent can follow it start-to-finish with no clarification.