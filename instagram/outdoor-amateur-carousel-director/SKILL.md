---
name: outdoor-amateur-carousel-director
description: "Nano Banana / Banana Pro prompt director for outdoor amateur-photoshoot carousels of a locked fictional adult persona in a swapped outfit. Two-phase workflow: Phase 1 builds one base frame from separate FACE_REFERENCE_HERE and BODY_REFERENCE_HERE placeholders with a hard written identity lock (face geometry + exact body volume, anti-slimming, anti-stretching); Phase 2 rebuilds the remaining carousel frames from the approved base image as a single REFERENCE_HERE master anchor. Realism is achieved positively — physical skin, fabric, and phone-sensor detail — never negation stacks. Use whenever the user wants an outfit swap onto a model reference, a multi-image outdoor carousel, 'dress her in this outfit', 'amateur photoshoot set', body-fit corrections ('body is slimmer/stretched/off'), or reference-lock prompt work for Nano Banana 2, Banana Pro, or similar image models."
---

# Outdoor Amateur Carousel Director

Builds prompt sets for a locked persona wearing a swapped outfit, shot as an outdoor amateur/phone-photoshoot carousel (default 4 frames). Target read: **a real person's unedited phone pictures from one outing** — never editorial, never studio, never AI-render gloss.

Built for Nano Banana 2 / Banana Pro models that accept reference images plus a text prompt. Reference images carry identity; the prompt narrates the moment and locks the fit.

---

## THE TWO-PHASE WORKFLOW (CORE INSIGHT)

The single most important lesson baked into this skill: **don't fuse three references per frame forever.**

**Phase 1 — Base frame.** Use separate face and body references with a heavy written identity lock to generate ONE approved base image (the main feed shot). This is where drift gets fixed.

**Phase 2 — Remaining frames.** Once the base is approved, it becomes the single master reference for every other frame. The fusion step (face-from-here + body-from-there + outfit-from-text) is where drift comes from — eliminating it is the fix. Prompts get leaner; the heavy lifting moves from text to the reference.

**Anti-drift rules:**
- Always branch every frame from the same approved base. **Never chain outputs of outputs** (Frame 2's output must not become Frame 3's reference) — generational drift compounds fast.
- Phase 2 default is base image ONLY. Add the face close-up as a second reference only for the close-up frame, and only if the first attempt drifts. Frames where the face is small never need it.
- If the tool exposes reference weights: in Phase 1, weight the body ref up relative to the face ref (face transfers easily; body is what drifts). In Phase 2 with two refs, keep the base image primary.
- For body-critical frames, a rear/three-quarter full-body reference transfers hip/glute volume better than a front-facing one.

---

## REFERENCE PLACEHOLDERS

Never write `@image` tags. Use literal placeholders the user replaces in their tool's UI:

- Phase 1: `FACE_REF_HERE` and `BODY_REFERENCE_HERE`
- Phase 2: `REFERENCE_HERE` (the approved base image)

Repeat the placeholder at every point in the prompt where the lock matters (identity sentence, garment-fit clauses, body-volume clauses) — repetition reinforces the anchor.

---

## THE IDENTITY LOCK (belt and braces)

Reference instruction alone is not enough. Every Phase 1 prompt locks identity TWO ways simultaneously:

1. **Reference instruction:** "whose face is taken exactly from FACE_REFERENCE_HERE and whose body is taken exactly from BODY_REFERENCE_HERE"
2. **Written description** of the actual markers pulled from the refs, so the model has two anchors. Extract and write out: face shape, cheeks, jawline, eye colour/shape, liner style, brows, lips, piercings/marks, skin tone, hair colour + signature streaks/styling.

### The body lock (the hard part — where models fail)

Image models default to a tall, straight, elongated editorial body. If the persona is anything else, the lock must say so explicitly, in both positive and negative terms:

- **Stature:** "petite and short in stature with a compact frame and proportionally shorter legs, not tall or elongated"
- **Shape contrast:** name the actual silhouette — e.g. "slim arms, a genuinely narrow waist, and a pronounced curvy lower body — wide flared hips that read wider than her shoulders, full rounded glutes, and full thighs that nearly touch — a strong hourglass contrast"
- **Volume pin:** "reproduced at exactly the volume shown in BODY_REFERENCE_HERE"
- **Triple negative:** "never slimmed down, never stretched taller, never straightened into a slim fashion-model silhouette"

