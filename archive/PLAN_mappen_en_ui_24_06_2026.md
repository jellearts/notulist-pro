# PLAN — Mappen/projecten en UI-verbeteringen Notulist-Pro

**Project**: /config/workspace/DefApps/Notulist-Pro  
**Versie-doel**: v1.1  
**Datum**: 24-06-2026  
**Auteur**: Claude Code

---

## 1. Context / Reason

Notulist-Pro v1.0 slaat alle vergaderingen op als één platte lijst. Voor terugkerende vergaderingen (zoals wekelijks werkoverleg) wil de gebruiker verslagen kunnen groeperen in mappen/projecten. Ook moet het transcript-invoerveld niet meer mee-uitrekken met de inhoud, en moet een vergadering vooraf én achteraf aan een map gekoppeld kunnen worden.

Deze wijziging raakt het datamodel, de sidebar, de vergaderweergave en de data-migratie. Daarom wordt het uitgevoerd als een geplande minor release.

---

## 2. Objective

Implementeren van:

1. Mappen/projecten waarin vergaderverslagen kunnen worden gegroepeerd.
2. Mogelijkheid om in de sidebar nieuwe mappen aan te maken, te hernoemen en te verwijderen.
3. Een fixed transcript-tekstveld met scrollbalk, dat niet uitrekt bij invoer.
4. Een optionele map-koppeling die zowel bij aanmaken van een nieuwe vergadering als achteraf kan worden ingesteld.
5. Backward-compatible datamigratie: bestaande vergaderingen blijven werken zonder map.
6. Federated-context-onboarding van het project, omdat Notulist-Pro nog geen `CLAUDE.md`, `MEMORY.md`, `SKILLS.md`, `ARCHITECTURE.md`, `CHANGELOG.md`, `AGENTS.md` en `skills/` heeft.

---

## 3. Milestones

| # | Milestone | Definition of Done |
|---|-----------|-------------------|
| 1 | Context op orde | De 5 federated-contextbestanden + `AGENTS.md` + `skills/` bestaan en audit passed. |
| 2 | Datamodel uitgebreid | `folders`-array en `folderId` op meetings toegevoegd; migratie werkt. |
| 3 | Sidebar met mappen | Sidebar toont mappen, kan inklappen, toont meetings per map. |
| 4 | Mapbeheer | Gebruiker kan mappen aanmaken, hernoemen, verwijderen (met bevestiging). |
| 5 | Map-koppeling vergadering | Dropdown in vergaderweergave voor optionele map-keuze + verplaatsen. |
| 6 | Transcript-veld fixed | Textarea heeft vaste hoogte, scrollt, en rekt niet uit. |
| 7 | Test & afronding | App opent in browser zonder JS-fouten; audit passed; CHANGELOG bijgewerkt. |

---

## 4. Design Choices

### 4.1 Datamodel

Kiezen voor een aparte `folders`-array en een `folderId`-veld op elke meeting.

```json
{
  "folders": [
    {"id": "f-123", "naam": "Werkoverleg", "sortIndex": 0, "aangemaakt": "...", "gewijzigd": "..."}
  ],
  "meetings": [
    {"id": "m-456", "naam": "Werkoverleg week 25", "folderId": "f-123", "datum": "2026-06-24", ...}
  ],
  "instellingen": {...}
}
```

**Waarom deze keuze:**
- Eenvoudiger dan geneste meetings in folders.
- Eén meeting hoort altijd bij maximaal één map (voldoet aan de vraag).
- Een meeting zonder `folderId` blijft in een "Geen map"-sectie staan, wat backward compatible is.
- Mappen kunnen later uitgebreid worden met metadata zonder meetings te hoeven aanpassen.

### 4.2 UI-structuur sidebar

Sidebar toont bovenaan een knop "＋ Nieuwe map". Daarna een lijst met mappen (uitklapbaar). Onderaan blijft "Nieuwe vergadering". Vergaderingen zonder map komen onder een vaste groep "Overige vergaderingen".

### 4.3 Mapacties

Per map in de sidebar:
- Inklappen/uitklappen (chevron).
- Hernoemen via inline input of klein potlood-icoon.
- Verwijderen met bevestiging. Bij verwijderen worden de gekoppelde meetings teruggezet naar "Geen map" (`folderId` wordt `null`).

### 4.4 Map-koppeling in vergaderweergave

In de kop van `MeetingView` komt een dropdown naast de datum:
"Map: [Werkoverleg ▼]". Deze is optioneel en kan op elk moment worden gewijzigd. Bij het aanmaken van een nieuwe vergadering vanuit een geselecteerde map wordt deze map vooringevuld.

### 4.5 Transcript-veld

Het huidige `AutoTextarea`-component rekt automatisch mee. Voor het transcript vervangen we dit door een standaard `<textarea>` met vaste hoogte (bijv. `180px`) en `overflow: auto`. Het tekentaal blijft zichtbaar.

---

## 5. Step-by-Step Task List

- [ ] **5.1** Maak de federated-contextbestanden voor Notulist-Pro aan (seed van template).
- [ ] **5.2** Voeg Notulist-Pro toe aan `/config/workspace/scripts/check_federated_context.sh` en draai de audit.
- [ ] **5.3** Breidt `ensureDataShape` uit met `folders`-array en `folderId` op meetings.
- [ ] **5.4** Pas `getDefaultData`, `maakMeeting` en `migreerMeeting` aan voor het nieuwe model.
- [ ] **5.5** Breidt `useData` uit zodat `folders` en `meetings` samen worden opgeslagen.
- [ ] **5.6** Herbouw `Sidebar`:
  - toon mappen als uitklapbare secties,
  - toon meetings per map gesorteerd op datum,
  - toon "Overige vergaderingen" voor meetings zonder map,
  - voeg "Nieuwe map"-knop toe,
  - voeg hernoemen/verwijderen-acties toe.
