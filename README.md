# SOOFIE

Een Nederlandstalige muziekblog gebouwd met Hugo en PaperMod.

## Lokaal bekijken

Installeer Hugo en start daarna de ontwikkelserver:

```sh
hugo server
```

Open vervolgens <http://localhost:1313/>. De server ververst de site automatisch wanneer je bestanden wijzigt.

## Een blogbericht toevoegen

Maak een Markdown-bestand in `content/posts/`, bijvoorbeeld `content/posts/nieuw-nummer.md`:

```markdown
---
title: "De titel van je bericht"
date: 2026-09-29
draft: false
---

Schrijf je bericht hier.
```

Maak voor de Engelse versie een bestand met dezelfde naam en de taalcode, bijvoorbeeld `content/posts/nieuw-nummer.en.md`. Engelse pagina's staan op de site onder `/en/`.

## Huisstijl aanpassen

- Kleuren en PaperMod-stijlen: `assets/css/extended/soofie.css`
- Lokale webfonts: `assets/css/extended/fonts.css` en `static/fonts/`
- Site-instellingen en navigatie: `hugo.yaml`

De site is ingesteld voor publicatie op <https://soofie.nl/> via GitHub Pages. Bij iedere push naar de `main`-branch bouwt en publiceert GitHub Actions de site automatisch.

Zet in GitHub bij **Settings → Pages → Build and deployment** de bron op **GitHub Actions**. Controleer daar ook dat het aangepaste domein `soofie.nl` is ingevuld en dat **Enforce HTTPS** is ingeschakeld zodra GitHub het certificaat heeft uitgegeven.

PaperMod is opgenomen als Git-submodule. Na het clonen van de repository initialiseer je het thema met `git submodule update --init --recursive`.
