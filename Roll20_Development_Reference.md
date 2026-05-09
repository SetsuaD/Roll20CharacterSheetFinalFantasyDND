# Roll20 Character Sheet Development Reference Guide

> Created: 2026-05-08
> Source: https://wiki.roll20.net/Browser_Developer_Tools + Building_Character_Sheets + Sheet Worker API

---

## 1. Browser Developer Tools for Roll20

### Opening Dev Tools
- **F12** or right-click → "Inspect"
- **Mac:** `Cmd + Option + C`
- **Windows/Linux:** `Ctrl + Shift + C`

### Key Uses
| Use Case | Description |
|---|---|
| Bug Reports | Capture technical details for Roll20 support requests |
| Custom Styling | Use extensions like **Stylus** to tweak Roll20 UI via CSS |
| Macro Creation | Find internal attribute names, repeating section RowIDs, inspect closed-source sheets |
| Sheet Development | Live-edit HTML/CSS, reverse engineer layouts, debug sheetworkers via console |

### Sheetworker Debugging
- Use the **Web Console** tab to monitor sheetworker activity
- Add `console.log()` statements to track variable values:
  ```javascript
  console.log("Setting strength modifier to: " + strmod);
  ```

---

## 2. HTML Rules & Restrictions

### Required Structure
- **NO** `<html>`, `<head>`, `<title>`, or `<body>` tags — Roll20 wraps your content automatically
- **NO** `<form>` tags — Roll20 wraps the sheet in its own form
- **NO** `id` attributes — IDs are global and conflict when multiple sheets are open. Use classes only
- Sheet content is sandboxed inside `.charsheet` parent class

### Attribute Naming
- All `<input>`, `<select>`, `<textarea>` elements **MUST** have `name` starting with `attr_`
  ```html
  <input type="number" name="attr_strength" value="10">
  ```
- Data will NOT be saved without the `attr_` prefix

### Blocked Elements
`<meter>`, `<progress>`, `<iframe>`, `<video>`, `<meta>`, `<input type="color">`

### Roll Buttons
```html
<!-- Simple roll -->
<button type="roll" name="roll_str" value="/roll 1d20 + @{strength_mod}">Roll STR</button>

<!-- Roll with template -->
<button type="roll" name="roll_attack" value="&amp;{template:default} {{name=Attack}} {{Roll=[[1d20+@{attack}]]}}">Attack</button>
```

### Action Buttons (for Sheet Worker triggers)
```html
<button type="action" name="act_mybutton">Click Me</button>
```
- Must start with `act_` prefix
- Triggers `on("clicked:mybutton", ...)` in sheet workers (without the `act_` prefix)

---

## 3. CSS Rules & Restrictions

### Class Prefix Rule
- **All custom CSS classes MUST be prefixed with `sheet-`** in the CSS file
- In HTML, you write the class WITHOUT the prefix — Roll20 adds it automatically
  ```css
  /* CSS file */
  .sheet-my-class { color: red; }
  ```
  ```html
  <!-- HTML file — Roll20 auto-prepends sheet- -->
  <div class="sheet-my-class">Hello</div>
  ```

### Forbidden Keywords (will break the entire sheet)
| Keyword | Notes |
|---|---|
| `eval` | Most critical — even in attribute/class names |
| `data:` | No data URIs |
| `cookie` | |
| `window` | |
| `parent` | |
| `this` | Forbidden in CSS, use carefully in JS |
| `behaviour` / `behavior` | |
| `expression` | |
| `moz-binding` | |

### External Resources
- **Cannot** link to external CSS files
- Images must be proxied by Roll20
- Font imports must comply with Roll20 security standards
- Google Fonts: use `css?family=` format (NOT `css2?family=`)

### CSS Scoping
- All CSS is scoped within `.charsheet` parent class
- Avoid conflicting with Roll20 internal classes: `.sheet-new-window`, `.sheet-row`, `.sheet-character`

---

## 4. Sheet Worker API (JavaScript)

### Script Tag
```html
<script type="text/worker">
    // All sheet worker code goes here
    // Must be a single script tag at the end of the HTML
</script>
```

### Core Functions

#### `getAttrs(attributeNames, callback)`
Asynchronously retrieves attribute values.
```javascript
getAttrs(["strength", "pb"], function(values) {
    let str = parseInt(values.strength, 10) || 10;
    let pb = parseInt(values.pb, 10) || 2;
});
```

#### `setAttrs(values, options, callback)`
Asynchronously updates attributes.
```javascript
setAttrs({
    "strength_mod": modifier,
    "athletics": modifier + pb
});

// With silent option (won't trigger change events)
setAttrs({ "hp": newHP }, { silent: true });
```

