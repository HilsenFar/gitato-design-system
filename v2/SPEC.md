# GITATO Design System v2 — bindende spec

*Skrevet 25. juli 2026. Alle tal i dette dokument er målt i browseren, ikke skønnet.*

---

## A. Designtesen

Siden er en anodiseret nær-sort stålplade. Kontroller er plader skåret og nittet fast
på den. Neon er ikke en `box-shadow` smurt ud under en kasse — det er en **emissiv
plade**: en oplyst flade der bærer mørk stenciltekst, præcis som ægte oplyst skiltning.

Det er et designvalg med en måling bag sig: **hvid tekst fejler WCAG AA på hver
eneste af vores signalfarver** (bedste tilfælde 3.86:1 på violet). **Mørk tekst
består på dem alle** (værste tilfælde 5.17:1 på violet). Oplyst flade med mørk tekst
er både det mere autentiske udtryk og det eneste tilgængelige.

### De fem regler enhver flade skal følge

1. **En rå signalfarve må aldrig bære tekst.** `--gt-cyan` fylder en plade, tegner en
   streg eller fylder en glyf. Tekst bruger `--gt-*-ink`, eller `--gt-ink-on-lit` når
   den ligger på en oplyst flade. Følges den regel, kan ingen tekst i systemet falde
   under 4.5:1 — uanset hvilken flade den lander på.
2. **`--gt-ink-4` er ikke en tekstfarve.** Den er til skillelinjer, inerte glyffer og
   nitte-skygger. Den måler 3.06:1 på den lyseste flade. *(Første udkast af dette
   system brød selv reglen i `.gt-serial` og målte 4.15:1. Den er rettet.)*
3. **Farve må aldrig være den eneste semantiske kanal.** Aktiv/fare/ok skal også
   bæres af form, ikon, kantbånd eller vægt. RTS'ens menu kodede fire tilstande i
   fire farver og intet andet; RL-trackerens hold er blå mod orange på 4px målere,
   hvilket er netop det par deuteranope-brugere har sværest ved.
4. **Alt klikbart skal have `:focus-visible`.** Der findes i dag **0** i hele
   porteføljen. Brug `--gt-focus-ring`.
5. **Dybde kommer fra fladens lyshed, ikke fra skygger.** Se afsnit E.

---

## B. Filer

| Fil | Rolle |
|---|---|
| `gitato-ds.css` | Hele systemet. Kopieres som `ds.css` ind i hver flades mappe. |
| `icons.svg` | 76 originale ikoner, `gi-*`. Kopieres med. |
| `gallery.html` | Levende komponent-galleri med temavælger. Kontrolleret 2/9-2026 i alle 10 temaer: 0 aktiverede tekstelementer under AA, laveste forhold 5.08:1 (se afsnit D). |
| `icons-preview.html` | Ikon-korrekturark. |
| `type-lab.html`, `texture-lab.html`, `plate-lab.html` | Beslutningsgrundlaget. Behold dem — de dokumenterer *hvorfor*. |

Indsæt **før** fladens egne styles, så fladen kan overskrive:

```html
<link rel="stylesheet" href="ds.css">
```

Sæt temaet på `<body>`: `<body data-gt="hub">` (eller `game`, `rts`, `beatsurfer`,
`frequencypilot`, `addson`, `mediestudio`, `sprunkiverse`, `hector`, `rltracker`).

---

## C. Typografi — og hvorfor den skiftede

**Den vigtigste enkeltfejl i v1:** `game/style.css` beder om `Rajdhani`, men fonten
hentes aldrig. Kun `beatsurfer/index.html` loader rent faktisk Google Fonts.
Verificeret ved grep: 8 forekomster af `Orbitron|Rajdhani|fonts.googleapis` i hele
deploy-træet, fordelt på 3 filer. **Brandtypografien nåede skærmen på 1 ud af 10
flader.**

Fem opsætninger blev sat op mod hinanden i `type-lab.html` og bedømt på skærm:

