# Put the new banners on gitato.net

## 1. Copy the images
Unzip the "Hub banners" download and put the 4 PNGs here in your gitato-deploy repo:

    gitato-deploy/hub/assets/beatsurfer-cover.png
    gitato-deploy/hub/assets/frequencypilot-cover.png
    gitato-deploy/hub/assets/addson-cover.png
    gitato-deploy/hub/assets/mediestudio-cover.png

## 2. Edit gitato-deploy/hub/index.html
In the `const PROJECTS = [` list near the bottom, find the four entries that have an
`emoji:` line (BEATSURFER, FREQUENCY-PILOT, ADDSON, MEDIESTUDIO) and swap the emoji
line for an img line:

    BEATSURFER:       replace  emoji: "🏄",   with  img: "assets/beatsurfer-cover.png",
    FREQUENCY-PILOT:  replace  emoji: "🛩️",  with  img: "assets/frequencypilot-cover.png",
    ADDSON:           replace  emoji: "🧩",   with  img: "assets/addson-cover.png",
    MEDIESTUDIO:      replace  emoji: "🎬",   with  img: "assets/mediestudio-cover.png",

Nothing else needs to change — the page already renders `img` when it's present
(that's how the GITATO and Sprunkiverse cards work).

## 3. Deploy
Commit + push gitato-deploy like you normally do. Done.
