# Chronicles of the Verdant Crossroads — Translation Guide

**Status**: English (`en.json`) is the source of truth. Spanish (`es.json`) and Catalan (`ca.json`) need review.  
**How to work**: Open the JSON file for your language side by side with `en.json`. Translate/correct the values — never touch the keys.

---

## 1. Ground rules

- **Never edit keys** — only values. Keys are the left side of `"key": "value"`.
- **Keep placeholders** like `{enemy}`, `{dmg}`, `{cost}` exactly as written — the game replaces them at runtime.
- **Keep emoji and symbols** like `⚔`, `☠`, `★`, `✨`, `←` as-is unless they feel culturally wrong.
- **The `_meta.status` field**: set it to `"done"` once you've reviewed all strings.
- **Line breaks** in `stats` strings use `\n` — keep them.

---

## 2. Section by section

### `ui` — Interface chrome
Shown in headers and navigation. The `logoTitle` and `gameTitle` are the game's proper name — translate freely or keep in English if a translated name sounds awkward.

| Key | Context |
|-----|---------|
| `logoTitle` | Big title on the start screen |
| `logoSub` | Tagline below the title |
| `langLbl` | Label next to the language buttons |
| `diffLbl` | Header above the difficulty cards |
| `menu` | Back button in the game header |
| `gameTitle` | Small title shown in the game header bar |

---

### `badges` — Short labels (shown in pills/tags)
These must be **very short** — they render in small badges. Max ~8 characters.

| Key | Context |
|-----|---------|
| `easy` / `balanced` / `hard` | Difficulty badge in the game header |
| `langCode` | 2-letter language code shown in the lang badge (e.g. "EN", "ES", "CA") |

---

### `difficulty` — Start screen cards
Three cards: `easy`, `balanced`, `hard`. Each has:
- `tag` — very short label at the top of the card (same as badge, can match)
- `name` — the poetic name of the difficulty (e.g. "Pilgrim's Path")
- `desc` — one sentence describing the feel
- `stats` — three lines of mechanical stats, separated by `\n`. The numbers (HP, ATK, etc.) should stay unchanged — only translate the labels like "Rest", "Flee", "Varied enemies".

---

### `stats` — Stat labels
Short labels for the five stat boxes: HP, ATK, DEF, Gold, Level, XP.  
HP/ATK/DEF are RPG conventions — keeping them untranslated is fine and often preferred.

---

### `warn` — Welcome messages when starting a game
Three difficulties, each with `msg` (bold intro line) and `sub` (explanatory sentence).  
Tone: `easy` is welcoming, `balanced` is neutral, `hard` is ominous.

---

### `locations` — Place names and descriptions
Five locations: `crossroads`, `forest`, `cave`, `ruins`, `town`.  
Each has `name`, `desc`, and `exits` (the button labels to travel between areas).  
The town is called **Millhaven** — keep it as a proper noun.

---

### `actions` — Button labels
Shown on action buttons during exploration and combat.

| Key | Placeholders | Context |
|-----|-------------|---------|
| `restCost` | `{cost}` = gold amount | Button to rest when it costs gold |
| `flee` | `{pct}` = failure percentage | Button to flee combat |
| `potion` | `{count}` = number of potions | Button to use a potion |

---

### `combat` — In-game log messages
These appear in the scrolling combat log. Tone should feel like a classic JRPG or tabletop game.

