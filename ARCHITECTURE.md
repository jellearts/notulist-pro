---
# ARCHITECTURE.md - Notulist-Pro
**Context**: Part of the federated server structure located at `/config/workspace/Def Apps/Notulist-Pro/`. Generated from `/config/workspace/templates/project-context/ARCHITECTURE.md`.
**Relationship**: Guided by root `/config/workspace/ARCHITECTURE.md` and `/config/workspace/Def Apps/ARCHITECTURE.md`. Interacts closely with the other 4 local context files.
**Purpose**: Dataflow, network topology (Docker networks), and API contracts for Notulist-Pro.
---

# Notulist-Pro — ARCHITECTURE.md

## Overview

Notulist-Pro is een zelfstandige browser-applicatie gebouwd als één HTML-bestand met React (via CDN) en Babel in de browser. Er is geen backend, geen Docker en geen build pipeline. De app wordt geopend als `file://` of via een eenvoudige webserver.

## Dataflow

1. Gebruiker opent `notulist-pro.html`.
2. Bij eerste start kiest de gebruiker een `.json`-bestand via de File System Access API.
3. De app leest het bestand, normaliseert het via `ensureDataShape`, en houdt de data in React state.
4. Wijzigingen aan vergaderingen, mappen of instellingen worden via `writeData` teruggeschreven naar hetzelfde `.json`-bestand.
5. Een backup kan worden gedownload als los `.json`-bestand.

## Network Topology

- Geen Docker, geen Caddy, geen server-side proxy.
- De app kan `file://` lokaal draaien of worden geserveerd vanuit een eenvoudige static host.
- DefGPT wordt rechtstreeks vanuit de browser aangeroepen. Bij CORS-problemen valt de app terug op een kopieer/plak-fallback.

## API Contracts

### DefGPT chat completions

- **Endpoint**: instelbaar, standaard `https://pro.defgpt.data.mindef.nl/api/chat/completions`
- **Methode**: `POST`
- **Headers**: `Content-Type: application/json`, `Authorization: Bearer <key>`
- **Body**:
  ```json
  {
    "model": "DefGPT-Chat-gpt-oss-120b",
    "messages": [
      {"role": "system", "content": "..."},
      {"role": "user", "content": "..."}
    ],
    "temperature": 0.3,
    "max_tokens": 4000
  }
  ```

### Lokale data file

- **Formaat**: JSON
- **Root keys**: `folders`, `meetings`, `instellingen`
- **Folder**: `{id, naam, sortIndex, aangemaakt, gewijzigd}`
- **Meeting**: `{id, naam, datum, folderId, transcript, verslagMd, verslagHtml, aangemaakt, gewijzigd}`
- **Instellingen**: `{defgptEndpoint, defgptKey, defgptModel, systemPrompt, verslagPrompt, maxTokens, transcriptMaxChars}`
