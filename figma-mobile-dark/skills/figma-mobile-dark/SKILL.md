---
name: figma-mobile-dark
description: Turns finished desktop screens in Figma into a proper mobile version (390 px) and derives dark versions (desktop and mobile) from the light frames through variable modes instead of hand-recolouring. Also repairs broken mobile frames — cut-off text, overflow, split rows, collapsed grids, fixed heights. Use when the user asks for mobile or dark versions of Figma screens, says a mobile frame looks broken or unfinished, or changed a desktop screen whose mobile/dark copies are now stale.
---

# Figma Mobile & Dark

Result per screen: **a light mobile frame that reads like it was designed for 390 px, and dark frames for desktop and mobile that are exact clones of the light frames in the dark variable mode.** A screen is done only when the overflow check reports 0 and you have looked at a screenshot of every new frame.

Needs the Figma MCP server (`use_figma`, `get_screenshot`). If a `figma-use` skill is available, load it before the first `use_figma` call. Work in small `use_figma` steps; a failed script is rolled back completely, so read the canvas again after any error.

## 0. Read before you change anything

1. Get the file key and the frame or page from the user's link.
2. Find the colour collection that has a light and a dark mode (modes named like `Light`/`Dark`). Note its id and the dark mode id. **No such collection → stop and tell the user:** dark versions come from variables; without them the result would be hand-picked colours that drift.
3. Dump each source frame compactly (≤ 20 KB): name, size, layout mode, `layoutSizingHorizontal/Vertical`, `layoutWrap`, `clipsContent`, absolutely positioned children, and every fill/stroke that is **not** bound to a variable.
4. Screenshot the source.

## 1. Naming and placement

- `<Screen>` (desktop light), `<Screen> · Dark`, `<Screen> · Mobile`, `<Screen> · Mobile · Dark`.
- Desktop keeps its width. Mobile is **390 wide, height hug**.
- Place the new frames in a row to the right of the source (≥ 80 px gap), never on top of other frames. Replacing an older version: same position, remove the old frame only after the new one exists.

## 2. Mobile (light)

Build the mobile frame from a clone of the desktop frame, then restructure. Rules:

1. **Shell.** Sidebar → hidden; top bar = logo left, avatar or menu button right (`SPACE_BETWEEN`), height 56–64. Bottom tab bar only if the product already has one.
2. **Content.** One padded column: padding 16, vertical, gap 16. Every section `FILL` width.
3. **Header.** Title 20 px (18 px when longer than ~18 characters), one line; action bar wraps (`layoutWrap = "WRAP"`, `counterAxisSpacing = 8`). Segmented controls and filter chips wrap or scroll in their own row.
4. **KPI rows** → 2 columns (or 1 for wide values); cards keep their padding, numbers centred where the desktop centres them.
5. **Charts** keep their type and data, `FILL` width, height 180–240, axis labels ≤ 4 characters; legends move below the chart.
6. **Tables** → stacked list rows: primary text + secondary line, status badge and amount on the right; drop columns the row cannot hold and say which.
7. **Grids.** Month calendars keep 7 columns (cells ≥ 84 high, chips `FILL`, single line with ellipsis). Week/time grids → 3 days, time column 40, day columns `FILL`; scale absolute children (`x`, `width` × factor).
8. **Text.** `FILL` + `textAutoResize = "HEIGHT"` instead of fixed widths; truncate only labels that must stay on one line (`textTruncation = "ENDING"`, `maxLines = 1`).
9. **Rows that the old transform split** (an "Actions" frame below its row): move the children back into the original row, delete the empty frame.
10. **Overlays** (popover, dialog, sheet): absolute child of the screen, width 358, with a black 20 % scrim behind it; the frame grows to fit.
11. **Colours and type** only from existing variables and text styles — no new hex values, no new variables. Load the node's current fonts (`getStyledTextSegments(["fontName"])`) before editing any text.
12. **Unrepairable frame** (structure completely broken): clone an intact mobile frame of the same product and replace only the content.

## 3. Dark (desktop and mobile)

1. Clone the finished **light** frame.
2. `clone.setExplicitVariableModeForCollection(colorCollection, darkModeId)`.
3. Name and place it (rule 1). Never recolour a dark frame by hand — when the light frame changes, derive the dark frame again.
4. **Unbound colours do not flip.** List every fill/stroke in the light frame that is not bound to a variable (from step 0) and report them: they stay light in dark mode. Offer to bind each one to the matching variable; don't guess silently.
5. Paint opacity set on a bound paint can be lost when cloning (heat maps, funnels, tinted bars): copy those paints from the light frame onto the clone after step 2.

## 4. Verify (measurable, in `use_figma`)

- No descendant with `absoluteBoundingBox` right edge > frame x + 390 on mobile.
- No `FIXED` height ≥ 150 on a frame whose children don't fill it (empty boxes).
- No text node with `FIXED` width and no truncation that overflows its parent.
- Every new frame screenshotted and looked at: light mobile, dark desktop, dark mobile.

## 5. Report

Per screen: what was broken → what changed, new frame ids, the list of unbound colours, and every deliberate difference from desktop (e.g. "3 days instead of 7", "table → list, column *Owner* dropped").

## Gotchas

- A long `use_figma` call can end with "transport dropped" although it ran: read the canvas, then continue with guarded, idempotent scripts.
- `layoutSizingHorizontal/Vertical` take `FILL | HUG | FIXED` and only after the node sits in an auto-layout parent; containers use `primaryAxisSizingMode/counterAxisSizingMode = AUTO | FIXED`.
- `resize()` resets sizing to `FIXED` — set `HUG`/`FILL` again afterwards.
- `findAll` only on nodes that have it (`"findAll" in node`); guard ellipses and vectors.
- Hidden layers inside instances are skipped unless `figma.skipInvisibleInstanceChildren = false`.
- Empty auto-layout frames are 100 px tall by default — resize spacers explicitly.