### Garment-conformance language

Clothing must drape on HER body, not a mannequin. Weave fit physics into the outfit description: "the waistband sitting snug against her wide hips with the fabric filled out through the hip and seat before falling loose down the leg", "faint horizontal tension lines across the fullest part of her hips and thighs", "every garment draping and tensioning against her actual body volume from BODY_REFERENCE_HERE."

---

## OUTFIT EXTRACTION

When the user uploads an outfit reference (often an anime/illustration image — translate to real-world garments):

- Catalogue every garment top to bottom: fabric, colour (precise — "rust-brown", "dusty slate-blue", "taupe-khaki"), cut, fit, layering, closures, prints/graphics (described generically), accessories, footwear.
- Mirror the spec back in one tight sentence before writing prompts, so the user can correct.
- **No brand names** in prompt output. No character/persona names. Describe text/graphics on clothing generically.
- Match light and location to the outfit's palette and vibe (earthy fit → golden hour gravel/stone; cyber-street fit → urban concrete; cool Y2K → overcast street).

---

## EVERY PROMPT IS SELF-CONTAINED

No frame may say "the same outfit as before" relative to *another prompt in the chat*. Each prompt carries the full outfit spec (Phase 1) or anchors everything to REFERENCE_HERE (Phase 2: "keeping her exact face, hair, body proportions, and complete outfit from REFERENCE_HERE — [one-line outfit recap] — and the same warm late-afternoon sunlight"). The user pastes prompts independently; nothing depends on conversational context.

---

## POSITIVE REALISM (the anti-AI-slop method)

Don't lean on negation ("no AI, no plastic"). Build realism by describing **what a phone camera physically records**. Pull 6–10 of these per frame, matched to distance, light, and weather:

**Skin (close range):** fine visible pores across nose and cheeks · slight natural redness at the nostril edges · peach fuzz lit along the jawline · real vertical lip creases under gloss, slight dryness at the centre · individual lower lashes · inner eye corners slightly glossy · soft under-eye texture/shadow · faint natural blotchiness on the chest · a small dry patch near an elbow · tiny faint blemish marks near the jaw

**Skin (body/midriff):** natural soft creasing of real skin at the waist · navel · fine downy hair caught in raking light · faint tan-line contrast · slight redness where a waistband or strap presses · real skin tone variation rather than an even surface · pressure crease from a choker/strap · thin sweat sheen at hairline and collarbone (warm) · goosebumps (cold) · fine arm hair backlit on bare shoulders

**Hair:** flyaways crossing the face · baby hairs curling at the temple · split ends · strands stuck briefly across the lips · coloured streaks glowing semi-translucent where backlit · real weight and movement

**Fabric:** creases at the ruching/hip crease/behind the knee · one hem dragging · worn fade lines at knees and pocket edges · a loose thread at one seam · dust on a thigh · scuffed leather, dulled buckle with fine scratches, a stretched second belt hole · fabric wrinkled from a day of wear

**Hands:** creased knuckles · short natural nails · simple stable jobs (thumb hooked in belt, hand in pocket, swinging loose)

**Phone sensor:** HDR pulling bright sky flat grey-white · faint HDR halo at a bright frame corner · mild sensor noise/grain in shadows · faint chromatic fringe on backlit hair edges · mild motion blur on a trailing hand or swinging drawstring · horizon tilted a few degrees · off-centre framing, too much sky or path · clipped shoulder/pocket at the crop edge · mild wide-angle distortion held low and close · focus locked on the near eye, background melting to compression softness

**Keep exactly one short anti-render close** at the end of every prompt:

> The frame reads as an ordinary unedited phone picture of a real adult woman outdoors, with no smoothing, no render gloss, and no interface elements.

**Trap words — never use as positive goals:** `8k`, `hyperrealistic`, `flawless`, `perfect skin`, `cinematic masterpiece`, `studio lighting`, `anamorphic`, `35mm film grain`, `supermodel body`, `dramatic rim light`.

---

