# skills

A personal collection of [Claude skills](https://docs.claude.com/en/docs/claude-code/skills), organized by category.

## Installing a skill

Point any Claude agent at this repo and ask it to install one, e.g.:

> Install the `outfit-flatlay-director` skill from https://github.com/Nandui/skills

Or do it manually:

- **Unpacked (for Claude Code):** copy the skill folder — e.g. `outfits-processing/outfit-flatlay-director/` — into your skills directory (`~/.claude/skills/` for user-wide, or a project's `.claude/skills/`).
- **Packaged (for the Claude desktop/web Skills UI):** upload the matching `.skill` file — e.g. `outfits-processing/outfit-flatlay-director.skill`.

## Categories

### outfits-processing

| Skill | What it does |
| --- | --- |
| [`outfit-flatlay-director`](outfits-processing/outfit-flatlay-director/) | Turns a photo of a worn outfit into a detailed **Nano Banana Pro** (Gemini 3 Pro Image) prompt that recreates the clothing and footwear as a clean overhead **knolling flat lay**. Attach the original photo as the reference image when generating. |

### instagram

| Skill | What it does |
| --- | --- |
| [`outdoor-amateur-carousel-director`](instagram/outdoor-amateur-carousel-director/) | Builds prompt sets for a locked fictional persona in a swapped outfit, shot as an outdoor amateur/phone-photoshoot **carousel** for Nano Banana / Banana Pro. Two-phase: lock face + body on a base frame, then rebuild the rest of the set from the approved frame. |

Each skill folder contains a `SKILL.md`. The packaged `.skill` file sits alongside the folder for one-click upload.
