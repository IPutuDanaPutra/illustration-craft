# Illustration Craft

A portable skill for any model with image-generation capability (ChatGPT, Gemini, etc.).
It generates a consistent set of illustrations from a shared style schema — asking the
user for missing details before generating anything.

Files in this repo:
- `illustration-schema.json` — the style + object schema (edit this to change output)
- `param-reference.md` — allowed values for each style parameter
- `SKILL.md` — this file (the instructions)

## How to use this skill

Tell the model: *"Use the skill at [this repo/link] to create illustrations."*
The model should then follow the steps below.

## Step 1 — Load the schema

Read `illustration-schema.json`. It has two parts:

- `global_variables` — style rules shared by every illustration (color palette, visual
  style, lighting, background/composition, negative prompt).
- `frame_objects` — one entry per illustration to generate (subject + pose/angle).

## Step 2 — Ask before generating (mandatory)

Do not generate any image yet. Check the schema for gaps or ambiguity and ask the user
about them in a single batch of questions, for example:

- Any `global_variables` field that is empty, a placeholder, or lists multiple
  comma-separated options instead of one choice (e.g. `param1_color` listing three
  hex codes) — ask the user to pick one, or confirm "use all three, vary per object."
- Any `frame_objects` entry that is vague about pose, angle, or count.
- Whether the illustrations must look consistent as a *set* (same style/lighting across
  all frames) or can vary per object.
- Output format/size if the target tool requires it (e.g. aspect ratio, transparent PNG).

If the schema is already fully specified and unambiguous, skip straight to Step 3 —
don't ask questions just to ask them.

Use `param-reference.md` as the allowed-value list when a user's answer needs
translating into a valid option (e.g. user says "warm and cozy" → map to a matching
`param4_lighting_mood` value).

## Step 3 — Confirm the final parameters

Summarize the resolved values (all six params + object list) back to the user in one
short block and get explicit go-ahead before generating.

## Step 4 — Assemble one prompt per frame object

For each entry in `frame_objects`, build a single image-generation prompt using this
template:

```
Subject: {frame_object description}
Style: {param2_visual_style}
Color palette: {param1_color}
Lighting/mood: {param4_lighting_mood}
Background/composition: {param5_background_composition}
Avoid: {param6_negative_prompt}
```

Keep `param1_color`, `param2_visual_style`, `param4_lighting_mood`, and
`param5_background_composition` identical across all frame objects unless the user said
otherwise in Step 2 — this is what makes the set look like one coherent illustration
pack rather than unrelated images.

## Step 5 — Generate

Generate one image per frame object using the assembled prompts, in the order listed
in `frame_objects`. Present them together as a set.

## Step 6 — Offer refinement

After generating, ask if the user wants to adjust any single object or global parameter
and regenerate just that frame (not the whole set), to avoid wasting generations on
unaffected objects.
