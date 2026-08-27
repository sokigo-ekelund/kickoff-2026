# Sokigo Designsystem "Fenix" — projektinstruktion

Detta projekt innehåller Sokigos designsystem. När du genererar något i detta
projekt (dokument, rapport, app-skärm, presentation, komponent) **ska** det följa
specen nedan: färg, typografi, logotyper och — viktigast för PDF — utskriftsregler.

> **Syfte & avgränsning.** Detta är en *referens* för att producera Sokigo-märkta
> leveranser, inte en låsning av designsystemet självt. Arbetar du med att
> *vidareutveckla* systemet (nya tokens, nya komponenter, omdesign) är det förstås
> tillåtet att ändra — uppdatera då även denna fil. Specen gäller i första hand när
> du *konsumerar* systemet för att skapa något nytt.

---

## 1. Varumärkesgrund

- **Primärfärg:** Steel Blue `#3EB1C8` (PMS 631 C). Detta är huvudfärgen — använd den
  för accenter, rubriknumrering, knappar, länkar och fokusmarkeringar. Inte som stor
  bakgrundsyta (det blir "90-tal"); håll den som accent mot ljusa/neutrala ytor.
- **Mörk neutral / text:** off-black `#232323`. Brödtext i dokument `#333`.
- **Typsnitt:** **Gotham** (filer i `fonts/`):
  - `Gotham-Book.otf` → `font-weight:400` (brödtext, stora tunna rubriker)
  - `Gotham-Medium.otf` → `font-weight:500` (etiketter, knapptext, mellanrubriker)
  - `Gotham-Bold.otf` → `font-weight:700` (KPI-tal, tunga rubriker)
  - Fallback-stack: `'Gotham','Segoe UI',system-ui,sans-serif`. Brödtext i appen
    använder medvetet systemfont (`--font-body`), rubriker använder Gotham (`--font-head`).
- **Logotyper** (i `assets/`, färgkorrigerade, currentColor-fria):
  - `Sokigo-logo-blue-pos.svg` — på ljus bakgrund (standard)
  - `Sokigo-logo-blue-neg.svg` — på mörk bakgrund
  - `Sokigo-logo-black.svg` — enfärgad mörk
  - (neg/vit variant finns för helmörka ytor)
- **Ikoner:** sharp-line-style i `icons/`, ritade med `stroke:currentColor`,
  `stroke-width:1.5`, `fill:none`. Inlina dem som SVG så de ärver textfärgen.
- **One Node Gradient:** radiell cyan glow (`radial-gradient(circle, rgba(62,177,200,0.45) 0%, rgba(62,177,200,0) 65%)`)
  bakom mörka hjältesektioner. Använd sparsamt som signaturdetalj.
- **Ton:** lugn, pålitlig, offentlig sektor. Inga emojis. Ingen slentriangradient.

---

## 2. Utskrift & PDF — KRITISKT

