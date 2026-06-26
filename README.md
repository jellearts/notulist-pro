# Notulist-Pro

Transcript erin, professioneel Nederlands vergaderverslag eruit — aangedreven door DefGPT.

Notulist-Pro is een standalone HTML-app die je in Edge of Chrome opent zonder installatie of server. Plak of upload een transcript van je vergadering, klik op "Genereer verslag", en ontvang een gestructureerd, bewerkbaar vergaderverslag. Vergaderingen worden per map georganiseerd en opgeslagen in een JSON-bestand op een locatie naar keuze.

Ontwikkeld bij de sectie Business, Information & Analytics (BIA), Staf Air Support Command.

---

## Inhoud

1. Wat Notulist-Pro doet
2. Vereisten
3. Starten
4. Eerste gebruik
5. Werkwijze
6. Verslag bewerken en exporteren
7. Mappenstructuur
8. DefGPT-integratie en CORS
9. Opslag en backup
10. Instellingen
11. Ontwerpkeuzes
12. Bekende beperkingen
13. Troubleshooting

---

## 1. Wat Notulist-Pro doet

Notulist-Pro zet een ruwe vergaderregistratie (transcript, aantekeningen, automatische ondertiteling) om naar een professioneel Nederlands vergaderverslag. De app stuurt het transcript samen met vergadermetadata naar DefGPT, dat een gestructureerd verslag teruggeeft. Dit verslag is direct bewerkbaar in de app en exporteerbaar als HTML-document.

Vergaderingen worden geordend in mappen (teams, projecten, overlegvormen) en bewaard in één JSON-bestand dat je zelf kiest en beheert.

---

## 2. Vereisten

- Microsoft Edge of Google Chrome (File System Access API vereist)
- Mulan-werkplek of elk ander Windows-systeem
- Een DefGPT API-sleutel (Bearer-token)
- Geen installatie, geen server, geen internetverbinding

Alle vier bestanden moeten in **dezelfde map** staan:

```
notulist-pro.html
react.min.js
react-dom.min.js
babel.min.js
```

Het databestand (`notulist-pro-data.json` of een zelfgekozen naam) kan ergens anders staan, ook op een netwerkschijf.

---

## 3. Starten

Dubbelklik `notulist-pro.html` of open het via Edge/Chrome (`Bestand → Openen`). De app vraagt direct om een databestand te kiezen of aan te maken.

> De app onthoudt het gekozen bestand via de browser. Bij de volgende opening volstaat één klik om de toegang te herstellen.

---

## 4. Eerste gebruik

1. Klik **"Nieuw databestand aanmaken"** en kies een locatie (bijv. Documenten of een netwerkschijf). De app maakt een leeg `notulist-pro-data.json` aan.
2. Ga naar **Instellingen** en vul de **DefGPT API-sleutel** in (Bearer-token uit OpenWebUI).
3. Controleer het endpoint (`https://pro.defgpt.data.mindef.nl/api/chat/completions`) en het model.
4. Klik **"Test verbinding"** om te bevestigen dat DefGPT bereikbaar is.
5. Maak desgewenst een map aan (bijv. "Staf ASC") en begin met de eerste vergadering.

---

## 5. Werkwijze

1. **Nieuwe vergadering**: klik "+ Vergadering", geef naam en datum.
2. **Transcript aanleveren**: upload een `.txt`-, `.vtt`-, `.srt`- of `.md`-bestand, of plak de tekst rechtstreeks in het invoerveld.
3. **Verslag genereren**: klik "Genereer verslag". DefGPT verwerkt het transcript en geeft een gestructureerd verslag terug.
4. **Beoordelen en bewerken**: het verslag verschijnt als opgemaakte HTML. Klik "Bewerk" om tekst, besluiten of actiepunten aan te passen.
5. **Opslaan**: klik "Opslaan". Het verslag wordt bewaard in het databestand.
6. **Exporteren**: klik "HTML" om het verslag als los `.html`-document te downloaden, klaar voor verspreiding of archivering.

---

## 6. Verslag bewerken en exporteren

Het gegenereerde verslag is volledig bewerkbaar in de app via de ingebouwde rich-text editor. Je kunt:
- Tekst toevoegen, wijzigen of verwijderen
- Besluiten en actiepunten corrigeren
- Ontbrekende informatie aanvullen

Het bewerkte verslag wordt opgeslagen in het databestand zodra je op "Opslaan" klikt.

**HTML-export** produceert een zelfstandig `.html`-bestand met ingebedde opmaak (Calibri-stijl, Defensie-kleuren), geschikt voor e-mail of archivering.

---

## 7. Mappenstructuur

Vergaderingen kunnen worden geordend in mappen. Gebruik mappen voor teams, projecten of overlegvormen (bijv. "Staf ASC", "BIA-team", "Portfolioboard").

- Maak een map aan via het mappenpaneel links.
- Sleep een vergadering naar een andere map, of wijs de map toe bij aanmaken.
- Mappen zijn puur voor organisatie; ze beïnvloeden de verslaggeneratie niet.

---

## 8. DefGPT-integratie en CORS

Notulist-Pro stuurt per verslag één aanroep naar DefGPT. Wat het model ontvangt:
- De verslagprompt (aanpasbaar in Instellingen)
- Vergadernaam en datum
- Het transcript

Het model ziet nooit andere vergaderingen of eerder gegenereerde verslagen.

