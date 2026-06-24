---
# CLAUDE.md - Notulist-Pro
**Context**: Part of the federated server structure located at `/config/workspace/Def Apps/Notulist-Pro/`. Generated from `/config/workspace/templates/project-context/CLAUDE.md`.
**Relationship**: Guided by root `/config/workspace/CLAUDE.md` and `/config/workspace/Def Apps/CLAUDE.md`. Interacts closely with the other 4 local context files and the `skills/` directory.
**Purpose**: Local commands, build steps, environment constraints, and the end-of-session checklist for Notulist-Pro.
---

# Notulist-Pro — Local CLAUDE.md

Standalone HTML/React-app voor Mulan. Transcript erin, professioneel Nederlands vergaderverslag eruit via DefGPT. Alles zit in één HTML-bestand; geen build step of server nodig.

## Quick Reference
- GitHub repo: `https://github.com/jellearts/notulist-pro`
- Project path: `/config/workspace/Def Apps/Notulist-Pro`
- Entrypoint: `notulist-pro.html` (dubbelklik in Edge/Chrome)
- Data file: door gebruiker gekozen `.json` via File System Access API

## Skills
Project-specific and reusable recipes live in `SKILLS.md` and the `skills/` directory. Before applying a recurring pattern (script, command sequence, copywriting style, deployment step, etc.), search the local `skills/` directory and the workspace root `/config/workspace/skills/` directory. If a matching skill exists, follow it; if not, consider creating one when the pattern is likely to be reused.

## Before Every Commit

- [ ] Does this change affect Caddy, global paths, or server-wide architecture? If yes, also update `/config/workspace/CLAUDE.md` or `/config/workspace/ARCHITECTURE.md`.
- [ ] Append this change to the project's `CHANGELOG.md`.
- [ ] Commit with a descriptive message and push to GitHub.

## SESSION CLOSE CHECKLIST (end of working session — execute before answering)

Before declaring the session done, run through every item below and confirm:

1. **Analyze Session**: identify new pitfalls, hidden requirements, bugs, and configuration shifts.
2. **No Memory Loss**: if something failed before it worked, document why in `MEMORY.md` and workspace auto-memory.
3. **`CHANGELOG.md`**: confirm the final state is captured.
4. **`MEMORY.md`**: update active state, tickets, and hard lessons.
5. **`SKILLS.md` / `skills/`**: save any script (>5 lines), non-trivial `docker exec` pattern, or constructed command sequence as a skill file under `skills/` and list it in `SKILLS.md`.
6. **`CLAUDE.md`**: update if any hard requirement or build/deploy step changed.
7. **`AGENTS.md` synchronization**: confirm every project `AGENTS.md` is a current mirror of its `CLAUDE.md`. If any `CLAUDE.md` was changed this session, the matching `AGENTS.md` must be updated in the same commit.
8. **Plan lifecycle**: completed `PLAN_*.md`? Archive it to `archive/` and note it in `CHANGELOG.md`.
9. **Project-specific checklist**: execute any items from this project's own checklist.
10. **Federated context audit**: run `/config/workspace/scripts/check_federated_context.sh` if the session added, removed, or restructured a project.
11. **Workspace auto-memory**: write findings to `/config/.claude/projects/…/memory/`.

Only answer the user after these steps are actually done.
