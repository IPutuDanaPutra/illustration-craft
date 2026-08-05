# Illustration Craft

**A portable illustration-generation skill for any AI model with image-generation
capability.**

Illustration Craft turns a scattered back-and-forth of "make it more like this, change
the lighting, no not that color" into a single guided flow: the model asks the right
questions upfront, confirms what it heard, then produces a coherent *set* of
illustrations that all share one visual identity — same palette, same lighting, same
mood — instead of a pile of unrelated images.

It isn't tied to any one platform. Point ChatGPT, Gemini, or any other model with image
generation at this repository, and it follows the same instructions.

## Why this exists

Most "just generate an image" prompts drift. Ask for four illustrations one at a time
and you'll often get four different styles, four different lighting setups, and a
color palette that wanders. Illustration Craft fixes that by generating every
illustration in a set from one shared style definition, resolved once at the start of
the conversation — not re-guessed for every image.

## How it works

1. **Load** — the model reads the skill's style configuration.
2. **Ask** — before generating anything, it checks for gaps or ambiguity and asks you
   about them in one batch, not one question at a time.
3. **Confirm** — it summarizes what it understood and waits for your go-ahead.
4. **Generate** — it builds one illustration per requested object, keeping the shared
   style identical across all of them.
5. **Refine** — afterward, you can tweak a single illustration or a single style
   choice and regenerate just that piece, without redoing the whole set.

The style configuration itself (color logic, rendering parameters, and how they're
assembled into prompts) is Illustration Craft's internal engine and isn't documented
here — you don't need to understand it to use the skill, only to talk to the model
naturally about what you want.

## Getting started

1. **Start a new chat and paste [`PASTE-THIS.md`](./PASTE-THIS.md) as your first
   message.** This is the recommended way to use the skill — it bundles the
   instructions and schema into one message, so the model doesn't need to fetch
   anything from a link. Chat assistants don't reliably treat a fetched URL as binding
   instructions (some can't fetch external links at all, and even when they can, they
   may only skim it instead of following it step by step), so pasting the full skill
   directly is the version that works consistently across ChatGPT, Gemini, and
   different plans/models.

2. **Answer the setup questions.** The model will ask about things like color
   preference, visual style (3D render, watercolor, flat design, etc.), lighting mood,
   and background/composition — only for whatever isn't already decided. If you don't
   have a preference, say so and it will choose something sensible.

3. **Describe what you want illustrated.** For example: "a birthday gift, a birthday
   cake, a teddy bear, and a rose, all front-facing and tilted slightly right." The
   model will resolve this into a matching set of illustrations.

4. **Review the confirmation.** Before generating, the model recaps the resolved style
   and object list. Check it, adjust anything that's off, then approve.

5. **Get your illustration set.** All illustrations generate together, sharing one
   consistent look.

6. **Refine as needed.** Want the teddy bear's lighting warmer, but everything else is
   perfect? Ask for just that change — no need to regenerate the whole set.

## Example conversation

> **You:** Use the Illustration Craft skill to create illustrations of a birthday
> gift, a birthday cake, a teddy bear, and a rose.
>
> **Model:** Before I generate, a few quick questions — any color palette preference,
> or should I choose one? Do you want a specific art style (3D render, watercolor,
> flat design, etc.), and should the background be transparent or something else?
>
> **You:** Soft pastel colors, 3D render style, transparent background.
>
> **Model:** Got it — pastel palette, 3D render, transparent background, applied
> consistently across all four illustrations. Generating now.

## Tips for best results

- **Be specific about mood, not mechanics.** "Warm and cozy" or "playful and bright"
  works better than trying to specify exact technical parameters yourself — the model
  translates intent into the right settings.
- **Ask for a set, not singles.** Illustration Craft is built for generating multiple
  illustrations that belong together; you'll get the most value requesting several at
  once rather than one-off images.
- **Iterate narrowly.** When something's close but not quite right, name the specific
  illustration and the specific thing to change, rather than restarting the whole
  request.

## License

This project is proprietary. See [LICENSE](./LICENSE) for terms — all rights
reserved; no reuse, redistribution, or derivative works are permitted without
written permission.
