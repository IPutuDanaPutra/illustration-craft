# Illustration Craft

**A guided illustration-generation skill for ChatGPT.**

Illustration Craft turns a scattered back-and-forth of "make it more like this, change
the lighting, no not that color" into a single guided flow: the model recommends a
style, confirms what you want, then produces a coherent *set* of illustrations that all
share one visual identity — same palette, same lighting, same mood — instead of a pile
of unrelated images.

> **Supported platforms:** ChatGPT only, for now, via its GitHub connector. Gemini
> can't connect to a plain GitHub repository without a Google Workspace account with
> the GitHub Marketplace app installed, which most personal accounts don't have — so
> it isn't supported until that's available another way.

## Why this exists

Most "just generate an image" prompts drift. Ask for four illustrations one at a time
and you'll often get four different styles, four different lighting setups, and a
color palette that wanders. Illustration Craft fixes that by generating every
illustration in a set from one shared style definition, resolved once at the start of
the conversation — not re-guessed for every image.

## Setup (do this once)

The skill lives in this GitHub repository. For it to work reliably, ChatGPT needs to
read the repository directly — pasting a plain link doesn't work consistently (it
can't always fetch external links, and even when it can, it may only skim the page
instead of following it).

1. Go to **Settings → Connectors → GitHub** and connect your GitHub account.
2. In a new chat, reference this repository (e.g. `@IPutuDanaPutra/illustration-craft`
   or however ChatGPT's connector UI lets you attach it) and say what you want — see
   [How to use](#how-to-use) below.

## How to use

Before starting, make sure you've completed the [Setup](#setup-do-this-once) step
above — GitHub connected in ChatGPT. If it's not connected yet, do that first; the
steps below assume it's already done.

1. **Reference the repository.** In a new chat, say something like:

   > "Use https://github.com/IPutuDanaPutra/illustration-craft to create illustrations
   > of [whatever you want]."

2. **Answer the style questions.** The model will recommend a color palette, visual
   style, lighting/mood, and background/composition suited to what you're illustrating
   — each with a short reason — and ask if each one works or if you'd rather change it.
   No preference? Just reply "use your recommendations."

3. **Confirm the object(s).** Say what you want illustrated — subject, pose/angle, how
   many. If you already said this up front, the model will just restate it back for you
   to confirm instead of asking again.

4. **Review the summary.** Before generating, the model recaps the resolved style and
   object list in plain language. Check it, adjust anything that's off, then approve.

5. **Get your illustration set.** All illustrations generate together, sharing one
   consistent look.

6. **Refine as needed.** Want one illustration's lighting warmer, but everything else
   is perfect? Ask for just that change — no need to regenerate the whole set.

## Example conversation

> **You:** Use the Illustration Craft skill to create a Bakso illustration.
>
> **Model:** Before generating, here's what I'd suggest:
> - **Color:** warm reds and browns (`#8B4513`, `#C1440E`) to feel appetizing
> - **Visual style:** detailed 3D render — reads clearly at any size
> - **Lighting:** warm, appetizing studio light
> - **Background:** transparent, so it drops into any layout
>
> Do these work, or want something different? Also — one Bakso illustration, or a set
> (e.g. bowl view, close-up, street-cart scene)?
>
> **You:** Use your recommendations, just one bowl view.
>
> **Model:** Got it — generating now.

## Pro tips

- **Ask to see the options.** "Give me the options for visual style" (or color,
  lighting, background) gets you the real list of choices, with a recommendation
  called out — not just a random pick with no explanation.
- **Ask for an example prompt.** "Show me an example prompt" gets you the actual
  assembled prompt text the model would use to generate — useful if you want to see
  exactly what's about to happen, or reuse the prompt elsewhere. This doesn't skip the
  confirmation step; it still asks before generating.
- **Be specific about mood, not mechanics.** "Warm and cozy" or "playful and bright"
  works better than trying to specify exact technical settings yourself — the model
  translates intent into the right choices.
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
