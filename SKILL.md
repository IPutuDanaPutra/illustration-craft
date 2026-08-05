# Illustration Craft

A portable skill for any model with image-generation capability (ChatGPT, Gemini, etc.).
It generates a consistent set of illustrations from a shared style schema — asking the
user for missing details before generating anything.

Files in this repo:
- `illustration-schema.json` — the style + object schema (edit this to change output)
- `param-reference.md` — allowed values for each style parameter
- `SKILL.md` — this file (the instructions)

## How to use this skill

Connect this repository to the model (e.g. ChatGPT's GitHub connector, or an
equivalent), then say: *"Use the Illustration Craft skill to create illustrations of
[whatever you want]."* The model should then follow the steps below.

## Step 1 — Load the schema (silently)

Load `illustration-schema.json` from the Illustration Craft repository using whatever
access you have (a connected GitHub source, a fetched URL, an attached file). Do this
quietly — don't narrate the loading process to the user ("I loaded the schema from the
repository...") and don't expose internal field names like `param1_color` or
`frame_objects`. Just use the data.

This file has two parts:

- `global_variables` — style rules shared by every illustration (color palette, visual
  style, lighting, background/composition, negative prompt).
- `frame_objects` — placeholder example objects. If the user has already told you what
  they want illustrated (in this message or an earlier one), ignore these placeholders
  entirely and don't mention them — use the user's own request instead. Only bring up
  the placeholders if the user hasn't said what they want yet and you need an example
  to explain what kind of thing to ask for.

## Step 2 — Ask before generating (mandatory)

Do not call the image-generation tool yet, and do not generate an image of a summary,
form, or confirmation card — Steps 2 and 3 are plain conversational text only, nothing
visual. Ask like a friendly designer, not a system reporting its internal state — plain
language, no field names, no raw schema dumps. Check for gaps or ambiguity and ask about
them in a single batch of natural questions, for example:

- If the style has multiple options instead of one clear choice (e.g. several colors
  listed) — ask the user to pick, or confirm "use them all together."
- Anything vague about the requested object(s): count, pose, angle, composition.
- Whether the illustrations must look consistent as a *set* (same style/lighting across
  all of them) or can vary per object.
- Output format/size if the target tool requires it (e.g. aspect ratio, transparent PNG).

If everything is already clear and unambiguous, skip straight to Step 3 — don't ask
questions just to ask them.

Use `param-reference.md` as the allowed-value list when a user's answer needs
translating into a valid option (e.g. user says "warm and cozy" → map to a matching
lighting/mood value), but translate it back into plain language when talking to the
user — never surface the reference file's internal names either.

## Step 3 — Confirm the final parameters

Summarize the resolved style and object list back to the user in plain, friendly
language (a short bulleted list is fine — color, style, lighting, background, and the
objects to generate — but no internal field names). It must be text the model writes
in the chat, not an image, card, or rendered graphic. Get explicit go-ahead before
generating. The image-generation tool is only ever called in Step 5.

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
