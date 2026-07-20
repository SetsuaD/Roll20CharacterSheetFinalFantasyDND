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
- [x] 2026-05-24 17:42:00 - BUG [CRITICAL]: Fix JSON files using uppercase row IDs (-W) which silently crashes setAttrs. Lowercased to -w. COMPLETE 2026-05-24 17:43:00
- [x] 2026-05-24 17:52:00 - Create Roll20_Gotchas_Guide.md to document sandbox restrictions and UI quirks (e.g., textarea blur bug, lowercase setAttrs bug). COMPLETE 2026-05-24 17:55:00
- [x] 2026-05-24 17:55:00 - TASK: Reorganize sheet layout (Inventory to Column 1, Class Features/Species Traits/Feats to Column 3) to prevent middle column bloat. COMPLETE 2026-05-24 17:56:00
- [x] 2026-05-24 17:58:00 - TASK: Add custom inline Action buttons (Trash Cans) to repeating Inventory, Weapons, and Armor rows using new removeRepeatingRow() API. COMPLETE 2026-05-24 17:59:00
- [x] 2026-05-25 03:39:00 - TASK: Implement dynamic Cantrip dropdown selector that automatically populates repeating row with D&D 2024 spell descriptions and auto-scaling roll buttons (Fire Bolt, Mind Sliver, Toll the Dead). COMPLETE 2026-05-25 03:39:00
- [x] 2026-05-25 03:39:00 - TASK: Update terra_figaro.json and cecil_highwind.json with D&D 2024 Shield and Magic Missile materia. COMPLETE 2026-05-25 03:39:00
- [x] 2026-05-25 04:09:00 - TASK: Add Sneak Attack to Cantrip dropdown and update Locke's JSON to include it. COMPLETE 2026-05-25 04:10:00
- [x] 2026-05-25 04:35:00 - TASK: Add Protect (Shield of Faith) and Healing (Healing Word) Materia to Lenna's JSON. COMPLETE 2026-05-25 04:36:00
- [x] 2026-05-25 04:45:00 - TASK: Add override fields for spell_save_dc_bonus and spell_attack_bonus_bonus to boilerplate HTML, then update Lenna's JSON to give her a 16 Save DC (+4 bonus). COMPLETE 2026-05-25 04:46:00
- [x] 2026-06-20 19:27:00 - TASK: Distribute Citadel hallway combat loot. Added 500 XP to all party members. Distributed 30 GP evenly (7.5 GP each). Gave Pendant to Lenna, Antidote to Terra, Rations and Javelins to Cecil, and Magic Missile Materia to Locke. COMPLETE 2026-06-20 19:27:00
- [x] 2026-06-20 19:46:00 - TASK: Add Perception roll button to Passive Perception bubble and style it, and update the Perception skill roll button to use the default roll template so it posts correctly to chat. COMPLETE 2026-06-20 19:47:00
- [x] 2026-06-20 20:04:00 - TASK: Update Terra Figaro's Limit Break to Arcane Ascension with scaling rules from Level 3 to 20 in terra_figaro.json. COMPLETE 2026-06-20 20:05:00
- [x] 2026-06-20 20:08:00 - TASK: Fix repeating sections delete bug in boilerplate_sheet.css by preventing itemcontrol overlay click capture. Add automatic Human species trait handler in boilerplate_sheet.html to auto-increment stats +1 and insert "Human Traits" description in species traits field when race is "Human". COMPLETE 2026-06-20 20:09:00
- [x] 2026-06-20 20:14:00 - TASK: Replace languages textarea with structured repeating languages UI including base race languages auto-calculation ("auto_languages") and manually added languages using select dropdown + custom option + custom delete/trash button. COMPLETE 2026-06-20 20:15:00
- [x] 2026-06-20 20:25:00 - TASK: Implement automated race/species rules for Elf (+2 Dex, Perception proficiency, Elf Traits), Half-Elf (+2 Cha, Half-Elf Traits), and Catfolk (Perception + Stealth proficiency, Catfolk Traits) with reversion logic when race changes. Added hidden state-tracking inputs to boilerplate HTML. Verified correct transition behavior via Node.js test script. COMPLETE 2026-06-20 20:26:00
- [x] 2026-06-20 20:45:00 - TASK: Replace Class, Race/Species, Alignment, and Size text inputs with standard D&D 5e select dropdowns to make sheet editing faster and fully selectable. Verified sheet worker events trigger correctly when dropdown selections change. COMPLETE 2026-06-20 20:46:00

- [x] 2026-06-20 20:54:00 - TASK: Add "Save Sheet" action button to the sheet header with status message fading out via CSS transitions. COMPLETE 2026-06-20 20:55:00
- [x] 2026-06-20 20:54:00 - TASK: Fix gold input field and item weight field HTML5 step-validation bug by adding step="any". COMPLETE 2026-06-20 20:55:00
- [x] 2026-06-20 20:54:00 - TASK: Fix repeating section delete buttons by refactoring dynamic click listeners to static listeners in boilerplate_sheet.html. COMPLETE 2026-06-20 20:55:00
- [x] 2026-06-20 20:54:00 - TASK: Add default placeholder choice option to repeating languages select dropdown. COMPLETE 2026-06-20 20:55:00
- [x] 2026-06-20 21:30:00 - TASK: Prefix weapon roll templates with character_name in boilerplate_sheet.html so character names appear on attacks in chat. COMPLETE 2026-06-20 21:31:00


