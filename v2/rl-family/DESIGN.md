# RL-familien — designkontrakt ("stadium night")

*Skrevet 22/9-2026 (UI 5.5). Gælder de RL-web-flader, der inviterer fremmede ind. Selve appen (RLLiveTracker.html, NITRO-skinnet) og rapporterne har deres egne tokens i rl-live-stats og er ikke omfattet her, men deler palet og skrift.*

Fladerne i dag, som hver bærer deres egen kopi af reglerne nedenfor, fordi de ligger på fire forskellige origins:

| Flade | Fil | Accent |
|---|---|---|
| rltracker.gitato.net | `G:\gitato-deploy\rltracker\index.html` (+ 404.html) | boost |
| rankoverlay.gitato.net | `rl-rank-overlay\docs\index.html` | blue |
| roll.gitato.net | `rl-training-randomizer\docs\index.html` | boost (download) + line (browser) |
| roll.gitato.net/app/ + lokal RLRoll | `rl-training-randomizer\public\index.html` → build-site → `docs\app\` | boost |
| rl-coach.gitato.net/portal, /console | `rl-live-stats\collector\portal.js` (PAGE-strengen) | boost |
| rl-coach.gitato.net/get | `/var/www/rl-dl/index.html` på collect-1 (lokal kilde i UI 5.5-scratchpad) | boost |

Ændres noget her, skal det rettes alle seks steder. Det er prisen for, at ingen side henter CSS på tværs af origins.

## 1. Palet

Målt mod de fire flader, tekst kan lande på (WCAG-kontrast):

| Token | Værdi | night `#101A2E` | deck `#172440` | deck2 `#1C2C50` | well `#070D19` | Rolle |
|---|---|---|---|---|---|---|
| `--ink` | `#EAF2FF` | 15.4 | 13.7 | 12.2 | 17.3 | overskrifter, vigtig tekst |
| `--ink2` | `#B8C7E0` | 10.2 | 9.0 | 8.1 | 11.4 | brødtekst |
| `--ink3` | `#8CA0BF` | 6.5 | 5.8 | 5.2 | 7.3 | meta, etiketter; laveste tekstfarve |
| `--line` | `#27D9C0` | 9.7 | 8.6 | 7.7 | 10.9 | banelinje-teal: tags, sekundær CTA, fokusring |
| `--boost` | `#FF9E2C` | 8.4 | 7.5 | 6.7 | 9.4 | primær CTA, tal-accent |
| `--blue` | `#38B6FF` | 7.7 | 6.8 | 6.1 | 8.6 | links, overlay-produktets accent |

Tekst PÅ en oplyst flade: `--on-boost #1A1206` (9,0:1 på boost), `--on-line #072019` (9,6:1 på line), `#06131F` på blue (8,3:1). Hvid tekst på de tre accenter er forbudt, samme regel som DS v2.

Kanter: `--edge rgba(184,199,224,.13)`. Sænket felt (inputs, koder): `--well #070D19` med `inset 0 1px 3px rgba(0,0,0,.75)`.

## 2. Skrift

**Archivo, variabel, én fil** (`archivo-var.woff2`, 90 kB, wght 100-900, wdth 62-125, målt på filen). Den bærer alt undtagen GITATO-mærket. **Bredden er hierarkiets akse**, ikke kun vægten:

| Rolle | stretch | vægt | tracking | case |
|---|---|---|---|---|
| Display / h1 | 112-118 % | 840-860 | -.005em | UPPERCASE |
| Kortoverskrift | 100 % | 700-750 | .005em | normal |
| Etiket / eyebrow / family-bar | 72-88 % | 600-700 | .14-.20em | UPPERCASE |
| Brødtekst | 100 % | 400-420 | 0 | normal |

**Orbitron 800** (6,7 kB, latin) kun til ordmærket GITATO øverst til venstre, `letter-spacing:.24em`.

Preload begge i `<head>` (`<link rel="preload" … as="font" crossorigin>`). Uden preload swappede Archivo ind ~1,4 s efter første maling, overskriften brød om og siden sprang 50 px (målt på overlay-landingen 22/9: CLS 0,275 → 0 efter preload).

