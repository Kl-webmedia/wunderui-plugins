---
description: Add WunderUI components to this project (e.g. /wunderui:add button badge card)
argument-hint: <component> [component ...]
---

Add these WunderUI components to the current project: $ARGUMENTS

1. If WunderUI is not set up here yet (no `.wunderui/` folder and no `styles/wunderui.css`), run the `wunderui-setup` skill first.
2. Run `npx wunderui-cli@1.0.1 add $ARGUMENTS` in the project root. It writes the source into the `@/` root and installs missing dependencies.
3. If a name is not a free component, the CLI says so — tell the user which plan includes it (Core or Pro, see https://wunderui.com/#pricing) and suggest the closest free component; use the WunderUI MCP tool `list_components` to find it.
4. Show the user the files that were added and a minimal usage example taken from the MCP tool `get_component` (real props only).
