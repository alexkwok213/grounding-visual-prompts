---
name: grounding-visual-prompts
description: Use when writing, replicating, or fixing image or video generation prompts from reference clips, GIFs, screenshots, or characters — especially when generated outputs have reversed motion or gaze direction, vanishing props, awkward pauses, jitter or trembling, invented actions, or reference-vs-prompt conflicts.
---

# Grounding Visual Prompts

```
NO GENERATION PROMPT WITHOUT A VERIFIED VISUAL-LOGIC SPEC FIRST.
```

**Violating the letter of the gates is violating the spirit of the gates.** Never draft, patch, or submit an image/video prompt from memory, low-fps sheets, or partial user complaints. Produce Gates 1–4 visibly first. For target-model syntax (tags, timestamps, limits, negative fields), always read that model's own guide.

**Use when:** Replicating/remastering clips or GIFs, character-swapping onto reference motion, writing I2I/I2V/reference prompts, or diagnosing failed outputs.
**Skip when:** Pure open-ended ideation with no reference assets and no spatial/prop continuity constraints.

## The 4 Pre-Prompt Gates (REQUIRED)

Produce all 4 Gates visibly — including the full Gate 2 and Gate 3 markdown tables — **before** drafting or editing any prompt. Never replace the tables with prose bullets. **If Gate 1 or Gate 4 fails, STOP** — clean local clips first or ask the user how to resolve asset conflicts. **Never output a "ready-to-submit" JSON with unresolved blockers tucked into footnotes.**

### Gate 1 — Source Cleanliness & >=8fps Observation Log
1. Probe every video (`ffprobe` resolution, fps, duration, audio) and view reference images directly.
2. Extract **>=8fps frames** (or every frame if <=3s) and explicitly inspect the **first 3 and last 3 frames**.
3. If loop-reset jump cuts, UI/white-screen frames, or boundary glitches appear -> **STOP: trim a clean single loop and re-verify its exact first and last frames.**
4. **Track screen position progression:** For every moving subject, record its horizontal/vertical path across consecutive zoomed frames (`x_start -> x_mid -> x_exit`, e.g., `viewer-left -> center -> viewer-bottom-right`). Never trust direction claims in a broken previous prompt.
5. Log only visible facts; mark unclear moments `[UNCERTAIN]` and zoom/re-extract. Never invent unseen acting or unheard audio (e.g., turning a silent open eating mouth into a roar, or inserting an extra roar beat when shock leads straight into action).

### Gate 2 — Beat, Gaze & Physics Vector Table (REQUIRED TABLE)
Use **viewer-perspective coordinates** (`viewer-left`, `viewer-right`, `viewer-top`, `viewer-bottom`). Pair any anatomical side explicitly with viewer side (e.g., `right arm (viewer-left)`).

| Beat | Trigger | Visible Expression | Motion Vector (Viewer) | Recoil & Gaze Target | Continuous or Pause? |
|---|---|---|---|---|---|

- **Newton & Gaze Check:** Impact vector must match exit trajectory (a kick moving `viewer-left -> viewer-right` exits `viewer-right`, never `viewer-left`), and ending gaze must follow the exit target.
- **Duration Ratio Check:** If `target_duration > 1.5x source_duration`, pick the model's shortest valid duration or design a natural lead-in — never stretch one beat with a freeze.

### Gate 3 — Prop Conservation Ledger (REQUIRED TABLE)
Trace every key prop through the **final 3 frames** of the source:

| Prop | Start (Holder & Viewer Side) | Mid-Action State | End (Holder & Viewer Side) |
|---|---|---|---|

Every prop must stay continuously held, drop visibly and remain in frame, or cleanly exit frame.

### Gate 4 — Reference-vs-Prompt Conflict Matrix
Audit every `reference_image`, `reference_video`, `first_frame`, `file_id`, and user goal:
1. **State conflict:** Never mark a mid-action image `fully_preserved` if the prompt asks for a different opening state — supply a matching clean opening frame, align opening action to the image, or downgrade to `partially_preserved` and ask user sign-off on risk first.
2. **Stale-upload conflict:** Confirm every `file_id`/URL comes from the *newly cleaned* local file, never an older uncleaned upload.
3. **Role/API conflict:** Confirm reference roles (`first_frame` vs `reference_image` vs `reference_video`) combine legally in the target model.

## Prompt Translation Contract
- **Continuous causal narrative:** Write `trigger -> immediate reaction -> follow-through -> steady finish`. Place timestamps **only** where the target model's official guide requires them (e.g., shot cuts `[Shot 2]`, never micro-timestamps inside a single continuous shot).
- **Positive stillness ("No Pink Elephants"):** Video models weakly parse `without trembling` or `zero shaking`. State desired physics positively (`lifts once and locks still in a steady pose`, `grips firmly throughout`).
- **Zero ungrounded embellishment:** Every expression, vector, prop state, and sound must map 1-to-1 to Gates 1–3.

## Fixing Failed Generations & Post-Generation Check
- **Full Audit, Never Patch:** On user complaints ("hand shakes", "prop vanishes", "weak expression"), extract >=8fps frames of **both** the failed output and clean source, then fill out **all 4 Gates from scratch** — auditing every row of Gate 2 (`Motion Vector` + `Recoil & Gaze Target`) against the broken prompt to catch **unreported** direction/gaze reversals, stale `file_id`s, micro-timestamps, and pink-elephant negatives.
- **Post-Gen Verification:** Before claiming a generated output passes, extract `>=2fps` frames spanning the full clip and verify every row of Gate 2 (expression, motion vector, gaze) and Gate 3 (prop start/mid/end).

## Rationalizations & Red Flags — STOP and Re-Run Gates

| Excuse | Reality |
|---|---|
| "User is rushing; just audit the 2 words complained about and skip tables." | Complaints hide reversed vectors, wrong gaze, and stale uploads. Fill every Gate 2 & 3 column from fresh frame progression. |
| "Output ready-to-submit JSON and note dirty video / image conflict below." | Gate 1/4 failures block submission. Fix clips or stop with the blocker first. |
| "Mark `<Picture 1>` `fully_preserved`, then tell text to omit an object at 0s." | Direct self-contradiction; visual anchors override text. |
| "Add `without trembling` and `hold 00:00.25–00:01.20` to force compliance." | Negations trigger jitter ("pink elephant"); micro-timestamps cause robotic pauses. |
| "Open mouth in a silent clip means it's roaring; add a roar beat." | Silent open mouth != roar. Never invent ungrounded acting or extra beats. |
| "Reuse existing `mm_file://` URL to save time." | Reusing an uncleaned upload re-injects loop cuts. Verify upload provenance. |