- [ ] **5.7** Voeg helperfuncties toe voor mapbeheer: `maakFolder`, `updateFolder`, `deleteFolder`.
- [ ] **5.8** Voeg in `MeetingView` een map-dropdown toe en sla `folderId` op.
- [ ] **5.9** Vooringevulde map bij nieuwe vergadering: als een map is uitgeklapt/geselecteerd, gebruik die `folderId`.
- [ ] **5.10** Vervang `AutoTextarea` voor het transcript door een fixed-height textarea met scroll.
- [ ] **5.11** Werk `CHANGELOG` bij in de HTML-header en in `CHANGELOG.md`.
- [ ] **5.12** Test de app in een browser (open `notulist-pro.html`) en controleer dat:
  - mappen aan te maken zijn,
  - meetings in mappen te plaatsen zijn,
  - het transcript-veld niet meer uitrekt,
  - het databestand correct wordt weggeschreven,
  - de migratie van een oud databestand zonder mappen foutloos verloopt.
- [ ] **5.13** Archiveer dit plan in `/config/workspace/DefApps/Notulist-Pro/archive/` en vermeld het in `CHANGELOG.md`.

---

## 6. Code Snippets

### 6.1 Nieuwe datamodel-functies

```js
function maakFolder(o){return{id:uid(),naam:"Nieuwe map",sortIndex:0,aangemaakt:new Date().toISOString(),gewijzigd:new Date().toISOString(),...o};}
function migreerFolder(f){return{id:f.id||uid(),naam:f.naam||"Map",sortIndex:f.sortIndex||0,aangemaakt:f.aangemaakt||new Date().toISOString(),gewijzigd:f.gewijzigd||new Date().toISOString()};}
function migreerMeeting(m){return{
  id:m.id||uid(),
  naam:m.naam||"Naamloze vergadering",
  datum:m.datum||todayISO(),
  folderId:m.folderId||null,
  transcript:m.transcript||"",
  verslagMd:m.verslagMd||"",
  verslagHtml:m.verslagHtml||"",
  aangemaakt:m.aangemaakt||new Date().toISOString(),
  gewijzigd:m.gewijzigd||new Date().toISOString(),
};}
const DATA_ROOT_KEYS=["folders","meetings","instellingen"];
function ensureDataShape(raw){
  const defs=getDefaultData();
  const data=raw&&typeof raw==="object"?{...raw}:{};
  DATA_ROOT_KEYS.forEach(k=>{if(data[k]===undefined)data[k]=JSON.parse(JSON.stringify(defs[k]));});
  data.folders=Array.isArray(data.folders)?data.folders.map(migreerFolder):[];
  data.meetings=Array.isArray(data.meetings)?data.meetings.map(migreerMeeting):[];
  data.instellingen={...defs.instellingen,...(data.instellingen||{})};
  if(!data.instellingen.verslagPrompt||!data.instellingen.verslagPrompt.trim())data.instellingen.verslagPrompt=DEFAULT_VERSLAG_PROMPT;
  if(!data.instellingen.systemPrompt||!data.instellingen.systemPrompt.trim())data.instellingen.systemPrompt=DEFAULT_SYSTEM_PROMPT;
  return data;
}
```

### 6.2 Fixed transcript textarea

```jsx
<textarea
  style={{...INP, height: 180, minHeight: 180, overflow: "auto", resize: "none", fontSize: 13, lineHeight: 1.5}}
  value={transcript}
  onChange={e=>setTranscript(e.target.value)}
  onBlur={()=>patch({transcript})}
  placeholder="Plak hier de transcriptie of upload een .txt-bestand."
/>
```

### 6.3 Sidebar map-sectie (schets)

```jsx
function Sidebar({folders,meetings,selectedId,selectedFolderId,onSelect,onNieuw,onNieuwMap,onSettings,onChangelog,bestand,onUpdateFolder,onDeleteFolder}){
  const [open,setOpen]=useState(()=>{
    const init={"__overige__":true};
    folders.forEach(f=>init[f.id]=true);
    return init;
  });
  // ...
}
```

---

## 7. Risico's en aandachtspunten

- **Datamigratie**: bestaande gebruikers hebben geen `folders`-array. De migratie moet die toevoegen zonder meetings te verliezen.
- **File System Access**: wijzigingen worden pas definitief opgeslagen bij `writeData`. Elke wijziging aan mappen moet via `updMeetings`/`updFolders` lopen.
- **AGENTS.md mirror**: elke wijziging aan `CLAUDE.md` moet meteen worden doorgeschreven naar `AGENTS.md`.
- **Geen build step**: de app is één HTML-bestand. Alle wijzigingen gebeuren inline in `notulist-pro.html`.

---

## 8. Goedkeuring

Plan goedkeuren betekent akkoord met:
- Één map per vergadering (niet meerdere labels/tags).
- Mappen in de sidebar uitklapbaar weergeven.
- Transcript-veld fixed height met scroll.
- Federated-context-onboarding wordt meegenomen in dezelfde sessie.
