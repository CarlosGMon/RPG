# Chronicles of the Verdant Crossroads

> *Choose your fate, hero. The world awaits.*

A browser-based RPG adventure built entirely with vanilla HTML, CSS, and JavaScript — no dependencies, no build tools, just files. Pick a difficulty, explore five locations, fight escalating enemies, level up, unlock abilities, and face legendary bosses.

---

## Play it

**[→ Play on GitHub Pages](https://litools-lab.github.io/verdant-crossroads/)**

---

## Features

- **Three difficulty modes** — Pilgrim's Path (easy), Adventurer's Route (balanced), Condemned's Road (hard) — each with tuned enemy stats, gold costs, and escape mechanics
- **Five explorable locations** — Verdant Crossroads, Dark Forest, Dripping Cave, Ancient Ruins, and Millhaven Town
- **Six unlockable abilities** — Power Slash, Shield Bash, Flurry, Rally, Execute, and Meteor, unlocked progressively as you level up
- **Dynamic enemy roster** — enemies appear and disappear based on your level, announced in the combat log
- **Phased boss fights** — bosses shift stats mid-battle as their HP drops
- **Persistent shop** — spend gold on potions, permanent ATK/DEF upgrades, and Max HP increases
- **Full i18n support** — English, Spanish, and Catalan, switchable at any time without reloading
- **Zero dependencies** — one HTML file plus JSON data files; runs from a local folder or any static host

---

## Project structure

```
verdant-crossroads/
├── index.html        # Game engine + all styles and scripts
├── en.json           # English strings (source of truth)
├── es.json           # Spanish strings
├── ca.json           # Catalan strings
├── enemies.json      # Enemy stats, spawn rules, boss phases
├── shop.json         # Shop inventory, effects, costs by difficulty
├── favicon.png       # Browser tab icon
├── logo.png          # Your logo (for the footer)
├── TRANSLATIONS.md   # Guide for adding or editing translations
└── README.md         # This file
```

---

## Running locally

No server needed for most browsers, but if you hit CORS issues with the JSON fetches:

```bash
# Python 3
python -m http.server 8000

# Node.js (npx)
npx serve .
```

Then open `http://localhost:8000` in your browser.

---

## Game mechanics

### Stats

| Stat | Description |
|------|-------------|
| HP | Current / maximum hit points |
| ATK | Base attack power (damage before enemy DEF) |
| DEF | Reduces all incoming damage |
| Gold | Currency for resting and shopping |
| XP | Experience toward the next level |

### Levelling

Each level-up grants +HP, +ATK, and occasionally a new ability. The XP threshold scales with level. Enemy availability also changes — the log announces which enemies appear or vanish in each area.

### Combat flow

1. Player attacks (or uses an ability / potion / flee)
2. If the enemy survives, it attacks back
3. Some enemies have **special attacks** (AoE, life steal, debuffs, self-heal) that trigger at a set probability
4. Bosses enter **phases** at HP thresholds, gaining ATK, DEF, or higher special frequency

### Abilities

| Ability | Unlock level | Effect |
|---------|-------------|--------|
| Power Slash | 1 | Heavy single hit (1.5× ATK) |
| Shield Bash | 2 | Moderate hit + stuns enemy for 1–2 turns |
| Flurry | 3 | 3–4 rapid hits |
| Rally | 4 | Self-heal (25–30% max HP) |
| Execute | 5 | Bonus damage; lethal multiplier on low-HP enemies |
| Meteor | 6 | Massive AoE strike (2.8× ATK + variance) |

All abilities have a cooldown (shown in brackets on the button).

### Difficulty comparison

| | Easy | Balanced | Hard |
|-|------|----------|------|
| Starting HP | 30 | 22 | 22 |
| Starting ATK | 6 | 5 | 5 |
| Starting DEF | 2 | 1 | 1 |
| Rest cost | Free | 6g | 6g |
| Flee fail % | 40% | 60% | 60% |
| Flee penalty | None | Half enemy ATK | Half enemy ATK |
| Potion heal | 15 HP | 18 HP | 18 HP |

---

## Data files

Game content lives in JSON files, separate from the engine:

- **`enemies.json`** — all enemy pools, stats, spawn level ranges, weights, rewards, special attack configs, and boss phase transitions. Edit values freely; never reorder entries (positions map to translation strings).
- **`shop.json`** — five shop items with effects (`addPotion`, `permAtk`, `permDef`, `permMaxHp`) and costs per difficulty.
- **`en.json` / `es.json` / `ca.json`** — all display strings, enemy names, ability descriptions, and UI text.

---

## Adding a language

1. Copy `en.json` to `[langcode].json` (e.g. `fr.json`)
2. Set `_meta.lang`, `_meta.name`, `_meta.status: "wip"`
3. Translate all values (never the keys)
4. In `index.html`, add the language code to the `LANGS` array and a `<button>` to the lang row
5. Check the browser console — the validator will list any missing keys

Full guide in [TRANSLATIONS.md](TRANSLATIONS.md).

---

## Extending the game

**New enemy** — add an entry to `enemies.json` in the correct difficulty/area array, then add a matching entry (same position) in each language JSON under `enemies.[difficulty].[area]`.

**New shop item** — add to `shop.json` items array (choose an `effect` type from the existing set), then add the display name to `shop.items` in each language JSON.

**New ability** — add to the `ABILITIES` array in `index.html` (assign an `unlockLvl`, `cd`, and `use` function), then add `name` and `log` keys to `abilities` in each language JSON.

**New location** — add a key to `locations` in each language JSON and wire up `exits` between locations. Enemy pools are keyed by location name in `enemies.json`.

---

## License

MIT — do whatever you want with it. Attribution appreciated but not required.

---

*Built with care, no frameworks harmed.*
