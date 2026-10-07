<p align="center"><img src="assets/logo.png" width="96" height="96" alt="WunderUI"></p>

# Figma Mobile & Dark

**Mobile and dark versions of your Figma screens, built the way a designer would.** Give Claude a link to a finished desktop screen and get:

- a **mobile frame (390 px)** that is restructured for the phone, not squeezed: one padded column, wrapped action bars, KPI rows in two columns, tables turned into list rows, charts at full width, overlays as sheets;
- **dark frames** for desktop and mobile, cloned from the light frames and switched to the dark mode of your colour variables, so they stay correct when the light frame changes;
- a **repair pass** for mobile frames that an automatic transform broke: cut-off text, overflow beyond 390 px, rows split into "Actions" frames, grids collapsed into lists, empty fixed-height boxes;
- a **report**: what was broken, what changed, which colours are not bound to variables and will not flip in dark mode.

Every frame is checked by measurement (nothing wider than 390 px, no empty fixed boxes, no overflowing text) and by screenshot before it counts as done.

## Requirements

- The Figma MCP server connected in Claude (it provides `use_figma` and `get_screenshot`).
- A colour variable collection with a light and a dark mode. Without it there is no reliable dark version; the plugin tells you instead of guessing colours.

## Use

```text
Make mobile and dark versions of https://www.figma.com/design/<file>/<name>?node-id=12-345
```

```text
The mobile frame "Billing · Mobile" is broken — fix it and redo the dark copies.
```

Claude Code picks the `figma-mobile-dark` agent on its own for requests like these; in other Claude apps the `figma-mobile-dark` skill does the same work.

## What's inside

| | |
| --- | --- |
| `skills/figma-mobile-dark` | the procedure: read, mobile rules, dark derivation, checks, report, Figma API gotchas |
| `agents/figma-mobile-dark` | runs the skill screen by screen in Claude Code |

## About

Made by [WunderUI](https://wunderui.com), the design system with a React library and a matching Figma file, where this workflow keeps 300+ screens in light, dark and mobile in sync. MIT licence.
