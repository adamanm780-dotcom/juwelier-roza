# CLAUDE.md – Juwelier Roza Website

## Projekt-Übersicht

Einseitige Premium-Website für **Juwelier Roza**, Wiesbaden.

- **Live:** https://adamanm780-dotcom.github.io/juwelier-roza/
- **Repo:** https://github.com/adamanm780-dotcom/juwelier-roza
- **Lokales Arbeitsverzeichnis:** `C:/Users/Adria/Documents/GitHub/juwelier-roza/`

Alle Änderungen hier bearbeiten, committen und nach `main` pushen. GitHub Pages serviert direkt aus dem `main`-Branch (Root `/`).

---

## Dateistruktur

```
index.html          ← komplette Website (CSS + JS inline, ~2200 Zeilen)
uizu.mp4            ← Hero-Hintergrundvideo (H.264, loop-gebacken via xfade)
huip.webm           ← Animations-Logo über piner (VP9, yuva420p, Alpha-Kanal)
huip.mp4            ← Fallback für huip.webm (H.264, 10fps)
zui.png             ← Mobiles Ersatzbild für die huip-Animation
Bilder/
  piner.png         ← Hauptlogo (PNG)
CLAUDE.md           ← diese Datei
```

---

## Design-System (CSS-Tokens)

| Token           | Wert        | Verwendung                     |
|-----------------|-------------|--------------------------------|
| `--ivory`       | `#FAF8F4`   | Seitenhintergrund              |
| `--cream`       | `#F3EDE2`   | Karten, Sektionen              |
| `--gold`        | `#C8A05C`   | Akzentfarbe, Buttons           |
| `--gold-light`  | `#DBBE8A`   | Hover-Zustände                 |
| `--charcoal`    | `#1A1614`   | Primäre Textfarbe              |
| `--font-display`| Cormorant Garamond | Überschriften           |
| `--font-body`   | Jost        | Fließtext, Navigation          |

---

## Seitenstruktur (HTML-Sektionen)

1. **`<nav>`** – Fixierte Navigation, wird bei Scroll mit Hintergrund gefüllt (`is-scrolled`)
2. **`<section class="hero">`** – Fullscreen-Videohintergrund mit Logo, Animation, Headlines, CTAs
3. **`<div class="vine-scene" id="vineScene">`** – Goldene Ranken-Szene (Canvas, nur Desktop)
4. **`<section class="about">`** – Über uns / Markenbotschaft
5. **`<section class="leistungen">`** – Leistungskarten (Reparatur, Ankauf, Neuanfertigung, Gravur)
6. **`<section class="kollektion">`** – Kollektions-Galerie
7. **`<section class="kontakt">`** – Kontaktformular + Karte + Öffnungszeiten
8. **`<footer>`** – Impressum-Links, Copyright

---

## Hero-Sektion – wichtige Details

### Video (`uizu.mp4`)
- Trimmed auf 9,54 s, nahtloser Loop via ffmpeg xfade (0,8 s Crossfade bei Offset 7,94 s)
- `autoplay muted playsinline loop` – läuft nativ ohne JS-Tricks
- Overlay: `rgba(10,8,7, 0.55)` oben → `0.32` Mitte → `0.72` unten + radiales Vignetting

### Animations-Logo (`huip`)
- Klasse `.hero__rizz`, Höhe 90 px, Margin-Bottom −18 px
- JS `initRizz()` erstellt `<video>` mit `src="huip.webm"`, Fallback `huip.mp4`
- Wird unsichtbar geladen, nach `canplay` wird `.is-visible` gesetzt (CSS-Fade)
- **Auf Mobile (≤768 px) ausgeblendet** (`display:none !important`)

### Mobile-Layout
- ≤768 px: `.hero__rizz` versteckt, `.hero__zui` zeigt `zui.png` an
- `.hero__content` bekommt `padding-top: 18vh` (480 px: 15vh)
- Logo (`piner.png`): 160 px → 120 px → 96 px

---

## Goldene Ranken-Szene (`vineScene`)

- Nur Desktop (≥1024 px, via `matchMedia`)
- `<div class="vine-scene">` sitzt **zwischen Hero und About**, `position:relative`, kein Scrollen
- Canvas `position:absolute`, füllt die Szene komplett
- **Alle Farben rein golden** (kein Grün): `rgba(185,145,55,…)` Stamm, `rgba(230,195,110,…)` hell, `rgba(150,110,35,…)` tief
- 6 Startpunkte: 3 links, 3 rechts am Szenenrand
- Features: Ranken, Spirallocken (Tendrils), spitze Blätter, mehrstufige Rosen
- **Maus-Interaktion**: Glow-Effekt bei Nähe (<180 px)
- **Klick auf Rankenspitze** (<70 px): Goldene Blütenblätter fallen (Physik-Animation)
- Render-Loop: `requestAnimationFrame`, gedrosselt auf ~35 fps

---

## Deployment-Workflow

```bash
cd "C:/Users/Adria/Documents/GitHub/juwelier-roza"
# Dateien bearbeiten, dann:
git add index.html
git commit -m "Kurze Beschreibung"
git push
# → GitHub Pages aktualisiert sich nach ca. 1–2 Minuten
```

---

## Bekannte Fallstricke

| Problem | Ursache | Fix |
|---------|---------|-----|
| `<source>`-Fehler bei Video-Animation | Fehler bubblen nicht zum `<video>`-Element | `video.src = url` direkt setzen, kein `<source>`-Kind |
| Video stoppt nach 1–2 Loops | `requestVideoFrameCallback` bricht nach `currentTime=0`-Seek ab | Nativer `loop` + xfade ins Video gebacken |
| Canvas-Pixel-Keying blockiert Video | 60fps-Hauptthread-Loop | Ersetzen durch WebM VP9 mit nativem Alpha-Kanal |
| Vine-Szene nicht zu sehen | Desktop-Only-Guard `matchMedia('(min-width:1024px)')` | Auf echtem Desktop testen, nicht unter 1024 px |

---

## Kontaktdaten (im Footer / Kontakt-Sektion)

- **Adresse:** Wellritzstraße 29, 65183 Wiesbaden
- **Tel:** 0611 73419779
- **Öffnungszeiten:** Mo–Sa 10–18 Uhr
