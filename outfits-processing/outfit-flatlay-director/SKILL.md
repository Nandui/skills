---
name: outfit-flatlay-director
description: "Turn a photo of a worn outfit into a richly detailed Nano Banana Pro (Gemini 3 Pro Image) prompt that recreates the clothing and footwear as a clean overhead knolling flat lay. Use whenever the user provides, attaches, or points to an image of a person in an outfit and wants a flatlay prompt, the outfit 'laid out' / 'flat lay' / 'knolling' / 'product shot', a breakdown of what they're wearing turned into a flat product image, or to extract the clothes from a photo and lay them flat — even when they don't say 'flatlay' or 'Nano Banana' explicitly (e.g. 'lay out this outfit', 'make this fit into a flat product photo', 'turn this pic into a clothing flat lay', 'extract the clothes from this and show them laid out')."
---

# Outfit Flatlay Director

Turn a single photo of a person wearing an outfit into one polished, copy-paste-ready **Nano Banana Pro** (Gemini 3 Pro Image) prompt. That prompt — with the original photo attached as a reference image — produces a clean, overhead **knolling flat lay** of the clothing and footwear, recreated in great detail.

The job has two halves, and both matter:

1. **The reference photo carries appearance fidelity.** When the user attaches it in Nano Banana Pro, the model can see the exact colors, fabrics, prints, and cuts. The prompt does not need to perfectly reverse-engineer every thread.
2. **The text carries the transformation the photo can't show.** The photo shows clothes *worn on a body*. Nobody has ever photographed these specific garments lying flat and separated. So the prompt's real work is to (a) name every piece so nothing gets dropped, (b) instruct the model to take each item *off the body* and lay it flat as a separate object, and (c) direct the overhead knolling layout, surface, light, and camera. Get those right and the result lands.

This is product/textile photography, not cinema — keep the realism language about fabric, weave, and true color, not pores and film grain.

---

## What counts as the outfit

Include **every garment and every piece of footwear**: tops, shirts, knitwear, dresses, jumpsuits, bottoms (pants, jeans, skirts, shorts), outerwear (jackets, coats, blazers), plus shoes/boots/sneakers. Treat belts, hats, and scarves as garments too — they're part of the worn outfit.

**Exclude** jewelry (earrings, necklaces, rings, bracelets, watches), bags/handbags, sunglasses, and small accessories. They clutter a clean clothing flat lay and the user has asked to leave them out. If the user explicitly asks to include accessories or jewelry this time, of course add them.

---

## Workflow

### 1. Get and actually look at the image

If no image is attached or referenced, ask for one — this skill can't run blind. Once you have it, **open and visually study it** before writing anything. You are doing real visual analysis, not guessing from a filename.

### 2. Build a private garment inventory

Go piece by piece through the outfit and, for each garment and pair of shoes, note:

- **Type** — what the piece is (e.g. oversized cable-knit sweater, high-waisted straight-leg jeans, low-top leather sneakers)
- **Color** — specific and true (e.g. "warm cream," "faded mid-blue," "off-white") rather than just "white" or "blue"
- **Fabric / material** — cotton knit, brushed wool, washed denim, smooth leather, satin, ribbed jersey, etc.
- **Fit / silhouette / cut** — oversized, cropped, fitted, straight-leg, A-line, relaxed
- **Construction details** — neckline, sleeve length and shape, hem, collar, pockets, pleats, paneling, distressing, embroidery
- **Closures / hardware** — buttons, zippers, drawstrings, eyelets, buckles (color/metal where it reads)
- **Print / graphic / logo** — any pattern, text, or graphic, described as you see it

This inventory is *working material*, not the deliverable. You write it so the prompt is complete and richly specific — the single biggest driver of a good result is that every visible piece is named and described, so the model knows exactly what to extract and lay out.

### 3. Resolve what you can't fully see

A worn-outfit photo never shows everything. Handle gaps deliberately:

