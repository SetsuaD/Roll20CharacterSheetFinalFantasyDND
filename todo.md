# Roll20 Character Sheets Final Fantasy DND Todo

- [x] 2026-05-08 22:51:00 - Clone empty repository and initialize basic README.
- [x] 2026-05-08 22:51:00 - Wait for user to attach character sheet files.
- [ ] 2026-05-08 22:51:00 - Update README with detailed "Purpose" and "How it works" after reviewing files.
- [x] 2026-05-08 22:57:00 - Add saving throw proficiency checkboxes + computed save values + roll buttons to each ability score block (STR/DEX/CON/INT/WIS/CHA).
- [x] 2026-05-08 22:57:00 - Wire saving throw sheet workers: recalculate on ability change, PB change, and proficiency toggle.
- [x] 2026-05-08 22:57:00 - Make passive perception editable with up/down arrow action buttons (removed readonly auto-calc).
- [x] 2026-05-08 22:57:00 - Add CSS for .sheet-save-row, .sheet-save-val, .sheet-pp-controls, .sheet-pp-arrow styling.
- [x] 2026-05-08 23:03:00 - Create Roll20_Development_Reference.md reference guide from wiki.roll20.net docs.
- [x] 2026-05-08 23:03:00 - Add mastery/expertise (second gold checkbox) to all 18 skill rows with sheet worker calc (mod + PB + PB if mastery).
- [x] 2026-05-08 23:03:00 - Add mastery change handlers and update PB/ability change handlers to include mastery in recalculations.
- [x] 2026-05-08 23:03:00 - Auto-calculate materia_slots_max from repeating_armor and repeating_weapons materia field sums.
- [x] 2026-05-08 23:03:00 - Generate FF-themed background watermark image and apply as CSS ::before overlay (opacity 0.06).
- [x] 2026-05-08 23:03:00 - Fix spellcasting initialization on sheet:opened (modifier, save DC, attack bonus now computed on open).
- [x] 2026-05-08 23:03:00 - Rename armor/weapon materia labels to 'M. Slots' and make materia_slots_max readonly.
- [x] 2026-05-09 01:17:00 - Implement a JSON character importer UI and sheet worker to boilerplate_sheet.html
- [x] 2026-05-09 01:17:00 - Generate Cecil Highwind JSON export for Roll20 import
- [x] 2026-05-09 01:17:00 - Generate Lenna Highwind JSON export for Roll20 import
- [x] 2026-05-09 01:17:00 - Generate Locke Valeclaw JSON export for Roll20 import

- [x] 2026-05-24 16:36:00 - Revamp repeating weapons section to calculate atk and dmg modifiers dynamically.
- [x] 2026-05-24 16:36:00 - Implement class starting equipment auto-population sheet workers on class change.
- [x] 2026-05-24 16:36:00 - Guide initiative rolling in Roll20 (add tooltips, character names to templates, warning label).
- [x] 2026-05-24 16:36:00 - Fix GP input box size in CSS to support large gold amounts.
- [x] 2026-05-24 16:36:00 - Update Cecil Highwind JSON with standard starting equipment and dynamic weapons config.
- [x] 2026-05-24 16:36:00 - Update Lenna Highwind JSON with standard starting equipment and dynamic weapons config.
- [x] 2026-05-24 16:36:00 - Update Locke Valeclaw JSON with standard starting equipment and dynamic weapons config.
- [x] 2026-05-24 16:36:00 - Update Terra Figaro JSON with standard starting equipment and dynamic weapons config.

- [x] 2026-05-24 16:51:00 - Refine Cleric, Rogue, and Wizard starting equipment sheet workers to match new specifications.
- [x] 2026-05-24 16:51:00 - Update Lenna Highwind JSON export with Scale Mail, Holy Symbol, and 1 Potion.
- [x] 2026-05-24 16:51:00 - Update Locke Valeclaw JSON export with Leather Armor, 2 Daggers (DEX), 50 ft rope in Burglar's Pack, and 1 Potion.
- [x] 2026-05-24 16:51:00 - Update Terra Figaro JSON export with Quarterstaff, Dagger, 1 Potion, and remove Mage Robes.

- [x] 2026-05-24 16:57:00 - BUG [CRITICAL]: Fix undefined i5/i6 variables in class equipment auto-population worker (Cleric/Rogue/Wizard branches). Add generateRowID() calls. COMPLETE 2026-05-24 16:59:00
- [x] 2026-05-24 16:57:00 - BUG [CRITICAL]: JSON import does not trigger recalculations — imported characters show stale/zero computed values. Add recalc chain after setAttrs(). COMPLETE 2026-05-24 16:59:00
- [x] 2026-05-24 16:57:00 - BUG [MEDIUM]: Mastery without proficiency still adds PB to skills. Should require proficiency checkbox first. COMPLETE 2026-05-24 16:59:00
- [x] 2026-05-24 16:57:00 - BUG [MEDIUM]: Initiative is hard-overwritten on DEX change, wiping any manual bonuses (e.g., Alert feat). Add initiative_bonus field. COMPLETE 2026-05-24 16:59:00
- [x] 2026-05-24 16:57:00 - TASK: Decide correct GP values for all 4 characters (currently fabricated: Cecil 150, Lenna 120, Locke 180, Terra 90). SET TO 0 PER USER. COMPLETE 2026-05-24 16:59:33
- [ ] 2026-05-24 16:57:00 - NOTE: Background image CSS uses local path — will need CDN URL for Roll20 deployment.
- [ ] 2026-05-24 16:57:00 - NOTE: JSON files missing armor proficiency checkboxes, alignment, appearance fields (uses defaults, not a blocker).

- [x] 2026-05-24 17:07:00 - BUG [CRITICAL]: JSON import handler broke all sheet workers. Rewrote to avoid try/catch around async callbacks, use var instead of let, concat instead of spread for getAttrs field list, and separate JSON parse from setAttrs call. COMPLETE 2026-05-24 17:08:00
- [x] 2026-05-24 17:33:00 - BUG [CRITICAL]: Fix silent compile crash of Roll20 Sheet Worker caused by ES6 spread operator on line 860. Replace spread operator with standard concat syntax. COMPLETE 2026-05-24 17:34:00
- [x] 2026-05-24 17:33:00 - BUG [MEDIUM]: Refactor Dexterity change handler in abilities loop to eliminate nested getAttrs and duplicate setAttrs calls. COMPLETE 2026-05-24 17:34:00

