---
# MEMORY.md - Notulist-Pro
**Context**: Part of the federated server structure located at `/config/workspace/DefApps/Notulist-Pro/`. Generated from `/config/workspace/templates/project-context/MEMORY.md`.
**Relationship**: Guided by root `/config/workspace/MEMORY.md` and `/config/workspace/DefApps/MEMORY.md`. Interacts closely with the other 4 local context files.
**Purpose**: Active state, architectural pitfalls, and session-drift protection for Notulist-Pro.
---

# Notulist-Pro — MEMORY.md

## Active State

- **Current version**: v1.1 live.
- **Laatste werk**: mappen/projecten toegevoegd aan de sidebar, fixed-height transcriptveld, optionele map-koppeling voor vergaderingen.
- **Git**: repo gepusht naar `https://github.com/jellearts/notulist-pro`.
- **Plan**: `PLAN_mappen_en_ui_24_06_2026.md` is gearchiveerd in `archive/`.

## Architectural Pitfalls

- **Single-file app**: alle logica zit in `notulist-pro.html`. Wijzigingen aan het datamodel moeten in meerdere plaatsen consistent zijn (`getDefaultData`, `ensureDataShape`, `migreerMeeting`, `useData`, UI-componenten).
- **File System Access API**: opslag gebeurt via een door de gebruiker gekozen `.json`-bestand. Elke wijziging moet via `writeData` lopen. De API is alleen beschikbaar in Edge/Chrome met een secure context (file:// wordt meestal geaccepteerd voor lokale files).
- **Datamigratie**: oude databestanden hebben soms geen `folders` array of `folderId` op meetings. `ensureDataShape` moet die altijd toevoegen zonder dataverlies.
- **AGENTS.md mirror**: elke wijziging aan `CLAUDE.md` moet meteen naar `AGENTS.md` worden doorgeschreven; het federatieve audit-script controleert byte-for-byte gelijkheid.

## Session Drift Protection

- Versie 1.0 had geen federated context files; deze zijn in dezelfde sessie als de v1.1 features toegevoegd.