**Reserveflader med Archivos mål (23/9).** Preload alene holdt ikke på en kold, langsom indlæsning: swappet flyttede stadig hver tekstlinje 3-9 px (tracker 390 px: CLS 0,15; overlay 1024: 0,14). Hver side har derfor tre reservefamilier lige efter Archivos `@font-face`: `'Archivo FB'` (Segoe UI, Windows), `'Archivo FB A'` (Arial, macOS/iOS) og `'Archivo FB R'` (Roboto, Android), hver i tre vægtspænd (100-549 / 550-749 / 750-900). De har Archivos linjemål (`ascent-override` 87,8 % og `descent-override` 21,0 %, begge divideret med `size-adjust`) og er skaleret til Archivos bredde på sidens egen tekst. Stakken er `'Archivo', 'Archivo FB', 'Archivo FB A', 'Archivo FB R', 'Segoe UI', system-ui, sans-serif`. Tallene er regnet ud med fontTools på selve fontfilerne (Roboto-bredden fra capsize-metrikkerne), med overskriftsmålet som parameter: wdth 118 / wght 840 på trackeren, 105 / 850 på overlay og roll. Resultat: CLS ≤ 0,07 i alle målte størrelser, og Archivo ses stadig ved første besøg.
`font-display: optional` blev prøvet og forkastet: ved 30 Mbit/s og 40 ms fik overlay og roll ved første besøg Segoe UI i overskriften, og en landingsside får mest førstegangsbesøg. Behold `swap`.

## 3. Komponenter

**Fræset plade** (kort, privatliv-boks, portal-kort, roll-kontroller): ét skåret hjørne øverst til højre, lyst 1 px chamfer mod lyset.
```css
background:
  linear-gradient(187deg, rgba(255,255,255,.05) 0%, rgba(255,255,255,.01) 34%, rgba(0,0,0,.06) 58%, rgba(190,220,255,.02) 100%),
  linear-gradient(163deg, #1E2B47 0%, #172440 44%, #0C1526 100%);
border: 1px solid var(--edge);
border-radius: 3px 13px 3px 3px;
box-shadow: inset 0 1px 0 rgba(196,224,255,.24), inset 1px 0 0 rgba(196,224,255,.11),
            inset -1px 0 0 rgba(0,0,0,.38), inset 0 -1px 0 rgba(0,0,0,.60),
            0 1px 0 rgba(0,0,0,.85), 0 10px 24px -12px rgba(0,0,0,.6);
@supports (corner-shape: bevel) { corner-shape: round bevel round round; }
```
`corner-shape:bevel` i stedet for `clip-path`: clip-path æder den ydre skygge (NITRO-fundet 27/8).

**Primær CTA** (boost): lys top-gradient over `linear-gradient(180deg,#FFC271,var(--boost) 55%,#D8801A)`, `0 0 0 1px var(--boost-dk)` som kant, varm glød `0 0 26-30px -8px rgba(255,158,44,.6)`. Overlay-landingen bruger blue i samme opskrift.

**Family-bar** (øverst på alle tre produktlandinger, identisk markup):
```html
<header class="top">
  <a class="mark" href="https://gitato.net/">GITATO</a>
  <span class="crumb">/ Produktnavn</span>
  <nav class="family" aria-label="Rocket League tools">
    <a href="https://rltracker.gitato.net/">Tracker</a>
    <a href="https://rankoverlay.gitato.net/">Overlay</a>
    <a href="https://roll.gitato.net/">Roll</a>
  </nav>
</header>
```
`aria-current="page"` på den side, man står på. Under 560 px skjules crumb.

**Handoff til telefon** (kun Windows-produkter): en boks under download-knappen, `hidden` som standard. Scriptet viser den kun når `(pointer: coarse)` og brugeragenten ikke er Windows. Knappen bruger `navigator.share`, ellers clipboard, ellers vises URL'en som tekst. Grund: Reddit-trafik er mest mobil, og en Windows-app kan ikke installeres dér, hvor de står.

## 4. Regler

1. **Ingen tredjeparts-requests for at tegne siden.** Fonte og billeder ligger på sidens egen origin. På rl-coach.gitato.net sætter Caddy `default-src 'self'`, så dér er det ikke bare en god idé, men et krav: brug `/portal/assets/…`.
2. **Hver side har et 1200×630 delingskort** (`og.jpg`) + `twitter:card=summary_large_image` + canonical. Kilde: `design-system\v2\og\rl-family-og.html`.
3. **Disclaimer i footeren:** "Not affiliated with or endorsed by Psyonix or Epic Games." Fjernes aldrig (samme regel som rank-badges-disclaimeren).
4. **Fakta skal kunne efterprøves.** Ini-filen hedder `DefaultStatsAPI.ini` (i `<RL install>\TAGame\Config\`). Tal, der ændrer sig (antal pakker, versioner), skrives ikke ind i statiske billeder. Brug "thousands of packs" eller hent tallet live.
5. **Download-links peger på `releases/latest/download/…`**, aldrig på en kopi af filen på egen server. /get lå en måned bagud, fordi den havde sin egen exe.
6. Al ny offentlig tekst går gennem de tre anti-AI-skills før deploy.
