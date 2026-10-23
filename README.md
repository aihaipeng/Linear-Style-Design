# Linear Style Design

<p align="center">English · <a href="./README.zh-CN.md">简体中文</a></p>

An [Agent Skill](https://agentskills.io) that brings the Linear design language — near-neutral surfaces, restrained typography, stable density, fast and predictable interaction — to **any frontend stack**: web, SSR, native apps, and design tools.

## What you get

- **A quick-reference of defaults** — colors, type scale, spacing, control heights, radii, motion timing, contrast floors — directly in [`SKILL.md`](linear-style-design/SKILL.md)
- **9 focused reference files**, loaded on demand: color, typography, layout, components, motion + accessibility, implementation, details, review checklist, principles & evidence
- **A drop-in CSS token sheet** ([`assets/linear-tokens.css`](linear-style-design/assets/linear-tokens.css)) with light/dark themes and zero build step
- **A review workflow**: graded anti-pattern table (Fail / Needs reason), render-critique passes with a micro-details sweep, five review questions, Definition of Done, and a verdict-capped report format for auditing existing interfaces

## Install

Copy the [`linear-style-design/`](linear-style-design) folder into your agent's skills directory — for Claude Code, `~/.claude/skills/` (personal) or `.claude/skills/` inside a repo (shared with your team); on Claude.ai, upload it as a custom skill. The folder follows the open [Agent Skills specification](https://agentskills.io/specification) and works with GitHub Copilot, Cursor, Codex, Gemini CLI, OpenCode, and others.

## Use it

| Intent | Example prompt |
|---|---|
| Build | "Create a Linear-style settings page in React, using our brand color instead of indigo." |
| Rework | "Make this dashboard feel more like Linear — keep our fonts and data." |
| Audit | "Review this page against the Linear design language and list real problems." |

## Examples

Two single-file CI/CD pipeline monitors generated from the same prompt with different models:

| File | Model |
|---|---|
| [`examples/index.html`](examples/index.html) | glm-5.3-flash |
| [`examples/pipeline-monitor.html`](examples/pipeline-monitor.html) | deepseek-4.1-flash |

The prompt:

```text
Generate a complete, runnable web page as a single HTML file that works by
simply opening it in a browser. No frameworks, no build tools.

Subject: A CI/CD pipeline monitor — an internal workbench where an engineering
team watches the build and deploy status of multiple services. Builds run,
queue, succeed, and fail; people need to scan the whole system at a glance,
locate problems, and take action.

Make full use of the linear-style-design skill. Everything else — naming,
structure, layout, colors, data, interactions — is your call.
```

## Repository layout

```
├── LICENSE                  	# MIT
├── README.md                	# English README
├── README.zh-CN.md          	# Simplified Chinese README
├── examples/
│   ├── index.html            	# example: pipeline monitor (glm-5.3-flash)
│   └── pipeline-monitor.html 	# example: pipeline monitor (deepseek-4.1-flash)
└── linear-style-design/    	# the skill — copy this folder
    ├── SKILL.md             	# entry point: DNA, defaults, routing
    ├── references/          	# 9 topic files, loaded on demand
    └── assets/
        └── linear-tokens.css
```

## The style in 30 seconds

| Dimension | Rule of thumb |
|---|---|
| Attention | Content before app shell; navigation never glows |
| Color | Near-neutral base + one brand accent family; semantic and data colors stay separate |
| Structure | Grouping and whitespace before dividers; not everything is a card |
| Density | Chosen by input method and task — compact desktop ≠ shrunk mobile |
| Feedback | Fast local response, unambiguous states, recoverable failures |

Defaults vary by context: **productivity UI** (compact, keyboard-first), **marketing pages** (typographic discipline, real product imagery), **docs / settings / mobile** (readability first). See "Three context types" in [`SKILL.md`](linear-style-design/SKILL.md).

## License

[MIT](./LICENSE)
