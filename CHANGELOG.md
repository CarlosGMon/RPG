# Changelog

All notable changes to *Chronicles of the Verdant Crossroads* are listed here.  
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

---

## [Unreleased]

### Planned
- French language support
- Sound effects (Web Audio API, no external assets)
- Save/load via `localStorage`

---

## [1.1.0] — 2026

### Added
- External data files: `enemies.json` and `shop.json` — game content is now fully separated from the engine
- Shop item descriptions now show effect labels (e.g. "+2 ATK") pulled from `shop.json` and the active language file
- Enemy name lookup now uses `_nameEn` matching instead of positional index, making the arrays more robust to edits
- Catalan (`ca.json`) language added
- `TRANSLATIONS.md` — full guide for translators and contributors
- i18n validator runs at startup and logs missing keys per language to the browser console

### Changed
- `_elookup()` now resolves translation by cross-referencing English names against the active locale, rather than relying on array position
- Shop `effectValue` for `addPotion` is now `null` (the heal amount comes from `CONFIGS.potionHeal`)

### Fixed
- Boss phase messages now use the correct locale string via `ephase()`
- Flee success on Easy mode no longer applies the damage penalty

---

## [1.0.0] — 2026

### Added
- Initial release
- Three difficulty modes: Easy, Balanced, Hard
- Five locations: Verdant Crossroads, Dark Forest, Dripping Cave, Ancient Ruins, Millhaven Town
- Six player abilities unlocked progressively on level-up
- Phased boss fights (stat boosts at HP thresholds)
- In-game shop with permanent upgrades and potions
- English and Spanish localisation
- Responsive single-file design with no external dependencies
