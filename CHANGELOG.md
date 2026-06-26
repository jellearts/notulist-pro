---
# CHANGELOG.md - Notulist-Pro
**Context**: Part of the federated server structure located at `/config/workspace/Def Apps/Notulist-Pro/`. Generated from `/config/workspace/templates/project-context/CHANGELOG.md`.
**Relationship**: Guided by root `/config/workspace/CHANGELOG.md` and `/config/workspace/Def Apps/CHANGELOG.md`. Interacts closely with the other 4 local context files.
**Purpose**: Append-only chronological history of changes, releases and operational events for Notulist-Pro.
---

# Notulist-Pro — CHANGELOG.md

## 2026-06-26 — v1.2 released — model-selector dropdown
- Model-keuze in instellingen vervangen door een `<select>`-dropdown, gelijk aan MeetingHero.
- Vrije tekstinvoer (input + datalist) verwijderd; gebruiker kan alleen nog kiezen uit de beschikbare modellen.
- `APP_VERSION` bijgewerkt naar `1.2`; title en loading screen bijgewerkt.
- Backward compatible: `defgptModel` in data.json ongewijzigd.

## 2026-06-26 — Versie-mismatch fix in loading screen
- Loading screen toonde "Notulist-Pro v1.0"; gecorrigeerd naar "Notulist-Pro v1.1" (overeenkomend met APP_VERSION en `<title>`).

## 2026-06-24 — Federated context onboarding
- Project context files toegevoegd vanuit `/config/workspace/templates/project-context/`.
- Notulist-Pro geregistreerd in `/config/workspace/scripts/check_federated_context.sh`.

## 2026-06-24 — v1.1 plan aangemaakt
- Plan `PLAN_mappen_en_ui_24_06_2026.md` aangemaakt voor mappen/projecten, fixed transcriptveld, en optionele map-koppeling.

## 2026-06-24 — v1.1 released
- Vergaderingen kunnen worden gegroepeerd in mappen/projecten in de sidebar.
- Nieuwe mappen aanmaken, hernoemen en verwijderen via het zijmenu.
- Optionele map-koppeling per vergadering, zowel vooraf als achteraf instelbaar.
- Transcript-invoerveld heeft vaste hoogte en scrollt in plaats van mee uit te rekken.
- Datamodel uitgebreid met `folders` en `folderId`; backward-compatible migratie via `ensureDataShape`.
- Federated context files toegevoegd en Notulist-Pro geregistreerd in de workspace audit.
- Plan `PLAN_mappen_en_ui_24_06_2026.md` gearchiveerd.