| Key | Placeholders | Context |
|-----|-------------|---------|
| `hitEnemy` | `{enemy}`, `{dmg}` | Player hits an enemy |
| `bossAppears` | `{name}` | Boss entrance announcement |
| `enemyAppears` | `{name}`, `{hp}` | Regular enemy appears |
| `enemyAttacks` | `{enemy}`, `{dmg}` | Enemy basic attack line |
| `stunned` | `{enemy}`, `{turns}` | Enemy is stunned, skips turn |
| `victory` | `{gold}`, `{xp}` | Player wins a fight |
| `escapedFull` | — | Player flees successfully (easy mode, no penalty) |
| `escapedHit` | `{dmg}` | Player flees but takes damage |
| `fleeFailed` | `{enemy}`, `{dmg}` | Player fails to flee |
| `levelUp` | `{lvl}`, `{hp}`, `{atk}` | Level up without new ability |
| `levelUpAbility` | `{lvl}`, `{hp}`, `{atk}`, `{ability}` | Level up with new ability unlock |
| `potionHeal` | `{hp}` | Potion used |
| `cantAffordRest` | `{cost}` | Not enough gold to rest |
| `restedFully` | `{cost}` | Rested successfully, gold deducted |
| `boughtMsg` | `{item}` | Item purchased in shop |

---

### `dead` — Death screen
Shown when the player dies.

| Key | Context |
|-----|---------|
| `title` | Big red heading |
| `btn` | "Try again" button |
| `restart` | `{location}` = location name. Log message when restarting |
| `sub.easy/balanced/hard` | Flavour text matching the difficulty's tone |

---

### `shop` — Merchant items
Array of 5 item names shown in the shop. Order matters — do not reorder.

```
0: Health Potion — restores HP
1: Iron Sword — +2 ATK permanently
2: Tower Shield — +2 DEF permanently
3: Max Elixir — restores HP to maximum
4: War Tome — +3 ATK permanently
```

---

### `abilities` — Player abilities
Six abilities unlocked as the player levels up. Each has:
- `name` — shown on the ability button
- `log` — shown in the combat log when used
- `lethal` (execute only) — inserted into `log` when the enemy is low HP

| Ability | Placeholders in `log` | Notes |
|---------|----------------------|-------|
| `slash` | `{dmg}` | Single heavy hit |
| `bash` | `{dmg}` | Hits and stuns the enemy |
| `flurry` | `{hits}`, `{total}` | Multiple rapid hits |
| `rally` | `{hp}` | Self-heal |
| `execute` | `{lethal}`, `{dmg}` | Bonus damage on low-HP enemies. `{lethal}` inserts `lethal` string (or empty) |
| `meteor` | `{dmg}` | Massive AoE hit, should feel dramatic |

---

### `enemies` — Enemy names and special attack messages
Organised by `easy`/`normal`/`hard` → `forest`/`cave`/`ruins` → array of enemies.

**Critical**: the order of enemies in each array **must match** the order in `en.json`. The game links translations to game data by position index.

Each enemy entry:
- `name` — displayed name
- `special` — short message shown when the enemy uses a special attack (`null` if none)
- `phases` (bosses only) — array of phase transition messages (in order: phase 1, phase 2)

**Style note**: special attack messages are short and punchy. They appear after the enemy name, e.g.: `"Stone Golem crushes you!"`. In languages with verb-subject inversion or articles, adjust so it reads naturally after the enemy name.

---

## 3. Common issues to watch

- `{turns}` in `stunned`: some languages need singular/plural distinction ("1 turn left" vs "2 turns left"). For now, a single form is acceptable — flag it if it sounds wrong.
- Boss `name` fields include the prefix (`BOSS:` / `JEFE:` / `CAP:`). This is intentional — translate the prefix too.
- `abilities.execute.lethal` is inserted mid-sentence. Make sure it reads naturally: `"Execute {lethal}{dmg} dmg!"` with `lethal = "— LETHAL! "` produces `"Execute — LETHAL! 42 dmg!"`. Adjust spacing/punctuation in `lethal` as needed.

---

## 4. Adding a new language

1. Copy `en.json` to `[langcode].json` (e.g. `fr.json`)
2. Set `_meta.lang`, `_meta.name`, `_meta.status: "wip"`
3. Translate all values following this guide
4. In `index.html`, add the locale to `LOCALES` at the top of the `<script>` and add a button in the HTML lang row
5. Run the game and check the browser console — it will warn about any missing keys

---

*Last updated: 2026. Source language: English.*
