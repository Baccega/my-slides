---
theme: seriph
title: Presentations
class: text-center
transition: fade-out
---

# Slide decks

Choose a presentation (each deck is built and deployed on its own — see README):

<div class="mt-12 grid gap-6 text-left max-w-md mx-auto text-lg">

[**Tutorial** — Slidev starter & features](/tutorial/)

[**Cursor** — company ad](/cursor-ad/)

</div>

---

## Local development

Run the hub with `pnpm dev`, or a nested deck with `pnpm dev:tutorial` / `pnpm dev:cursor-ad`.

The links above match **path bases** if you deploy multiple apps under one domain; with a **single** deployed deck, change [`vercel.json`](./vercel.json) / [`netlify.toml`](./netlify.toml) `buildCommand` to the matching `build:*` script.
