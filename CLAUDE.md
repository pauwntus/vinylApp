# Vinyl — iOS Music App

Alternativ Apple Music-klient där känslan av att äga och bläddra bland fysisk media är kärnan. Ingen discovery, inga algoritmer — bara ditt bibliotek.

## Koncept

Appen simulerar en fysisk skivsamling. Varje interaktion ska kännas som att röra vid ett riktigt föremål.

### Album (vinyl)
- Visas som omslag i ett 3-kolumns grid
- Press lyfter skivan — den glider ut ur fodralet med en kort animation
- Öppnar en sheet med suddigt album art som bakgrund och tracklista

### Spellistor (kassettband)
- Visas som kassetter i genomskinliga plasthöljen, 3 per rad
- Kassettfärg genereras deterministiskt via hash på spellistenamnet (alltid samma färg för samma spellista)
- Etikettstil baseras på namnlängd:
  - 0–10 tecken → Dymo (tryckt text)
  - 11–16 tecken → maskeringstejp (handskrivet)
  - 17–24 tecken → sharpie (markör)
  - 25+ tecken → sharpie i minifont
- Långa namn trunkeras vid ordgräns med `…` på etiketten; fullt namn visas i detaljvyn
- Öppnar en sheet där kassetlocket roterar upp (3D-transform) och spårlistan delas i Sida A / Sida B

## Teknik

| Källa | Användning |
|---|---|
| MusicKit (Apple Music API) | Biblioteksdata — album, spellistor, metadata |
| iTunes Search API | Album art (gratis, ingen API-nyckel krävs) |
| MusicBrainz / Cover Art Archive | Baksidesomslagsbild vid behov |

**Touch:** Använd `touchstart`/`touchend` för interaktion. Ingen hover-logik — appen är touch-first.

## Design

CSS-variabler (definierade i `:root`):

```
--bg:       #0e0c09   (djup varm svart)
--surface:  #161410
--text:     #e8ddd0
--text-dim: #5a4e40
--accent:   #c8a97e   (varm guld)
--border:   rgba(255,255,255,0.05)
```

Typsnitt:
- UI-text: `DM Mono`
- Albumtitel: `Playfair Display`
- Dymo-etikett: `Rubik Mono One`
- Maskeringstejp: `Special Elite`
- Sharpie: `Permanent Marker` / `Caveat`

Subtil brusöverläggning (SVG `feTurbulence`) ligger alltid överst med `pointer-events: none`.

## Filstruktur

Projektet är just nu ett single-file HTML-prototyp:

- `index.html` — aktiv version
- `vinyl-app-v3.html` — tidigare iteration (referens)

## Utvecklingsprinciper

- Inga algoritmer, inga rekommendationer — biblioteket är alltid utgångspunkten
- Animationer ska kännas fysiska och ha rätt tröghet (ease-out, inga linjära kurvor)
- Minimalt UI-chrome — innehållet är gränssnittet
- Allt ska fungera offline efter initial dataladdning
- Håll JavaScript inline i HTML tills projektet växer tillräckligt för att motivera en byggprocess