| | Logo | UI | Brødtekst | Dom |
|---|---|---|---|---|
| 1 | Orbitron | Rajdhani | Rajdhani | Rajdhani er kondenseret — fin til HUD, hårdt arbejde at læse et afsnit i |
| 2 | **Orbitron** | **Chakra Petch** | **Barlow** | **Valgt** |
| 3 | Orbitron | Barlow | Barlow | Læsbar, men overskrifterne mister teknisk karakter |
| 4 | Chakra Petch | Saira | IBM Plex | GITATO-logoet mister sin genkendelighed — afvist på brand-DNA |
| 5 | Orbitron | Barlow Condensed | Barlow | Tættere på 2, men mindre karakter i UI'et |

- **Orbitron** — kun logotypen. Det er brandmærket, og det bliver.
- **Chakra Petch** — UI, overskrifter, HUD, tal. Kantet og teknisk, og markant mere
  læsbar end Orbitron under ~20px.
- **Barlow** — brødtekst.

**Undtagelse: RL-familien (besluttet 3/9-2026).** RL Live Tracker-appen, rapporterne,
portalen på collect.gitato.net, RL Rank Overlay, RL Roll (roll.gitato.net) og
tracker-landingen (gitato.net/rl-tracker + rltracker.gitato.net) bærer NITRO-skinnet:
selvhostet variabel **Archivo** (wdth 62-125) i UI og brødtekst, Orbitron kun i
GITATO-mærket. Deres stadium-night-tokens (`--st-*`) må bære tekst når kontrasten er
målt ≥ 4,5:1. Chakra Petch/Barlow gælder for hub, spillene, MedieStudio, AddSon og
Frequency Pilot.

Letterspacing sættes i **em**, aldrig px. Spillet satte det i px, hvilket gav samme
3px-mellemrum på en 12px-label og et 180px rangbogstav.

Tal der ændrer sig på plads (score, timer, målere) skal have `.gt-num` —
`font-variant-numeric: tabular-nums`, ellers hopper layoutet.

---

## D. Farve

Substrat, blæk og kanter er **fælles for alle flader**. Det er det gulv der får
addson og RL-trackeren til at holde op med at ligne fremmede firmaers software.
Et tema drejer kun fem knapper: `--gt-sig`, `--gt-sig-ink`, `--gt-sig-dk`,
`--gt-sig-2`, `--gt-sig-2-ink`.

Bevidst deling (besluttet 3/9-2026): `rts` og `frequencypilot` sætter samme fem
værdier (teal + magenta), og `game`, `mediestudio` og `rltracker` deler cyan som
`--gt-sig` og adskilles kun på `--gt-sig-2` og `--gt-r-cut`. Det er ikke en fejl;
temaerne skelner ikke produkterne alene, det gør indholdet.

### Målte kontrastforhold

