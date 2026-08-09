# Agent Skills

Engineering skill pack for AI coding agents. Each skill is a `SKILL.md` with
actionable standards, concrete thresholds, and explicit anti-patterns —
followed consistently across every phase of development.

## Skills

| Skill | Covers |
| --- | --- |
| `engineering` | Product discovery, requirements, project management, docs |
| `security` | Auth, authorization, vulnerabilities, AI safety, secrets |
| `architecture` | System design, layered architecture, repository structure |
| `backend` | API design, external APIs, caching, payments |
| `frontend` | UI states, components, accessibility, responsive design, EyuTheme design system |
| `database` | Schema design, integrity, SQL performance, backups |
| `testing` | Test strategy, error handling, code review |
| `devops` | CI/CD, Docker, deployment, observability, Git |
| `handoff` | Agent/session handoffs, context transfer, continuation |
| `skill-creator` | Authoring new skills: structure, frontmatter, verification |

## Install

Clone into a project and reference the pack:

```bash
git clone https://github.com/Joelorbit/Agent-skills.git .agents
```

Each skill auto-activates from its `description` when the agent works on the
matching concern. `AGENTS.md` at the pack root is the engineering
constitution: development phases, coding principles, definition of done.

## Design System

All frontend work must use the [EyuTheme](https://github.com/Joelorbit/Mytheme)
design system — token contract, component reuse, dark-by-default. See
`frontend` skill section 6.

## Layout

```
.agents/
  AGENTS.md               # engineering constitution
  skills/
    <skill>/SKILL.md      # one concern, one skill
```

## License

MIT — see [LICENSE](LICENSE).