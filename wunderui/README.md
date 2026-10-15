# WunderUI for Claude

![WunderUI](assets/logo.png)

Build product UI with [WunderUI](https://wunderui.com) right inside Claude — React components with every state, Figma-matched tokens and dashboards that look designed, not generated.

## What you get

| | |
|---|---|
| **`wunderui-setup`** | Sets up WunderUI in a React project from scratch to the first rendered screen — detects framework, Tailwind version and existing tokens. |
| **`wunderui-screen`** | One sentence in, one finished screen out: picks components and a block layout, adds loading, empty and error states. |
| **`wunderui-theme`** | A complete brand theme from one hex colour — the eleven-step brand scale, accent, radius and font. |
| **`wunderui-token-check`** | Finds hard-coded colours, spacing and radii and maps each to the nearest WunderUI token. |

The 76 free components are MIT and live at [github.com/wunder-ui/wunderui](https://github.com/wunder-ui/wunderui). Core and Pro add charts, data tables, AI & agent components, blocks and templates — see [wunderui.com](https://wunderui.com/#pricing).

## Install

In Claude Code:

```
/plugin marketplace add Kl-webmedia/wunderui-plugins
/plugin install wunderui@wunderui
```

Then ask: *"Set up WunderUI in this project"* or *"Build a settings screen with WunderUI"*.

Works in any React project with Tailwind v4. To add components, ask Claude to build the screen; the components come from the WunderUI library in your project.

## License

MIT © WunderUI
