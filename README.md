# Notulist-Pro v1.0

Standalone HTML-app voor Mulan. Transcript erin, professioneel Nederlands vergaderverslag eruit via DefGPT.

## Inhoud van de map
- `notulist-pro.html` — de applicatie (alles erin)
- `react.min.js`, `react-dom.min.js`, `babel.min.js` — libraries, lokaal, naast de HTML

Alle vier bestanden moeten in dezelfde map staan.

## Starten
Dubbelklik `notulist-pro.html` (Edge of Chrome). Geen installatie, geen server.

## Eerste keer
1. Klik "Nieuw databestand aanmaken" en kies een locatie (mag een netwerkschijf zijn). Dit `.json` bewaart al je vergaderingen en instellingen.
2. Ga naar Instellingen en vul de DefGPT API-sleutel (Bearer token) in. Endpoint en model staan al voorgevuld.
3. Test de verbinding.

## Werkwijze
1. Klik "Nieuwe vergadering", geef naam en datum.
2. Upload of plak het transcript (.txt/.vtt/.srt/.md).
3. Klik "Genereer verslag". Het verslag verschijnt als HTML.
4. Klik "Bewerk" om besluiten, acties of tekst direct in de weergave aan te passen. Opslaan.
5. "HTML" exporteert het verslag als los `.html`-bestand.

## DefGPT en CORS
De app roept DefGPT direct vanuit de browser aan. Blokkeert de browser dat (CORS), dan opent automatisch een venster: kopieer de prompt, plak in DefGPT, plak het antwoord terug. Het verslag wordt dan alsnog opgeslagen.

## Prompt tweaken
Instellingen → Verslag-prompt. Dit is de instructie die met het transcript meegaat.
Placeholders: `{{TRANSCRIPT}}`, `{{NAAM}}`, `{{DATUM}}`. Staat `{{TRANSCRIPT}}` er niet in, dan wordt het transcript onderaan toegevoegd. Knop "Standaard" zet de prompt terug.

## Opslag en backup
Data staat in het gekoppelde `.json`. Instellingen → Data → "Backup downloaden" maakt een losse kopie. Een ander bestand koppelen kan ook daar.

## Model
- `DefGPT-Chat-gpt-oss-120b` — standaard (75K context)
- `DefGPT-Chat-gemma-4-31B-it` — voor grote transcripts (256K context)

Instelbaar in Instellingen → Model.