Målt mod `--gt-pl-3` (#232A35) — den lyseste flade nogen tekst kan lande på,
altså værst tænkelige tilfælde.

| Token | Værdi | På pl-3 | Dom |
|---|---|---|---|
| `--gt-ink-1` | #E8ECF2 | 12.18:1 | AAA |
| `--gt-ink-2` | #B9C2CF | 8.03:1 | AAA brødtekst |
| `--gt-ink-3` | #909AA9 | 5.08:1 | AA |
| `--gt-ink-4` | #6B7484 | 3.06:1 | **ikke tekst** |

| Signal | `-ink` på pl-3 | `--gt-ink-on-lit` PÅ pladen | hvid PÅ pladen |
|---|---|---|---|
| cyan | 10.19 | 12.48 | 1.60 ✗ |
| violet | 6.07 | 5.17 | 3.86 ✗ |
| magenta | 6.35 | 6.24 | 3.20 ✗ |
| amber | 7.62 | 8.46 | 2.36 ✗ |
| green | 9.98 | 11.56 | 1.72 ✗ |

Sidste kolonne er hele begrundelsen for emissions-modellen.

**Verificeret i galleriet (2/9-2026, alle 10 temaer):** 69 tekstelementer + placeholder
kontrolleret, **0 aktiverede under AA**, laveste målte forhold **5.08:1** — nøjagtig det
tal `ink-3` lover. Placeholder måler 7.08:1 (ink-3 på void; med ink-4 var den 4.27:1).
Deaktiveret `.gt-btn` måler 6.25:1 (ink-3 på pl-1; med `opacity:.45` var den 3.72:1).
Deaktiverede kontroller er undtaget i WCAG 1.4.3, men systemet holder dem over 4.5 alligevel.

### Afledte tokens — var()-fælden

En custom property hvis værdi indeholder `var()` substitueres dér hvor den **deklareres**,
ikke hvor den bruges. Derfor ligger alle afledte tokens (`--gt-bloom-*`, `--gt-focus-ring`,
`--gt-hz-*`) i én blok på `:root, [data-gt], [class*="gt-"], [style*="--gt-sig"]`, og
fokusringen er desuden skrevet ud på brugsstedet (`:focus-visible`-reglerne sidst i filen).
Konsekvens: en flade der sætter `--gt-focus-ink`/`--gt-focus-gap` på en kontrol får nu den
ring den beder om. Invertér den ikke: hvidt bånd mod cyan-fyld måler 1.60:1 og
`#07090C` mod siden 1.02:1. Oplyste flader (`.gt-plate`, `.gt-chip--lit`) har deres egen
ring INDE i kanten, fordi `clip-path` klipper pladens ydre `box-shadow`.

---

### Mønsterkanalen — hvornår farve ikke er nok

Sortér signalfarverne så hver ny serie ligger så langt fra de øvrige i luminans som
muligt. Den mindste indbyrdes afstand bliver da:

| Antal serier | Mindste ΔL | Dom |
|---|---|---|
| 2 | 0.385 | farve alene er rigeligt |
| 3 | 0.173 | farve alene er nok |
| **4+** | **0.048** | **mønsterkanal påkrævet** |

Under ΔL 0.05 er parret ikke til at skelne for en deuteranop. Det er nøjagtig den
tilstand RL-trackeren shippede i: `#58A6FF` blå mod `#F0883E` orange på 4px høje
målere — det værst tænkelige par for rød-grøn farveblindhed.

Fra fire serier og op **skal** `.gt-pat-a/-b/-c/-d` (skravering, prikker, gitter)
lægges oven på fyldet. Det er ikke en anbefaling.

## E. Overflader og dybde

**Målt, ikke antaget:** på et nær-sort substrat gør en drop-shadow næsten ingenting.
Renderet side om side var `--gt-e-0` til `--gt-e-4` som skygger alene *umulige at
skelne*. På mørkt UI bæres dybde af **fladens lyshed** og af **top-lippen**; skyggen
sælger kun kontaktkanten.

Brug derfor `.gt-elev-0` … `.gt-elev-4` og `.gt-sunken`, som hæver fyldet med.
Skygge-tokens alene er én ingrediens, ikke opskriften.

Radius-skalaen er bevidst stram (2/3/5/8px). Porteføljen havde 13 forskellige
værdier på Hector alene. Indre radius = ydre minus padding, så indbyggede hjørner
forbliver koncentriske.

---

## F. Tekstur

Filmkorn bagt til en data-URI: én afkodning, derefter fliselægning.

**Kornet SKAL have en blend-mode.** Første forsøg malede det direkte på uden en, og
resultatet var fladt gråt hen over hele siden — sortet forsvandt. Rettet til
`mix-blend-mode: overlay` ved `opacity: .10`, med vignetten som separat, ikke-blendet
lag så den mørkner i stedet for at sløre.

Brug `<div class="gt-vignette"></div><div class="gt-grain"></div>` som første to
elementer i `<body>`. **Ikke** på spilflader — se afsnit H.

---

## G. Bevægelse

Rigtige kurver, ikke bart `ease`. Fire varigheder (`--gt-d-1` … `--gt-d-4`), fire
kurver. `prefers-reduced-motion` fjerner transforms helt frem for at forkorte dem —
et 1ms løft læses stadig som et hop.

---

## H. Adoptionsguide pr. flade

**Fælles for alle:** kopiér `ds.css` + `icons.svg` ind i flademappen, link `ds.css`
først, sæt `data-gt` på `<body>`, erstat emoji med `<svg class="gi"><use
href="icons.svg#gi-NAVN"></use></svg>`, tilføj `:focus-visible` overalt, fjern
duplikerede `:root`-blokke.

**Rør aldrig:** funktionel JS, de selektorer/id'er/`data-`attributter JS binder til,
sprogvalget, filnavne, links, manifest- og sw-referencer, gameplay-logik.

| Flade | Særligt |
|---|---|
| **hub** | Vigtigste flade. Kortene er i dag rene billedcontainere; giv dem behandling. `.card.soon` bruger `opacity:.65`, som dæmper tekst og ramme samtidig — brug `.gt-card--soon`. `PROJECTS`-arrayet styrer kortene; behold dets form. |
| **www + 404** | 550 bytes hver. Inline CSS er fint — de har ingen `ds.css` ved siden af. |
| **addson** | Børnevenlig tone SKAL bevares. `data-gt="addson"` blødgør allerede radier og tracking. De 8 inline chip-hex skal ud. **Logoet klippes i toppen** — ret det. |
| **frequencypilot** | Behold canvas-tunnelen, men respektér `prefers-reduced-motion` og stop rAF-loopet når hero er ude af syne. De fire korttyper ser identiske ud. Stat-barerne har inline px-bredder uden skala. |
| **beatsurfer** | Porteføljens bedste flade — vær varsom. `#scanlines` ligger over *alt* inkl. tekst med `mix-blend-mode:multiply` og sænker global kontrast; gate den. **Tilføj `ds.css` til `sw.js` PRECACHE.** |
| **game** | Den gamle kilde `OneDrive\Skrivebord\gitato\renderer` **findes ikke længere** — disse filer er nu kilden. `app.bundle.js` er bygget, rør den ikke. **Bekræftet fejl:** `#tribute-credit` er `position:absolute; bottom:20px` og lander oven på menuknapperne når skærmen er højere end viewporten. Fonten hentes aldrig. |
| **rts** | Knapper har forskellig højde og teksten brydes. `CODE`-inputtet ligner en knap — brug `.gt-input` (nedsænket). Panelkanten `#1c1c40` på `#050510` er ~1.2:1 og reelt usynlig. Tre knapsystemer skal blive til ét. |
| **mediestudio** | Dansk UI — **sproget må ikke ændres.** Alle toolbar-ikoner er emoji og er appens mest mættede elementer. `--acc #4f8cff` er stock VSCode-blå. Fokus findes kun på tekstfelter; 12× `outline:none`. |
| **sprunkiverse** | `#bonus-banner` er en 5-stop regnbue med uendelig slide — erstat. Near-dublet af hub'ens tokens med utilsigtet drift. Rør ikke `js/`. |
| **hector** | Har porteføljens bedste fokus-tilstand og et rigtigt 6-temasystem — byg videre på det. `font-weight: 650/750/780` findes ikke i systemfonten, så `.tab` og `.tab.on` rendrer **identisk**; hierarkiet i CSS'en findes ikke på skærmen. Bevar alle 6 temaer inkl. light. |
| **rltracker** | Board på afstand — læsbarhed før finesse. Paletten er GitHub Primer. **0 transitions i hele filen**, i en tracker hvor tal opdateres kontinuerligt. Rør ikke polling/parse/API. **Typografi-undtagelse (afsnit C):** trackeren, rapporterne, portalen (collect.gitato.net), RL Rank Overlay, RL Roll og tracker-landingen bærer Archivo/NITRO, ikke Chakra Petch/Barlow; deler cyan-signal med game/mediestudio (afsnit D). |

---

## I. Service worker

PWA-reglen gælder alle gitato-sites: **bump `VERSION` ved hver deploy.**

| Flade | Målt 2/9-2026 |
|---|---|
| hub | `gitato-v17` |
| game (+rts) | `gitato-game-v19` |
| beatsurfer | `beatsurfer-v6` |
| mediestudio | `mediestudio-v2` |
| sprunkiverse | `sprunkiverse-v7` |

Alle fem precacher allerede `ds.css` og `icons.svg`. Tallene ovenfor er et øjebliksbillede;
den gældende værdi står i hver flades `sw.js`.
