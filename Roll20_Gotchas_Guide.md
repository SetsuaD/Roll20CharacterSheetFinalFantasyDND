# Roll20 Character Sheet Development: Gotchas, Quirks, and Best Practices

Developing custom character sheets in Roll20 requires navigating a highly restricted environment. Roll20 does not use standard DOM manipulation; it uses a proprietary system of HTML attributes (`attr_`), Action Buttons (`act_`), and Sheet Workers (sandboxed JavaScript) that interact with a hidden backend database.

This document serves as a living guide of hard-learned rules, silent failure modes, and "gotchas" discovered during development.

---

## 1. The Sheet Worker Sandbox

Sheet Workers are restricted JavaScript blocks that run in the background. They do not have access to `window`, `document`, or standard DOM APIs.

### 🚫 The "Spread Operator" Crash
**Gotcha:** Modern ES6 syntax like the spread operator (`...array`) can cause the Roll20 pre-compiler to silently crash when loading the sheet. 
**Symptom:** Your sheet will load visually, but **none of the buttons will work** and **none of the `on("change:")` events will fire**. No errors will appear in the developer console.
**Rule:** Use `.concat()` instead of `...` when merging arrays for `getAttrs` lists or creating arrays dynamically. Stick to older ES5/ES6 syntax for absolute safety.

### 🚫 The Asynchronous Race Condition
**Gotcha:** `getAttrs` and `setAttrs` are asynchronous. If you put `getAttrs` inside a loop (like iterating through ability scores), the callbacks will fire out of order, or you will accidentally overlap `setAttrs` calls, causing Roll20's backend to drop database updates.
**Rule:** Group everything into a single `getAttrs` call, do all math synchronously, and push all updates in a single `setAttrs` call at the very end of the function.

---

## 2. Attribute Naming & `setAttrs`

The `setAttrs()` function is how Sheet Workers push data to the visible HTML and the backend database.

### 🚫 The "Uppercase Letter" Silent Abort
**Gotcha:** Roll20 strictly requires all attribute names to be completely lowercase. If you call `setAttrs()` and pass an object where *even one key* contains an uppercase letter (e.g., `repeating_weapons_-W000...`), Roll20 will **silently abort the entire `setAttrs` operation**.
**Symptom:** Nothing updates on the sheet. No errors in the console. 
**Rule:** Ensure dynamically generated row IDs or attribute strings are always converted to `.toLowerCase()`.

### 🚫 Modifying `<span name="attr_...">`
**Gotcha:** You cannot use `setAttrs()` to update a `<span>` directly unless there is a corresponding `<input type="hidden">` with the same name, or the span relies entirely on auto-calculated values from other inputs.
**Rule:** If a Sheet Worker needs to update a value that is only displayed as text, always use `<input type="hidden" name="attr_my_value">` alongside the `<span name="attr_my_value"></span>`.

---

## 3. Repeating Sections

Repeating sections are lists of items (weapons, inventory, etc.) defined by `<fieldset class="repeating_something">`.

### 🚫 The Missing `generateRowID` Caveat
**Gotcha:** If you are auto-populating a repeating section via a Sheet Worker, you MUST generate a unique, Roll20-compliant row ID for every new row.
**Rule:** Use the standard Roll20 `generateRowID()` function (which must be manually defined in your script since it's not natively exposed in the sandbox) to create IDs like `-w000000000000000001`.

---

## 4. User Interface and Inputs

The way Roll20 handles user input in the browser has a few notorious quirks.

### 🚫 The Textarea "OnBlur" Bug
**Gotcha:** Roll20 updates backend attributes via an `onchange` event listener on HTML elements. For `<textarea>` and text `<input>` fields, `onchange` ONLY fires when the user clicks outside the box (losing focus / `onblur`). 
**Symptom:** If a user pastes JSON into an import box and immediately clicks an "Import" Action Button without clicking away first, the Sheet Worker's `getAttrs` will see the box as **empty**.
**Rule:** Always program error reporting into Action Buttons (e.g., "ERROR: No data found") to warn the user that they need to click out of the box first.

### 🚫 Action Button Naming
**Gotcha:** Action Buttons must use the `type="action"` attribute, and their `name` must begin with `act_`.
**Rule:** `<button type="action" name="act_do_something">` matches the Sheet Worker event `on("clicked:do_something")`. Do not include `act_` in the `on()` listener string.

---

## 5. Development Workflow

### 🚫 "It Worked Before!"
**Gotcha:** Because of the sandbox and how attributes are cached in the browser, a broken Sheet Worker might appear to work for a few minutes while Roll20 serves cached JavaScript, or vice versa.
**Rule:** Whenever you change the HTML or Sheet Worker code, completely close the character sheet in the game, refresh the Roll20 tab, and reopen the sheet to guarantee the new code is executing. 

*Remember: In Roll20, silent failure is the default behavior. If a sheet stops calculating, check your JS syntax, check for uppercase letters in `setAttrs`, and verify your HTML `name="attr_..."` bindings.*
