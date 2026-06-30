---
# CHANGELOG.md - Notulist-Pro
**Context**: Part of the federated server structure located at `/config/workspace/DefApps/Notulist-Pro/`. Generated from `/config/workspace/templates/project-context/CHANGELOG.md`.
**Relationship**: Guided by root `/config/workspace/CHANGELOG.md` and `/config/workspace/DefApps/CHANGELOG.md`. Interacts closely with the other 4 local context files.
**Purpose**: Append-only chronological history of changes, releases and operational events for Notulist-Pro.
---

# Notulist-Pro — CHANGELOG.md

## 2026-06-30 — v1.5 — PDF/Word-export + nettere verslagopmaak
- **PDF-export**: nieuw `exportVerslagPDF(meeting)` exporteert het verslag als PDF via native print-naar-PDF (verborgen iframe + `window.print()`). Vector-tekst, echte koppen/tabellen, selecteerbaar, 0 externe libraries — volledig offline (Mulan air-gapped). Print-CSS met `@page`-marges en `break-inside:avoid` voor tabellen.
- **Word-export**: nieuw `exportVerslagWord(meeting)` levert een `.doc` (MS Word-compatible HTML met `xmlns:o/w`, `mso-outline-level` per kop, `@page A4`, pt-eenheden, `border-collapse`-tabellen). Opent netjes in Word, volledig bewerkbaar, echte `<h1>/<h2>/<p>/<table>`-elementen.
- **Gedeelde stijl-helper**: `verslagDocumentHtml(meeting, mode)` (`html`/`print`/`word`) + `VERSLAG_CSS` + `verslagBestandsnaam(meeting, ext)` hergebruikt door HTML-, PDF- en Word-export. Bestaande `exportVerslagHTML` behouden.
- **Nettere verslagopmaak** (reden: weergave oogde rommelig vergeleken met MeetingHero):
  - Standaard verslag-prompt herschreven: gebruikt nu `##` voor secties (was `#` per sectie → overmatige grote H1-koppen met onderstreping). Eén H1-titel wordt door de export-wrapper toegevoegd. `-` bullets, `###` voor deelonderwerpen.
  - `renderMarkdown` ruimt overtollige `<br/>` op die tussen blokelementen ontstonden (`</h2><br/><ul>` → `</h2><ul>`); geen ongewenste extra witruimte meer.
  - `.report-view`-CSS verfijnd (`line-height:1.6`, `:first-child`/`:first-of-type`-marge, `ol`, geneste lijsten, `blockquote`, `strong`).
  - Verslag-weergave in niet-bewerkmodus is nu een witte documentkaart (rand, padding, border-radius) in plaats van kale `padding:0 2px`.
- **UI**: knoppen "⬇ PDF" en "⬇ Word" naast bestaande "⬇ HTML" in de verslag-header.
- Backward compatible: datamodel ongewijzigd; bestaande verslagen en opgeslagen prompts blijven werken. Voor de nieuwe sectiestructuur: reset de verslag-prompt via "↺ Standaard" in instellingen.
- Zie `PLAN_pdf-word-export_30_06_2026.md`.

## 2026-06-30 — v1.4 — Dynamische model-selector (API + fallback)
- Instellingen → DefGPT: de model-selector haalt de beschikbare modellen live op via de DefGPT API (`GET …/api/models`, Bearer auth) en vult de dropdown daarmee.
- Nieuwe knop "🔄 Haal modellen op" in instellingen voor handmatige vernieuwing; auto-ophalen bij openen van instellingen als endpoint+key bekend zijn.
- Fallback: reageert de API niet (CORS/netwerk/auth) of geeft geen modellen, dan wordt de complete hardcoded lijst getoond (5 modellen, uitgebreid van 2 naar 5).
- `fetchDefGptModellen(endpoint,key)` leidt het `/models`-endpoint af van het `chat/completions`-endpoint en normaliseert `{data:[]}` / `[]` / `{models:[]}` en string- zowel als object-items.
- Het gekozen model blijft altijd selecteerbaar, ook als het niet in de opgehaalde lijst staat.
- Nieuwe instelling `defgptBeschikbareModellen` (backward compatible — wordt aangevuld als hij ontbreekt).
- Zie `/config/workspace/DefApps/PLAN_model-selector-dynamisch_30_06_2026.md`.

## 2026-06-26 — v1.3 released — startup-fix File System Access
- `requestPermission()` wordt niet meer automatisch aangeroepen bij het laden van de pagina (veroorzaakte een browser `SecurityError` omdat user activation vereist is).
- `checkPermission()` toegevoegd: controleert alleen via `queryPermission` of toegang al verleend is — veilig voor auto-load.
- `readData()` gebruikt nu `checkPermission` in plaats van `ensurePermission`; bij afwezige permissie wordt stil `null` teruggegeven.
- `KoppelScherm` toont een groene knop "🔓 Verleen toegang aan eerder gekozen bestand" als er een opgeslagen handle in IndexedDB staat — de klik is het vereiste gebruikersgebaar voor `requestPermission`.
- `APP_VERSION` bijgewerkt naar `1.3`.

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