### CORS-terugvalmodus
Als de browser de DefGPT-aanroep blokkeert (CORS bij gebruik via `file://`), opent automatisch een dialoogvenster:
1. Kopieer de volledige prompt (knop "Kopieer prompt").
2. Plak in DefGPT Web (`pro.defgpt.data.mindef.nl`).
3. Kopieer het antwoord.
4. Plak terug in het dialoogvenster en klik "Verslag opslaan".

Het verslag wordt dan alsnog opgeslagen, identiek aan een directe API-aanroep.

### Verslagprompt aanpassen
Instellingen → Verslag-prompt. Beschikbare placeholders: `{{TRANSCRIPT}}`, `{{NAAM}}`, `{{DATUM}}`. Staat `{{TRANSCRIPT}}` er niet in, dan wordt het transcript automatisch onderaan de prompt toegevoegd. De knop "Standaard" zet de prompt terug naar de fabrieksinstellingen.

---

## 9. Opslag en backup

Data staat in het gekozen JSON-bestand. Dit bestand kan op een netwerkschijf staan en mag van naam worden gewijzigd zolang je het opnieuw koppelt via Instellingen → Data.

**Backup downloaden**: Instellingen → Data → "Backup downloaden". Produceert een gedateerd JSON-bestand zonder de bestandskoppeling (zodat je het veilig kunt archiveren).

**Bestand wisselen**: Instellingen → Data → "Ander bestand koppelen". Je huidige data blijft intact in het oude bestand; de app opent het nieuwe bestand.

**Backward compatibility**: nieuwere versies van de app openen altijd oudere databestanden. Ontbrekende velden worden aangevuld; niets gaat verloren.

---

## 10. Instellingen

| Instelling | Beschrijving |
|-----------|--------------|
| DefGPT endpoint | URL van de DefGPT API (standaard voorgevuld) |
| DefGPT API-sleutel | Persoonlijke Bearer-token uit OpenWebUI |
| Model | `DefGPT-Chat-gpt-oss-120b` (75K, standaard) of `DefGPT-Chat-gemma-4-31B-it` (256K, grote transcripts) |
| Verslag-prompt | De instructie die met elk transcript meegaat; aanpasbaar per organisatie of stijl |
| Databestand | Koppel een bestand of wissel naar een ander bestand |

---

## 11. Ontwerpkeuzes

- **Eén bestand, geen server.** Mulan heeft geen installatiemogelijkheden. De standalone HTML-architectuur werkt zonder IT-tussenkomst op elke Mulan-werkplek.
- **File System Access API.** Een JSON-bestand op een netwerkschijf is portabel, back-upbaar en deelbaar. LocalStorage zit vast aan de browser en is niet overdraagbaar.
- **Eén DefGPT-aanroep per verslag.** Notulist-Pro kent geen meerstapsproces. Het model krijgt alles in één aanroep en geeft één verslag terug. Dit houdt de flow simpel en de foutdiagnose eenvoudig.
- **Prompt volledig aanpasbaar.** Elke organisatie, stijlgids en vergadersoort heeft andere eisen aan een verslag. De prompt is geen zwarte doos maar een bewust aanpasbaar veld.
- **CORS-terugvalmodus als vangnet.** Browserbeveiliging bij `file://`-gebruik blokkeert soms API-aanroepen. De terugvalmodus garandeert dat de workflow altijd werkt, ook zonder directe API-toegang.

---

## 12. Bekende beperkingen

- Werkt alleen in Edge of Chrome (File System Access API niet beschikbaar in Firefox of Safari).
- Grote transcripts kunnen de tokenlimiet van het oss-model (75K) overschrijden. Gebruik het gemma-model (256K) voor uitgebreide vergaderingen.
- De verslaggeneratie is afhankelijk van de kwaliteit van het transcript. Automatisch gegenereerde ondertiteling (`.vtt`) bevat soms fouten die in het verslag doorklinken.
- DefGPT hallucineert incidenteel (verzint deelnemers, claimt genoemde bijlagen). Beoordeel elk verslag voor verspreiding.
- Geen real-time samenwerking; het databestand is bedoeld voor één gebruiker tegelijk.

---

## 13. Troubleshooting

**De app opent maar toont alleen "Laden..."**
Controleer de browserconsole (F12). Meest voorkomende oorzaak: een van de drie JS-bestanden (`react.min.js`, `react-dom.min.js`, `babel.min.js`) ontbreekt naast `notulist-pro.html`.

**"Geen bestand gekoppeld" na herstarten van de browser**
De app toont het koppelscherm met drie knoppen. Klik **"🔓 Verleen toegang aan eerder gekozen bestand"** (groene knop) om opnieuw toegang te verlenen zonder het bestand opnieuw te selecteren. Lukt dat niet, gebruik dan **"📂 Bestaand bestand koppelen"**.

**Verslag genereren mislukt**
Controleer de API-sleutel in Instellingen → DefGPT. Als de sleutel correct is maar de aanroep toch mislukt, is DefGPT tijdelijk niet beschikbaar. Probeer de CORS-terugvalmodus (verschijnt als de aanroep faalt).

**Verslag is leeg of onlogisch**
Het transcript was waarschijnlijk te kort of bevatte te veel ruis (overlappende sprekers, stiltes). Herschrijf of schoon het transcript op en genereer opnieuw.

**Fout: "AbortError" bij het aanmaken van een databestand**
Je hebt het bestandskiesvenster gesloten zonder een locatie te kiezen. Klik opnieuw op "Nieuw databestand aanmaken" en voltooi de selectie.
