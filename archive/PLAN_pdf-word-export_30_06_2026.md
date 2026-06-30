# PLAN — PDF & Word export Notulist-Pro

**Datum**: 30-06-2026
**Versie**: v1.5
**Aanleiding**: Gebruiker wil het verslag niet alleen als markdown/standalone HTML, maar ook als net gestileerde **PDF** en **Word-document** (koppen, alinea's, tabellen, echte opmaak).

## Context / Reden
Notulist-Pro draait op de Mulan-werkplek: Windows, browser, **geen internet**. Externe libraries of CDN's zijn niet toegestaan (air-gapped). De bestaande `exportVerslagHTML` produceert al een gestileerde standalone HTML met Calibri-stijl, koppen en tabellen. Die stijl moet herbruikbaar worden voor PDF en Word.

## Keuzes (bevestigd met gebruiker)
- **PDF**: native *print-naar-PDF* via een verborgen iframe + `window.print()`. Vector-tekst, echte koppen, selecteerbaar, 0 afhankelijkheden. Geen jsPDF/html2canvas (raster, zwaar, extra vendoring).
- **Word**: MS Word-compatible HTML met `.doc`-extensie (`application/msword`). Opent netjes in Word met echte `<h1>/<h2>/<p>/<table>`-elementen, `mso-outline-level` voor navigatie, `@page`-marges. Volledig bewerkbaar. Geen `docx`-library nodig.

## Milestones
1. Refactor: gedeelde `VERSLAG_CSS` + `verslagBody(meeting)` helper, hergebruikt door HTML/PDF/Word.
2. `exportVerslagPDF(meeting)` via iframe-print.
3. `exportVerslagWord(meeting)` via `.doc` HTML-blob.
4. UI: knoppen "⬇ PDF" en "⬇ Word" naast bestaande "⬇ HTML".
5. Versie-bump v1.4 → v1.5, CHANGELOG, AGENTS.md mirror, tarball, README, commit+push.

## Design
- Bestaande `exportVerslagHTML` blijft werken (zelfde stijl).
- Print-CSS: `@page { margin: 2cm }` en tabelranden behouden bij afdruk.
- Word-CSS: pt-eenheden (niet px), `mso-outline-level` per kop, `border-collapse` voor tabellen.
- Bestandsnamen: `verslag-<naam>-<datum>.pdf` / `.doc` (datum = ISO). Let: PDF-naam wordt door de browser-printdialoog bepaald; titel van het iframe-document wordt bestandsnaam-suggestie.

## Step-by-step Task List
- [x] PLAN-bestand aangemaakt.
- [ ] Refactor + `exportVerslagPDF` + `exportVerslagWord` in `notulist-pro.html`.
- [ ] Knoppen toevoegen in verslag-header.
- [ ] Versie-header, CHANGELOG, AGENTS.md, tarball, README bijwerken.
- [ ] Lokaal visueel controleren (knoppen aanwezig, geen JS-syntaxfout).
- [ ] Plan archiveren naar `archive/` na afronding.

## Code-snippet (kern)
```js
function exportVerslagPDF(meeting){
  const html=verslagDocumentHtml(meeting,"print");
  const iframe=document.createElement("iframe");
  iframe.style.cssText="position:fixed;right:0;bottom:0;width:0;height:0;border:0;";
  document.body.appendChild(iframe);
  const doc=iframe.contentWindow.document;
  doc.open();doc.write(html);doc.close();
  iframe.contentWindow.focus();
  // geef de browser even tijd om te renderen
  setTimeout(()=>{iframe.contentWindow.print();setTimeout(()=>iframe.remove(),2000);},300);
}
function exportVerslagWord(meeting){
  const html=verslagDocumentHtml(meeting,"word");
  const blob=new Blob(["﻿",html],{type:"application/msword"});
  const url=URL.createObjectURL(blob);
  const a=document.createElement("a");a.href=url;
  a.download=verslagBestandsnaam(meeting,".doc");a.click();URL.revokeObjectURL(url);
}
```