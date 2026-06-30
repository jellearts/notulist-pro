---
# SKILLS.md - Notulist-Pro
**Context**: Part of the federated server structure located at `/config/workspace/DefApps/Notulist-Pro/`. Generated from `/config/workspace/templates/project-context/SKILLS.md`.
**Relationship**: Guided by root `/config/workspace/SKILLS.md` and `/config/workspace/DefApps/SKILLS.md`. Interacts closely with the other 4 local context files and the `skills/` directory.
**Purpose**: Copy-pasteable CLI commands, custom scripts, and runtime recipes for Notulist-Pro. Reusable, topic-specific recipes live as individual files under `skills/` and are indexed here.
---

# Notulist-Pro — SKILLS.md

## Core Recipes

```bash
# Open de app in de standaard browser (Edge/Chrome aanbevolen)
cd "/config/workspace/DefApps/Notulist-Pro" && xdg-open notulist-pro.html

# Federated context audit draaien
cd /config/workspace && bash scripts/check_federated_context.sh
```

## Skill Index

Search the `skills/` directory before applying a recurring pattern. Each skill is a focused, reusable recipe.

| Skill | Description | Tags |
|-------|-------------|------|
| [skill-example](skills/skill-example.md) | Placeholder skill; replace when project-specific recipes are added. | example |
| [skill-tarbal-distributie](skills/skill-tarbal-distributie.md) | Tarbal aanmaken na een nieuwe build op workspace-root. | distributie, release |

## Adding a New Skill

1. Create `skills/skill-<kebab-name>.md` with the standard frontmatter:
   ```markdown
   ---
   name: <short-kebab-name>
   description: <one-line summary>
   tags: [tag1, tag2]
   ---
   ```
2. Add the skill to the index table above.
3. Commit and push with the code change that depends on it.