PDF skapas av **webbläsarens egen utskriftsmotor** (Chromium "Skriv ut → Spara som
PDF"). Det finns ingen weasyprint eller extern motor. Det betyder: **det du ser på
skärmen är det som hamnar i PDF:en, och vanlig CSS styr resultatet.**

### Bakgrundsfärger kommer INTE ut automatiskt om du inte tvingar det

Webbläsare tar bort bakgrundsfärger/-bilder vid utskrift som standard. En människa
kan kryssa i "bakgrundsgrafik" i dialogen — men förlita dig **aldrig** på det. Baka
istället in detta i CSS:en på varje dokument/sida som ska skrivas ut:

```css
@page { size: A4; margin: 0; }
@media print {
  html { -webkit-print-color-adjust: exact; print-color-adjust: exact; }
  html, body { margin: 0; padding: 0; background: #fff; }
}
```

Utan `print-color-adjust:exact` blir Steel Blue-rubriker, mörka kort och
tabellbakgrunder **vita** i PDF:en.

### Sidbrytningar

- Lägg `break-inside:avoid` på element som inte får delas mellan sidor: tabeller,
  callouts, KPI-kort, figurer, rubrik+ingress som hör ihop.
- `break-before:page` / `break-after:page` för medvetna sidbyten (t.ex. ny sektion).
- Bygg dokument som **fasta A4-sidor** med explicита brytpunkter — då blir
  skärm = PDF (WYSIWYG). Se `02-rapportmall.dc.html` som referensimplementation;
  den har redan rätt `@page`, `print-color-adjust` och `break-inside`.

### Löpande sidfot/sidhuvud + helt utfallande element (VANLIG FALLGROP)

För en sidfot (eller sidhuvud) som **upprepas på varje utskriven sida** används
spacer-tabell-knepet: lägg innehållet i en `<table>` med en tom `<thead>` och
`<tfoot>` som reserverar plats upptill/nedtill, och låt den verkliga sidfoten ligga
`position:fixed`. Webbläsaren upprepar `<thead>`/`<tfoot>` på varje sida, så texten
krockar aldrig med den fixerade sidfoten.

```css
.hdr-space,.ftr-space{display:none;}            /* dolda på skärm */
@media print{ .hdr-space,.ftr-space{display:table-cell;height:0.7in;} }
```

**Fallgropen:** spacern upprepas på *varje* sida – även den första. Lägger du ett
**helt utfallande element** (försättssida, mörk hjältesektion, sektionsavdelare som
ska gå kant-i-kant) *inuti* samma tabell, får det ett vitt band på 0,7 tum överst.
Bandet syns **bara i PDF/utskrift**, aldrig på skärm (där spacern är `display:none`)
— därför är det lätt att missa.

**Regel:** Helt utfallande element ska ligga **utanför** spacer-tabellen, som syskon.
Bara löpande brödtext (det som får brytas över sidor) läggs *inuti* tabellen.

```html
<main class="doc">
  <div class="doc-footer">…</div>            <!-- position:fixed, syns på alla sidor -->

  <!-- Helt utfallande omslag: UTANFÖR tabellen, full sidhöjd -->
  <div style="height:296mm;break-after:page;overflow:hidden;…">…</div>

  <!-- Brödtext: INUTI tabellen med spacers -->
  <table class="doc-frame">
    <thead><tr><td class="hdr-space"></td></tr></thead>
    <tbody><tr><td> …sektioner… </td></tr></tbody>
    <tfoot><tr><td class="ftr-space"></td></tr></tfoot>
  </table>
</main>
```

Tips: ge utfallande sidor `height:296mm` (inte `297mm`) + `break-after:page` så
slipper du en tom extrasida av avrundning. Spacerns höjd (`0.7in`) = den reserverade
topp/botten-marginalen för brödtexten; lägg därför ingen extra topp/botten-`padding`
på själva brödtextblocket, annars dubblas marginalen.

**Checklista vid nya dokumentmallar:** verifiera alltid i *utskriftsläge* (eller PDF),
inte bara på skärm — vita band, krockande sidfot och saknade bakgrundsfärger syns
bara där.

### Måttenheter för print

Använd fysiska enheter i dokument (`pt`, `in`, `mm`) för text och marginaler, inte
bara `px` — det ger förutsägbar utskrift. Minsta brödtext: 11–12pt.

---

## 3. Dokument-tokens (ljust, för rapporter/dokument)

| Roll | Värde |
|---|---|
| Accent / primär | `#3EB1C8` |
| Accent djup (länktext, etiketter) | `#1F7488` |
| Text rubrik | `#232323` / `#1B2A30` |
| Brödtext | `#333` |
| Svag text | `#5C6B72` |
| Linjer / ramar | `#E6E6E6` / `#E6EBEE` |
| Tabellhuvud-yta | `#F2F2F2` |
| Callout-yta (ljus cyan) | `#F7FBFC` med `border-left:3px solid #3EB1C8` |
| Mörkt kort | `#232323` (text `#EDEDED`), ev. med One Node Gradient |

Återkommande dokumentmönster (se rapportmallen): försättssida med metadata-rutnät,
numrerade sektioner (`01`, `02`…), Q&A-/datatabeller, "Ärlig avgränsning"-callout,
mörkt relevans-kort, sidfot med fixerad position.

---

## 4. App-tokens (Fenix) — tre teman

Appen använder CSS-variabler scoped under `.nova` med `data-theme="light|dark|hc"`.
**Kopiera token-blocket direkt från `03-app.dc.html`** (helmet `<style>`) när du
bygger nya app-skärmar — duplicera inte värden för hand. Sammanfattning:

**Ljust (default):** `--bg:#F4F7F8` · `--surface:#FFFFFF` · `--text-1:#1B2A30` ·
`--primary:#3EB1C8` · `--primary-active:#1F7488` · `--primary-subtle:#E4F3F6` ·
`--border:#E6EBEE` · `--danger:#D6534A`

**Mörkt:** `--bg:#10171E` · `--surface:#19222B` · `--text-1:#EAF1F4` ·
`--primary:#4FBDD2` (ljusare cyan för kontrast) · `--border:#2A3741`

**Högkontrast:** `--bg:#000` · `--surface:#000` · `--text-1:#FFF` ·
`--primary:#5FD0E6` · `--border:#FFF` · skuggor blir vita ramar (`0 0 0 1px #FFF`)

Konventioner i appen:
- Radie: små `5–6px`, kort `8px` (medvetet stramt, inte rundat "90-tal").
- Skuggor via `--shadow` / `--shadow-pop`.
- Vänster-rail `--rail:#1B2532` (mörk) i ljust/mörkt tema.
- Flikar: underline-stil (inte färgade block). Knappar: solid primär ELLER
  mjukt tonad (`--primary-subtle`-bakgrund + primärfärgad ram/text) för mildare uttryck.
- Status soft-tints: ljust tonad yta + ram i samma färg + matchande text
  (som statusnoteringar i tabeller). Undvik starka mättade gröna/röda fyllda block.
- Hit-targets minst 44px på mobil.

---

## 5. Mappstruktur & filkarta

Projektet har **två mappar** — håll isär dem:

- **`arbetsmaterial/`** — källfilerna (`.dc.html`) som vi redigerar, plus allt de
  behöver (`assets/`, `fonts/`, `icons/`, `support.js`, `branding/`). **Här arbetar du.**
- **`publicerat/`** — fristående, självständiga kopior (allt inbakat: typsnitt, logor,
  kod). Genereras från arbetsmaterialet när en sida är klar. **Redigera aldrig här.**

Källfiler i `arbetsmaterial/`:

- `00-oversikt.dc.html` — index/startsida för hela systemet
- `01-grund.dc.html` — varumärkesgrund (färg, typ, logo, ikoner)
- `02-rapportmall.dc.html` — utskriftsklar A4-rapport (print-referens)
- `03-app.dc.html` — Fenix app-skal + skärmar (app-token-referens)
- `04-vyer.dc.html` — vy-mönster (master–detalj, dialog, stegguide, tomt läge)
- `05-komponenter-bas.dc.html` — Bas (åtgärder, formulär, navigation)
- `06-komponenter-yta.dc.html` — Yta & indikatorer (alerts, badges, splitter…)
- `07-komponenter-data.dc.html` — Interaktion & data (tabell, palett, 2FA…)
- `assets/` — logotyper · `fonts/` — Gotham · `icons/` — sharp-line-ikoner

`CLAUDE.md` ligger i **roten** (måste göra det för att läsas automatiskt).

### Navigation (hub-and-spoke)

`00-oversikt.dc.html` är hubben som listar allt. Varje annan sida har en
**"← Översikt"**-länk tillbaka dit (i chrome på app-sidor, i hjälten på
dokument). På rapporten är länken `class="screen-only"` så den **inte** följer
med i utskrift/PDF. Lägger du till en ny sida: ge den samma "← Översikt"-länk och
lista den på översikten.

### Arbetsflöde: bygg → publicera

När en sida är klar, generera dess fristående kopia till `publicerat/` med
**samma filnamn** (så att översiktens länkar funkar mellan de publicerade sidorna):
`super_inline_html(arbetsmaterial/<fil> → publicerat/<fil>)`. Varje källfil har en
`<template id="__bundler_thumbnail">` i `<head>` som krävs för bygget — behåll den.
Använder en sida en asset via ett dynamiskt värde (t.ex. `src="{{ x }}"`) kan
bundlern inte baka in den — gör sökvägen literal (ev. med `<sc-if>`-växling).

När du bygger något nytt: **forka närmaste befintliga fil** istället för att börja
från tomt — då ärver du tokens, typsnitt och mönster korrekt.

Filerna är **numrerade** (`00-`–`07-`) för automatisk sortering i utforskare/Finder.
Byt inte tillbaka till produkt-/varumärkesprefix i filnamn — numrering + tydligt
funktionsnamn räcker. Lägger du till en ny sida: ge den nästa lediga nummer
(i `arbetsmaterial/`) och publicera den till `publicerat/` med samma namn.

---

## 6. Versionsstämpel

Varje sida visar en versionsstämpel **`datum · rev N`** (t.ex. `2026-06-27 · rev 1`)
i sidfot (dokument/index) eller chrome (app-sidor). Konventionen:

- **Datum** = ISO-format `ÅÅÅÅ-MM-DD`, dagen för ändringen.
- **Löpnummer** = `rev N`, höjs med 1 vid varje förändring (oavsett datum).
- Uppdatera stämpeln på den/de sidor du ändrar — så vet medarbetare och AI:ar exakt
  vilken version som gäller.

### OBLIGATORISKT efter varje ändring av designsystemet

När du justerat **något** i designsystemet (en sida, en token, en komponent, eller
den här `CLAUDE.md`) ska du **alltid**, utan att fråga, göra båda dessa steg innan du
är klar:

1. **Höj versionsstämpeln** (`datum` → dagens datum, `rev N` → `N+1`) på den/de
   ändrade sidorna. Ändrade du `CLAUDE.md` eller varumärkesgrunden, höj även på
   `00-oversikt.dc.html` så hubben speglar att systemet rörts.
2. **Bygg om den publicerade kopian** för varje ändrad sida:
   `super_inline_html(arbetsmaterial/<fil> → publicerat/<fil>)` (samma filnamn).
   Arbetskopia och publicerad kopia får aldrig hamna i otakt.

Detta är inte valfritt och ska inte kräva en påminnelse från användaren.

---

## 7. Snabb checklista innan leverans

- [ ] Steel Blue `#3EB1C8` som accent, inte stor bakgrundsyta
- [ ] Gotham laddad via `@font-face` från `fonts/`
- [ ] Rätt logotypvariant för bakgrunden (pos/neg/black)
- [ ] **Dokument/print:** `@page`, `print-color-adjust:exact`, `break-inside:avoid`
- [ ] **App:** token-block kopierat från `03-app.dc.html`, alla tre teman fungerar
- [ ] Inga emojis, inga slentrianmässiga gradienter, stram radie
- [ ] Versionsstämpel uppdaterad (`datum · rev N`) på ändrade sidor — **obligatoriskt**
- [ ] Fristående kopia ombyggd till `publicerat/` (samma filnamn) efter ändring — **obligatoriskt**
