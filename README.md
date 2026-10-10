# SOOFIE

De website van SOOFIE: muziek, verhalen achter nummers en updates over nieuwe projecten.
De ingestelde publieke URL is [soofie.nl](https://soofie.nl/).

## Techniek en opbouw

De website wordt als statische HTML gebouwd met **Hugo Extended** en het thema
**PaperMod**. Er is geen eigen backend, database of Node.js-installatie nodig.
De publicatieworkflow gebruikt Hugo **0.167.0**; gebruik lokaal dezelfde versie
om verschillen in de uitvoer te voorkomen. PaperMod is opgenomen als Git-submodule.

| Bestand of map | Functie |
| --- | --- |
| `hugo.yaml` | Domein, talen, menu's, metadata en introductieteksten |
| `content/` | Pagina's in Markdown, met YAML-frontmatter |
| `content/posts/` | Blogberichten |
| `layouts/` | Eigen templates die de PaperMod-templates overschrijven |
| `assets/css/extended/` | Eigen stijlen, automatisch verwerkt door het thema |
| `static/fonts/` | Lokaal gehoste WOFF2-lettertypen |
| `static/CNAME` | Domeinbestand dat Hugo naar de publicatiemap kopieert |
| `CNAME` | Domeinbestand in de repository; houd dit gelijk aan `static/CNAME` |
| `themes/PaperMod/` | Vastgelegde versie van het externe thema |
| `.github/workflows/hugo.yml` | Bouwen en publiceren naar GitHub Pages |
| `public/` | Gegenereerde website; niet opgenomen in Git |

## Lokaal werken

Installeer Git en Hugo Extended 0.167.0. Voer vervolgens vanuit de hoofdmap uit:

```sh
git submodule update --init --recursive
hugo version
hugo server -D
```

Open het lokale adres dat Hugo toont, normaal `http://localhost:1313/`.
Hugo verwerkt wijzigingen tijdens het ontwikkelen automatisch. `-D` toont ook
concepten met `draft: true`; deze worden niet door de normale productiebuild gepubliceerd.

Bouw de website zoals in de publicatieworkflow:

```sh
HUGO_ENVIRONMENT=production HUGO_ENV=production hugo --gc --minify
```

De uitvoer staat in `public/`. Bewerk deze bestanden niet rechtstreeks: een
volgende build genereert ze opnieuw. Ook `resources/_gen/` en `.hugo_build.lock`
zijn lokale Hugo-bestanden en worden genegeerd door Git.

## Pagina's en talen

Nederlands is de standaardtaal en staat direct onder `/`. Engels staat onder
`/en/`. Vertalingen delen dezelfde bestandsnaam; de Engelse variant krijgt
`.en` vóór `.md`.

| Pagina | Nederlands | Engels |
| --- | --- | --- |
| Startpagina | `content/_index.md` → `/` | `content/_index.en.md` → `/en/` |
| Over SOOFIE | `content/about.md` → `/about/` | `content/about.en.md` → `/en/about/` |
| Welkomstbericht | `content/posts/welkom.md` → `/posts/welkom/` | `content/posts/welkom.en.md` → `/en/posts/welkom/` |

De blogoverzichten staan op `/posts/` en `/en/posts/`. Hugo genereert deze uit de
berichtenmap. De zichtbare introductie op de startpagina komt uit
`languages.<taal>.params.homeInfoParams` in `hugo.yaml`; pas die tekst daar aan.
De menu's en omschrijvingen voor beide talen staan eveneens in `hugo.yaml`.

### Een bericht toevoegen

Maak bijvoorbeeld `content/posts/nieuw-project.md`:

```yaml
---
title: "Een nieuw project"
date: 2026-10-10
draft: true
---

Schrijf hier de tekst van het bericht in Markdown.
```

Voeg de vertaling toe als `content/posts/nieuw-project.en.md`, met een Engelse
titel en tekst. Controleer beide versies lokaal en zet `draft` op `false` wanneer
het bericht gepubliceerd mag worden. Gebruik de bedoelde publicatiedatum;
berichten met een toekomstige datum worden standaard niet gebouwd.

Voor een gewone pagina kan het `date`-veld worden weggelaten, zoals bij de
bestaande Over-pagina's. Voeg een nieuwe menulink indien nodig in beide talen toe.
Afbeeldingen kunnen bijvoorbeeld in `static/images/` worden geplaatst en vanuit
Markdown worden gebruikt als `![Beschrijvende alternatieve tekst](/images/bestand.jpg)`.

## Vormgeving en toegankelijkheid

De website combineert roze en paarse kleuren met een decoratief terminalvenster.
De standaardweergave is licht; er zijn ook kleuren voor de donkere weergave.

- `fonts.css` definieert DM Sans en Playfair Display met lokale lettertypebestanden.
- `soofie.css` bevat de kleuren, typografie, focusmarkering en skiplink.
- `terminal.css` bevat het terminalvenster, opdrachtregelaccenten, mobiele aanpassingen
  en de knipperende cursor. De cursor respecteert `prefers-reduced-motion`.
- `layouts/_default/baseof.html` bepaalt de paginaomhulling, terminalbalk en skiplink
  naar `#main-content`.
- `layouts/partials/home_info.html` toont de introductie met decoratieve opdrachten.
- `layouts/partials/extend_head.html` geeft de themaknop een Nederlands toegankelijk label.
- `layouts/404.html` bevat een foutpagina met teksten voor beide talen.
- `layouts/partials/header.html` en `translation_list.html` gebruiken het actuele
  Hugo-veld `Language.Label` voor taalnamen.
- `layouts/rss.xml` en `layouts/partials/templates/opengraph.html` gebruiken
  `Language.Locale` voor taalcodes in RSS en Open Graph. Deze vier lokale
  PaperMod-overschrijvingen voorkomen waarschuwingen over verouderde taalvelden;
  vergelijk ze bij een thema-update opnieuw met de oorspronkelijke templates.

Plaats eigen wijzigingen in deze projectbestanden, niet rechtstreeks in de
PaperMod-submodule. Behoud zichtbare toetsenbordfocus, de skiplink en de
`aria-hidden`-markeringen op decoratieve terminalonderdelen.

## Publicatie en onderhoud

Een push naar `main` start `.github/workflows/hugo.yml`. De workflow kan ook
handmatig worden gestart via `workflow_dispatch` in GitHub Actions.
De workflow installeert Hugo Extended met een vastgelegde checksum, haalt het
repository inclusief submodule op, bouwt de site en publiceert `public/` naar
GitHub Pages via de omgeving `github-pages`.

Voor publicatie moet GitHub Pages in de repository zijn ingesteld op GitHub
Actions. Het aangepaste domein en de bijbehorende DNS-instellingen worden buiten
deze broncode beheerd. Bij een domeinwijziging moeten `baseURL` in `hugo.yaml`,
beide `CNAME`-bestanden en de Pages-/DNS-instellingen op elkaar aansluiten.

Werk PaperMod alleen bewust bij: de repository legt één submodule-commit vast.
Controleer na een thema-update vooral de eigen template-overschrijvingen.
Bij een Hugo-update moeten versie en SHA256-checksum in de workflow samen worden
bijgewerkt en moet de nieuwe build lokaal worden gecontroleerd.

## Controle vóór publicatie

Er is geen aparte geautomatiseerde testsuite in deze repository. Gebruik de
productiebuild als technische basiscontrole. Controleer bij zichtbare wijzigingen
ook beide talen, menu's en taalwissel, mobiel en desktop, lichte en donkere
weergave, toetsenbordnavigatie en de foutpagina's (`/404.html` en `/en/404.html`).
Een geslaagde build bewijst niet dat de live publicatie of DNS-configuratie werkt;
controleer na publicatie ook de GitHub Actions-run en de website.

## Lokale agentinstructies

`AGENTS.md` bevat werkinstructies voor agents in deze checkout. De bestaande
`.gitignore` sluit dit bestand en `private-docs/` bewust uit van versiebeheer.
Een nieuwe clone krijgt deze lokale bestanden dus niet automatisch mee.
