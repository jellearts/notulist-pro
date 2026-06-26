# Skill: Tarbal distributie — Notulist-Pro

Maak een distributable tarbal aan op de workspace-root na elke nieuwe build.

## Commando

```bash
tar -czf /config/workspace/notulist-pro.tar.gz \
  -C "/config/workspace/Def Apps/Notulist-Pro" \
  "notulist-pro.html" \
  "react.min.js" \
  "react-dom.min.js" \
  "babel.min.js" \
  "notulist-pro-data.json"
```

Als `/config/workspace/notulist-pro.tar.gz` al bestaat, wordt het zonder waarschuwing overschreven — dit is het gewenste gedrag.

## Verificatie

```bash
tar -tzf /config/workspace/notulist-pro.tar.gz
```

## Wanneer uitvoeren

Altijd na een build of versie-update, als onderdeel van stap 12 van de SESSION CLOSE CHECKLIST.
Notulist-Pro heeft geen versie-submappen; het commando is stabiel.
