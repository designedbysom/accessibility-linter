# accessibility-linter

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](./LICENSE)

A Claude skill that catches accessibility failures while you're designing and building, not after you ship.

The premise: bad a11y is a structural problem. Broken focus order means the hierarchy is wrong. An unlabelled button means the action was implied but never named. A color-only error state means state was styled, not modeled. This skill is built to catch those decisions mid-design, ask the structural question behind them, and bake the right patterns into the code from the start.

---

## What it does

- **Writes accessible code by default:** semantic HTML, ARIA labels, focus states, and proper states go in without being asked
- **Asks before building** when an interaction decision has structural consequences (keyboard model, focus management, modal behavior, live regions)
- **Surfaces the design decision behind the failure:** not a WCAG citation, but the structural question that was never answered
- **Raises intentional deviations.** When breaking a default is the right call (e.g. disabling `Escape` on a form modal), it flags the tradeoff and confirms the intent before building

It's a conversation, not a report. Direct, mid-build, the way a senior design engineer would push back on a decision mid-PR.

---

## What it covers

Semantic HTML · ARIA · Keyboard navigation · Focus management · Component states · Color contrast · Motion · Touch targets

---

## What it isn't

- A WCAG audit
- An automated linter or CI plugin
- A post-ship review tool
- A checklist to run at the end

---

## Installation

### Claude Code

Add `SKILL.md` as a skill in your Claude Code project:

```
.claude/skills/accessibility-linter/SKILL.md
```

Or add it to your user-level skills so it's available across all projects.

### Claude Projects

Upload `SKILL.md` to your project's knowledge base. The skill will activate automatically when you're designing or building components.

### Cursor / Windsurf / other AI editors

Paste the contents of `SKILL.md` into your system prompt or rules file (`.cursorrules`, `.windsurfrules`, etc.).

### Any AI with a system prompt

Copy the contents of `SKILL.md` and include it in your system prompt directly.

---

## Usage

The skill activates automatically when:

- You describe a component you're designing
- You share a Figma link or screenshot
- You paste HTML, JSX, or CSS
- You ask about keyboard behavior, focus, or component states
- You say things like "is this accessible", "how should I handle focus", "review this interaction"

You don't need to invoke it explicitly. If a component is being actively designed or built, the skill is relevant.

---

## How it behaves

**Tier 1: built in by default.** Semantic HTML, ARIA labels, focus styles, proper state attributes. These go into every component without being prompted. They're part of what "finished" means.

**Tier 2: asked before building.** Keyboard navigation models, focus management, modal vs. non-modal decisions, live region strategy. These have structural consequences that can't be patched later. The skill asks one focused question before writing code, not a list.

**Intentional deviations.** When the correct call is to break a default (disabling `Escape` on a form with unsaved state, capturing `Tab` in a rich text editor), the skill raises it, explains the tradeoff, and asks for confirmation.

---

## Author

Som