## THE DEFAULT 4-FRAME CAROUSEL

Same persona, same outfit, same location, same light = one believable outing. Vary capture method, framing, pose.

1. **Main feed photo** — full/three-quarter body, taken by a friend a few steps away, weight on one hip (pushes the hip curve out — helps the body read), golden-hour or soft daylight. *This is the Phase 1 base frame.*
2. **Candid story still** — mid-step walking, glancing aside, mouth relaxed as if mid-sentence, motion blur on trailing hand and swinging garment ties, tilted horizon, loose crop.
3. **Face/top detail close-up** — chest-up, phone just above eye level, mild close-range distortion, focus on the near eye, maximum honest skin read, garment neckline/collar detail.
4. **Outfit detail close-up** — midsection/belt/hem or legs/shoes, phone held low and close, wide-angle edge distortion, focus on a hard object (buckle), raking light on fabric and skin texture.

Each frame in its own fenced code block, labeled. Deliver Frame 1 alone first when running the two-phase flow; deliver 2–4 together after the base is approved.

---

## PROMPT SKELETONS

### Phase 1 — base frame

```
A vertical smartphone photo taken by a friend of a young woman whose face is taken exactly from FACE_REFERENCE_HERE and whose body is taken exactly from BODY_REFERENCE_HERE. Her face is locked to the reference: [written face spec — shape, cheeks, jaw, eyes + liner, brows, lips, piercings, skin tone, hair colour + streaks]. Her body is locked to BODY_REFERENCE_HERE and must match it exactly: [stature line, not tall or elongated], [shape-contrast description], reproduced at exactly the volume shown in BODY_REFERENCE_HERE, never slimmed down, never stretched taller, never straightened into a slim fashion-model silhouette. She stands [location + pose, weight on one hip], wearing [full outfit spec with fit-physics clauses tied to BODY_REFERENCE_HERE]. [Light direction + what it does to her face]. [6–10 positive realism details: skin, hair, fabric]. [Phone sensor behaviour + casual crop]. The frame reads as an ordinary unedited phone picture of a real adult woman outdoors, with no smoothing, no render gloss, and no interface elements.
```

### Phase 2 — subsequent frames

```
A [spontaneous vertical phone story still / close chest-up phone photo / close detail crop phone photo] of the same woman as REFERENCE_HERE, keeping her exact face, hair, body proportions, and complete outfit from REFERENCE_HERE — [one-line outfit recap] — and the same [light] as REFERENCE_HERE. She is now [new framing, pose, action in the same location]. [Frame-appropriate positive realism details]. [Phone sensor behaviour for this capture type]. The frame reads as an ordinary unedited phone picture of a real adult woman outdoors, with no smoothing, no render gloss, and no interface elements.
```

---

## DRIFT TRIAGE (when the user reports a bad output)

| Symptom | Fix |
|---|---|
| Body slimmer / stretched / taller | Harden the body lock: add stature line, shape-contrast description, volume pin, triple negative. Add garment tension-line language. Bump body-ref weight. Use rear/three-quarter body ref. |
| Face drifted on close-up | Add original face close-up as secondary ref for that frame only; base stays primary. |
| Output looks AI/editorial | Audit for trap words; add more sensor imperfections and mundane fabric/skin details; check the location isn't reading as a set. |
| Outfit changed between frames | Confirm frames branch from the approved base, not from each other; ensure each prompt recaps the outfit. |
| Lighting inconsistent across set | Pin the same light phrase in every frame ("the same warm late-afternoon sunlight as REFERENCE_HERE"). |

---

## DELIVERY RULES

1. One fenced code block per frame, labeled (**Frame 1 — main feed photo**, etc.).
2. Short outfit-spec mirror + any assumptions BEFORE the code blocks; keep it terse, no process dump.
3. No aspect ratios in prompt bodies — framing in plain language only; the user sets ratio in the tool.
4. No names, no brands, no `@image` tags, no negative-prompt blocks, no meta-commentary inside prompts.
5. After delivering Phase 1, remind the user: approve the base, then return for Phase 2 frames anchored to it.
6. Minor iterations on an approved prompt (pose tweak, light nudge, single garment swap) ship directly without re-confirmation.