#### `getSectionIDs(sectionName, callback)`
Returns array of IDs for repeating section rows.
```javascript
getSectionIDs("repeating_weapons", function(idArray) {
    // idArray = ["-MaBcDeFgH", "-NoPqRsTuV", ...]
});
```
> Note: sectionName should NOT include the `repeating_` prefix in the function call

### Event Handlers

#### Attribute Change
```javascript
on("change:strength", function(eventInfo) {
    // eventInfo.previousValue, eventInfo.newValue, eventInfo.sourceAttribute
});
```

#### Multiple Changes
```javascript
on("change:strength change:dexterity change:pb", function() {
    // Fires on any of these changes
});
```

#### Sheet Opened
```javascript
on("sheet:opened", function() {
    // Runs when sheet is first opened — good for initialization
});
```

#### Action Button Clicked
```javascript
// HTML: <button type="action" name="act_mybutton">
on("clicked:mybutton", function() {
    // Triggered when the action button is clicked
});
```

#### Repeating Section Changes
```javascript
on("change:repeating_weapons remove:repeating_weapons", function() {
    // Fires when any weapon row is added, changed, or removed
});
```

---

## 5. Repeating Sections

### HTML Structure
```html
<fieldset class="repeating_weapons">
    <div class="sheet-repeating-row">
        <input type="text" name="attr_weapon_name" placeholder="Weapon Name">
        <input type="number" name="attr_weapon_damage" value="0">
    </div>
</fieldset>
```

### Naming Rules
- Fieldset class MUST start with `repeating_`
- **Do NOT use underscores** after `repeating_` in the section name
  - ✅ `repeating_weapons`
  - ❌ `repeating_weapon_list`

### Accessing Repeating Data in Workers
```javascript
getSectionIDs("weapons", function(idArray) {
    let fieldNames = [];
    idArray.forEach(id => {
        fieldNames.push(`repeating_weapons_${id}_weapon_name`);
        fieldNames.push(`repeating_weapons_${id}_weapon_damage`);
    });
    getAttrs(fieldNames, function(values) {
        // Process each weapon row
    });
});
```

---

## 6. Roll Templates

### Default Template
```html
<button type="roll" value="&amp;{template:default} {{name=My Roll}} {{Result=[[1d20+@{modifier}]]}}">Roll</button>
```

### Important Rules
- All attributes inside roll templates MUST use **double quotes** (`"`)
- Single quotes will cause the template to be **silently ignored**
- Use `&amp;` for `&` in HTML attribute values

---

## 7. Testing & Development

### Testing Environment
- The in-editor **Preview** panel does NOT support sheet workers
- You MUST test in an actual Roll20 campaign game
- Use **Custom Sheet Sandbox** (Pro subscribers) for best workflow

### Custom Sheet Sandbox
- Upload separate HTML, CSS, and translation files
- Configure `sheet.json` for Legacy or CSE mode
- More efficient than the standard Sheet Editor

### Local Development Workflow
1. Edit HTML/CSS locally in your editor
2. Copy-paste into Roll20 Game Settings → Custom Character Sheet
3. Open a character sheet in the game to test
4. Use F12 Console for debugging

---

## 8. Quick Reference: Common Patterns

### Auto-calculated Modifier
```javascript
const calculateModifier = (score) => Math.floor((parseInt(score, 10) || 10 - 10) / 2);
```

### Proficiency Toggle
```javascript
on("change:athletics_prof", function() {
    getAttrs(["athletics_prof", "strength_mod", "pb"], function(v) {
        let prof = parseInt(v.athletics_prof, 10) || 0;
        let mod = parseInt(v.strength_mod, 10) || 0;
        let pb = parseInt(v.pb, 10) || 2;
        setAttrs({ "athletics": mod + (prof * pb) });
    });
});
```

### Checkbox-driven CSS Toggle
```html
<input type="checkbox" name="attr_toggle" class="sheet-toggle-input" value="1">
<div class="sheet-toggle-target">I appear when checked!</div>
```
```css
.sheet-toggle-input { display: none; }
.sheet-toggle-target { display: none; }
.sheet-toggle-input:checked ~ .sheet-toggle-target { display: block; }
```

---

## 9. External References
- [Roll20 Building Character Sheets](https://wiki.roll20.net/Building_Character_Sheets)
- [Roll20 Sheet Worker Scripts](https://wiki.roll20.net/Sheet_Worker_Scripts)
- [Roll20 CSS Wizardry](https://wiki.roll20.net/CSS_Wizardry)
- [Firefox Web Dev Tools](https://developer.mozilla.org/en-US/docs/Tools)
- [Chrome DevTools](https://developer.chrome.com/docs/devtools/)
- [MDN Debugging HTML](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Debugging_HTML)
- [MDN Debugging CSS](https://developer.mozilla.org/en-US/docs/Learn/CSS/Building_blocks/Debugging_CSS)
