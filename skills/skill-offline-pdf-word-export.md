---
name: offline-pdf-word-export
description: Offline PDF- en Word-export uit een standalone HTML-app zonder externe libraries (Mulan air-gapped).
tags: [export, pdf, word, offline, standalone-html]
---

# Offline PDF/Word-export uit standalone HTML-app

**Probleem**: Mulan-werkplek is air-gapped (geen internet, geen CDN). Voor een
gestileerd PDF- of Word-export uit een browser-app kan je niet leunen op jsPDF,
html2canvas of de `docx`-library via npm. Toch wil je vector-PDF en bewerkbaar
Word met echte koppen/alinea's/tabellen.

**Oplossing**: beide formaten via de browser zelf, 0 afhankelijkheden.

## PDF — native print-naar-PDF via verborgen iframe

Bouw het verslag als volledige HTML-pagina met een `@media print`-stijl
(`@page{margin:2cm}`, `break-inside:avoid` op tabellen), schrijf het in een
verborgen iframe en roep `print()` aan. De browser-renderer levert vector-PDF.

```js
function exportPDF(html){
  const iframe=document.createElement("iframe");
  iframe.style.cssText="position:fixed;right:0;bottom:0;width:0;height:0;border:0;opacity:0;";
  document.body.appendChild(iframe);
  const doc=iframe.contentWindow.document;
  doc.open();doc.write(html);doc.close();
  const w=iframe.contentWindow; w.focus();
  setTimeout(()=>{try{w.print();}catch(e){}setTimeout(()=>iframe.remove(),3000);},350);
}
```

- Getriggerd door een klik (user activation) → geen popup-blokkade.
- Bestandsnaam-suggestie in de printdialoog = `<title>` van het iframe-document.

## Word — `.doc` via MS Word-compatible HTML

Blob met `type:"application/msword"` en `.doc`-extensie. HTML met Word-XML-
namespaces en `mso-outline-level` per kop, pt-eenheden, `@page A4`,
`border-collapse`-tabellen. Opent netjes in Word, volledig bewerkbaar.

```js
function exportWord(html){
  const blob=new Blob(["﻿"+html],{type:"application/msword;charset=utf-8"}); // BOM voor UTF-8
  const url=URL.createObjectURL(blob);
  const a=document.createElement("a");a.href=url;a.download="verslag.doc";a.click();
  URL.revokeObjectURL(url);
}
```

Word-template kern (head):
```html
<!DOCTYPE html>
<html xmlns:o="urn:schemas-microsoft-com:office:office" xmlns:w="urn:schemas-microsoft-com:office:word" xmlns="http://www.w3.org/TR/REC-html40">
<head><meta charset="UTF-8">
<!--[if gte mso 9]><xml><w:WordDocument><w:View>Print</w:View><w:Zoom>100</w:Zoom></w:WordDocument></xml><![endif]-->
<style>
@page WordSection1{size:A4;margin:2cm;}
div.WordSection1{page:WordSection1;}
h1{font-size:20pt;mso-outline-level:1;}
h2{font-size:14pt;mso-outline-level:2;}
table{border-collapse:collapse;}
</style></head>
<body><div class="WordSection1">…</div></body></html>
```

## Valkuilen / lessons

- **Dubbele H1**: als de export-wrapper een `<h1>` toevoegt én het verslag zelf
  met `#` begint, krijg je twee titels. Laat de wrapper de H1 leveren en laat het
  verslag `##`-secties gebruiken (pattern uit MeetingHero).
- **Spurious `<br/>`**: een simpele `\n`→`<br/>` markdown-renderer zet ook
  newlines tussen blokelementen om in `<br/>` → ongewenste extra witruimte.
  Ruim ze op: `</(h[1-6]|ul|table|p)>` gevolgd door `<br/>` → verwijder de `<br/>`.
- **BOM**: zet een UTF-8 BOM (`﻿`) voor de Word-blob, anders kunnen accenten
  verkeerd tonen.

## Hergebruik

Patroon toegepast in Notulist-Pro v1.5 (`verslagDocumentHtml` met mode
`html`/`print`/`word`). Direct herbruikbaar voor MeetingHero, PortfolioManager
en andere standalone HTML Def Apps.