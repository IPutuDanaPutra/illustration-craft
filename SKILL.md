# Illustration Craft

A skill for ChatGPT (via its GitHub connector) with image-generation capability. It
generates a consistent set of illustrations from a shared style schema — asking the
user for missing details before generating anything.

Files in this repo:
- `illustration-schema.json` — the style + object schema (edit this to change output)
- `param-reference.md` — allowed values for each style parameter
- `SKILL.md` — this file (the instructions)

## How to use this skill

Connect this repository via ChatGPT's GitHub connector (Settings → Connectors →
GitHub), then say: *"Use this repository to create illustrations of [whatever you
want]."* The model should then follow the steps below.

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
language, no field names, no raw schema dumps. Ask one question per style dimension,
plus one about the object(s), so nothing gets silently defaulted without a chance to
change it:

For each of the four style dimensions below, do not just recite the schema's stored
default — pick a real recommendation from `param-reference.md`'s option list, suited to
the object(s) being illustrated, and say briefly why. The schema's stored value is a
fallback if the user has no preference and no recommendation fits better, not the thing
to lead with.

1. **Color** — recommend a palette as hex codes suited to the subject (e.g. warm
   reds/browns for a food subject, pastels for a birthday theme) per
   `param-reference.md` — never a color name, always a hex code — and ask if that
   works or if they'd prefer something else.
2. **Visual style** — recommend a style from `param-reference.md` (3D render, flat
   design, watercolor, line art, etc.) suited to the subject, and ask if that works or
   if they want something different.
3. **Lighting/mood** — recommend a mood from `param-reference.md` suited to the
   subject, and ask if that works or if they want something different.
4. **Background/composition** — recommend a background/composition from
   `param-reference.md` suited to the subject, and ask if that works or if they want
   something different.
5. **The object(s) to illustrate** — this is the one thing that's never defaulted or
   recommended away: confirm how many illustrations, and get a clear description of
   each one (subject, pose/angle, any distinguishing details). If the user already
   described this clearly, just restate it back for confirmation instead of re-asking.

Ask all of this as one combined, easy-to-skim message — not five separate messages —
and make clear the user can just say "use your recommendations" to accept 1–4 as
suggested. If everything was already answered earlier in the conversation, skip
straight to Step 3 — don't ask questions just to ask them.

**If the user asks to see the options for a specific dimension** (e.g. "give me the
options for visual style," "what colors can I pick from"), list the real choices from
`param-reference.md` for that dimension in plain language, and recommend one — don't
just dump the list and leave them to guess. Base the recommendation on what suits the
object(s) being illustrated (e.g. for a food subject, favor a warm/appetizing palette
and a style that reads well at small sizes over something like line art); say briefly
why in one clause.

Use `param-reference.md` as the allowed-value list when a user's answer needs
translating into a valid option (e.g. user says "warm and cozy" → map to a matching
lighting/mood value), but translate it back into plain language when talking to the
user — never surface the reference file's internal names either.

**If the user asks to see an example prompt** (e.g. "show me an example prompt,"
"what would the prompt look like"), assemble one using the Step 4 template with the
resolved (or currently recommended, if not yet confirmed) values and show it as plain
text — this is fine to share since it's just the final prompt, not the schema
structure behind it. Showing it doesn't skip Step 3's confirmation before generating.

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