- **Keep visible details exact.** Don't soften or generalize what you can clearly see.
- **Plausibly complete hidden parts.** The back of a shirt, the full length of pants, the second shoe — complete them consistently with what's visible. Bake this into the prompt by telling the model to render the complete garment laid flat, faithful to the visible portions.
- **Don't invent items.** If footwear is cropped out or simply not visible, don't conjure a specific shoe. Omit it and add a one-line note to the user, or ask — your call based on how central it is.
- **Flag low confidence briefly.** If the image is blurry or a color is ambiguous, make your best call and mention it in one short line rather than padding the prompt with hedges.
- **Multiple people?** Default to the most prominent/foreground person and say which one you used; offer to redo for another if it's ambiguous.

### 4. Compose the prompt

Fill the canonical structure below. Then deliver it.

---

## The Nano Banana Pro prompt structure

Use this exact skeleton. It is descriptive natural-language prose — what Nano Banana Pro responds to best — front-loaded with the task and the item list, because the model weights the front of the prompt most heavily.

```
Using the attached photograph as the exact reference for every garment, create a high-resolution flat-lay (knolling) product photograph of the complete outfit the person is wearing. Take each piece of clothing and footwear off the body and lay it flat on the surface, recreated faithfully and photographed straight down from directly overhead.

Lay out the following pieces, each matching the reference in color, fabric, pattern, cut, and proportion:
- [Garment 1 — rich one-line description: type, exact color, fabric, fit, key construction details, closures/hardware, any print or graphic]
- [Garment 2 — same depth]
- [Garment 3 — same depth]
- [Footwear — type, color, material, sole, fastening, any visible branding described generically]

Arrangement: a neat knolling layout — every item fully separated with even spacing, nothing overlapping, edges squared and aligned to an invisible grid, the composition balanced and centered in the frame. Place upper-body pieces toward the top, bottoms in the middle, and the shoes as a clean pair toward the bottom. Style each garment as if smoothed flat by hand: tops laid flat and squared with sleeves arranged naturally, bottoms laid straight with a crisp clean shape, outerwear laid open or neatly closed, shoes set side by side. Soft natural fabric folds where a real garment would settle — never stiff, never inflated, with no body, mannequin, or hanger filling them out.

Surface and background: a single seamless pure-white studio surface, clean and even, no props or clutter.

Lighting: soft, even, diffused overhead studio light with no harsh shadows and no hotspots; a subtle soft contact shadow directly beneath each piece so it sits with real weight and separates cleanly from the white.

Camera: a perfectly perpendicular 90-degree top-down view, true flat-lay perspective with no tilt and no keystone distortion, even sharp focus from edge to edge.

Fidelity and realism: match every item's exact color true-to-life, with realistic fabric texture and visible weave, real stitching and seams, accurate hardware, and any print, graphic, or logo reproduced as it appears. Render each fabric's natural character — matte cotton and knit, soft sheen on satin or leather, the grain of washed denim — with believable drape and fine natural wrinkles. Editorial e-commerce quality, ultra-detailed, photographed on a real camera — not a 3D render, not illustrated, not glossy CGI.

No person, no body, no mannequin, no hands, no hangers — only the garments and footwear lying flat on the surface.
```

**Notes on filling it in:**

