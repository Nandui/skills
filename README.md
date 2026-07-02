# skills

A personal collection of [Claude skills](https://docs.claude.com/en/docs/claude-code/skills), organized by category.

## Installing a skill

Point any Claude agent at this repo and ask it to install one, e.g.:

> Install the `outfit-flatlay-director` skill from https://github.com/Nandui/skills

Or do it manually:

- **Claude Code:** copy the skill folder — e.g. `outfits-processing/outfit-flatlay-director/` — into your skills directory (`~/.claude/skills/` for user-wide, or a project's `.claude/skills/`).
- **Claude desktop/web Skills UI:** zip that skill folder into a `.skill` file first, then upload it (ask any agent to "package this skill folder").

## Categories

### outfits-processing

| Skill | What it does |
| --- | --- |
| [`outfit-flatlay-director`](outfits-processing/outfit-flatlay-director/) | Turns a photo of a worn outfit into a detailed **Nano Banana Pro** (Gemini 3 Pro Image) prompt that recreates the clothing and footwear as a clean overhead **knolling flat lay**. Attach the original photo as the reference image when generating. |

### instagram

| Skill | What it does |
| --- | --- |
| [`outdoor-amateur-carousel-director`](instagram/outdoor-amateur-carousel-director/) | Builds prompt sets for a locked fictional persona in a swapped outfit, shot as an outdoor amateur/phone-photoshoot **carousel** for Nano Banana / Banana Pro. Two-phase: lock face + body on a base frame, then rebuild the rest of the set from the approved frame. |
| [`instagram-post-optimizer`](instagram/instagram-post-optimizer/) | Analyzes a photo or carousel and writes the full post package — a **real-girl caption** (with variants), on-image hook, tiered hashtags, and posting tips — tuned to the creator's persona and goal. Built around sounding like a real person, not a brand. |

### web-design

| Skill | What it does |
| --- | --- |
| [`web-ui-design`](web-design/web-ui-design/) | Designs, builds, or redesigns **modern web app UIs** (Tailwind, Base UI, shadcn/ui, or plain CSS) with real UI/UX grounding — from a single component to a full project. Interviews you before designing from scratch, audits existing code before redesigning, and covers responsive desktop/tablet/mobile behavior, states, and accessibility. |

Each category holds one folder per skill, named after the skill, containing that skill's `SKILL.md` (plus any bundled resources). Nothing else lives at a category's root.