- The item list is where "great detail" lives. Write a vivid, specific line per piece — this is the difference between a flat lay that looks like *these clothes* and a generic one.
- Keep descriptors **brand-neutral**: "white low-top leather sneakers with a thin gum sole," not a brand name. "Three-stripe athletic sneaker" is fine; the real brand is not. (Nano Banana Pro can render logos from the *attached reference* faithfully — the text just shouldn't name brands.)
- Don't write an aspect ratio into the prompt body — deliver it as a separate recommendation (next section).
- If the user picked a different look earlier (lifestyle surface, props, matching the source photo's mood), swap the surface/lighting lines accordingly. The default here is clean neutral knolling on white.

---

## Aspect ratio

Recommend one alongside the prompt:

- **1:1 (square)** — the default for knolling. Balanced, even, the natural flat-lay frame.
- **4:5 (portrait)** — better when the outfit has many pieces or long items (full-length coat + pants + boots) that need vertical room.

Set the ratio in the Nano Banana Pro UI; don't bury it in the prompt text.

---

## What you hand back

The deliverable is **one polished prompt**, ready to copy. Keep the wrapper tight:

1. **One short line** naming what you detected — e.g. *"Detected: cream cable-knit sweater, faded straight-leg jeans, white leather low-top sneakers."* This lets the user catch a misread instantly. Add any one-line caveat here (e.g. "shoes were cropped out, so I left them off").
2. **The prompt**, in a single fenced code block.
3. **Aspect ratio + usage note** — one line: which ratio and a reminder to **attach the original photo as the reference image** in Nano Banana Pro, since the whole approach depends on it.

Resist the urge to dump the full inventory or multiple variants — the user asked for a single best prompt. Everything you analyzed should be *expressed inside the prompt*, not pasted alongside it.

---

## Worked example

**Input:** a photo of someone wearing a chunky cream sweater, blue jeans, and white sneakers.

**Detected:** cream cable-knit sweater, faded straight-leg jeans, white leather low-top sneakers.

```
Using the attached photograph as the exact reference for every garment, create a high-resolution flat-lay (knolling) product photograph of the complete outfit the person is wearing. Take each piece of clothing and footwear off the body and lay it flat on the surface, recreated faithfully and photographed straight down from directly overhead.

Lay out the following pieces, each matching the reference in color, fabric, pattern, cut, and proportion:
- An oversized warm-cream cable-knit sweater in chunky brushed wool, dropped shoulders, a ribbed crew neckline, ribbed cuffs and hem, with a prominent vertical cable pattern down the front.
- High-waisted straight-leg jeans in faded mid-blue washed denim, with a five-pocket cut, visible contrast topstitching, a button-and-zip fly, and light natural fading at the thighs.
- White low-top leather sneakers, smooth leather upper with a rounded toe, flat white laces, and a thin off-white rubber sole.

Arrangement: a neat knolling layout — every item fully separated with even spacing, nothing overlapping, edges squared and aligned to an invisible grid, the composition balanced and centered in the frame. Place the sweater toward the top, the jeans in the middle laid straight, and the sneakers as a clean pair toward the bottom. Style each garment as if smoothed flat by hand: the sweater laid flat and squared with sleeves arranged naturally beside the body, the jeans laid straight with a crisp clean shape, the sneakers set side by side. Soft natural fabric folds where a real garment would settle — never stiff, never inflated, with no body, mannequin, or hanger filling them out.

Surface and background: a single seamless pure-white studio surface, clean and even, no props or clutter.

Lighting: soft, even, diffused overhead studio light with no harsh shadows and no hotspots; a subtle soft contact shadow directly beneath each piece so it sits with real weight and separates cleanly from the white.

Camera: a perfectly perpendicular 90-degree top-down view, true flat-lay perspective with no tilt and no keystone distortion, even sharp focus from edge to edge.

Fidelity and realism: match every item's exact color true-to-life, with realistic fabric texture and visible weave, real stitching and seams, accurate hardware, and any print or graphic reproduced as it appears. Render each fabric's natural character — the matte chunky wool of the knit, the grain of washed denim, the smooth leather of the sneakers — with believable drape and fine natural wrinkles. Editorial e-commerce quality, ultra-detailed, photographed on a real camera — not a 3D render, not illustrated, not glossy CGI.

No person, no body, no mannequin, no hands, no hangers — only the garments and footwear lying flat on the surface.
```

**Aspect ratio:** 1:1 (square). Attach the original photo as the reference image in Nano Banana Pro — the prompt relies on it for exact colors and fabric.
