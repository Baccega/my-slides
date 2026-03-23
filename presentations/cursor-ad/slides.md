---
addons:
  - ..
theme: seriph
title: Cursor — Examples from my storywise day-to-day
info: |
  Internal talk: real bugs, how we used the tool, and what we’d do again.
class: text-left
transition: slide-left
drawings:
  persist: false
comark: true
duration: 10min
---

# Cursor on real work

**Examples from my storywise day-to-day**

<!--
Goal: show what happened in our codebase, not slides from Cursor’s marketing.
-->

---
layout: intro
---

## What this talk is

- Cursor applied to **real tickets**
- A few tips that made the difference

<!--
Honest section later; Q&A welcome.
-->

---

# Story 1 — YouTrack export titles

#### What the client sent:

```text
Ich habe gerade wieder nach youtrack gepusht.

Organisationseinstellungen:
{abbreviation}: {auto}

Projekteinstellungen:
{abbreviation}: {oneSentence}

Für das Projekt wurde für User Stories exportiert: {abbreviation}: ({persona}) {action}
und für Anforderungstexte der Titel. Ich weiß nicht, welches Format bei auto rauskommt,
aber es wurde offenbar nicht die Einstellung aus dem Projekt verwendet.
```

**TL;DR**: The export template feature does not work as expected.

---
layout: image-right-wide-text
image: /assets/pikachu.png
backgroundSize: auto 35%
---

## The Single Prompt that fixed the issue

```text [Prompt] {1-3|5|6-7}
<client message>

this is the message from the client.

@common/helper/export.template.helper.ts:15 
it is probably due to this @common/helper/export.template.helper.ts:76-79
logic.
```

No magic, just the message + `@` file references + my guess.

<!--
Mention how @-mentions work.
-->

---

## What was wrong

Resolution order was:

```ts
tenant ?? project ?? default
```

Project-specific settings should win over tenant defaults:

```ts
project ?? tenant ?? default
```

Basically the kind of nasty bug that passed multiple reviews.

<!--
Nullish coalescing: first non-nullish wins. Tenant "auto" was eating project templates.
-->

---

## How long it took

| Phase | Roughly |
|-------|---------|
| **Finding** the issue | ~2 minutes |
| **Tests + PR** | ~30 minutes |

**Takeaway:** Try it for buxfixing, it can save you a lot of time.
---
layout: image-right-wide-text
image: /assets/what-simon-wrote.png
backgroundSize: auto 45%
---

# Story 2 — Epic list page size

Another bug it one-shotted:

Ticket: the page-size control on the epic list didn’t change how many rows actually loaded.

```text [Prompt] {1|3-5|7}
on this component @frontend/src/components/molecules/TablePageButtons.tsx

I have this ticket:

"epic list, this selector (display page size) does not work"  

can you figure out whats wrong
```



<!--
Also good to figure out what Simon wrote
-->

---
layout: image-right-wide-text
image: /assets/my-lazy-ass.png
backgroundSize: auto 75%
---

## What was wrong

`usePagination({})` kept the hook default, `pageSize = 10` instead of using the user preference.

### Also useful for a refactor

```text [Prompt]
@frontend/src/.../EntityPermissionList.tsx:39-40 

Why do we need to pass the pageSize everywhere. 
As we do it basically everywhere, can't we get it inside 
usePaginationPageSizeSetting and make it overwriteable?
We could pass the userId or also get that one inside as it should 
only be used inside a page context...
```

One shotted as well, just had to fix a lint issue.

<!--
Just tell it: Change every instance of X into Y
-->

---
layout: image-right-wide-text
image: /assets/knowledge-base.png
backgroundSize: 100% auto
---
# Story 3 — Knowledge base (Hard)

A brand new feature for storywise, interacting with Confluence through APIs.

Requirements:

- Brand new Page + Layout
- Page Tree Component on the left
- Editor on the right
- Import/Export functionality from/to Confluence

Simon wanted to mock it, but ended up creating a draft PR for it.

---
layout: image-right-wide-text
image: /assets/simon-dump.png
backgroundSize: 100% auto
---

## Did it succeed?

Yes....kind of 😂

The code was mostly working, but it had some problems, like:

- Files were all over the place
- Using older/wrong components for things
- UI-wise it was ok, but could be improved a lot
- It was using `window.confirm` instead of our dialog pattern 😥

It required a lot of work still, but it did save me a bunch of time, especially on the API side.



<!--
Nothing revolutionary, but still a faily big scope.
-->

---

# How do we use it properly?

<v-clicks>

- **Bugs** — Yes! Always try it first (worst case you waste a few minutes)

  _Remember_: Give it at least a hint of where the problem might be (`@` syntax).
- **Refactors** — Yes

  Just telling it to `Refactor <this> to <that>` is usually enough, it will save you a lot of time.
- **New features** — Sure 🤷🏻‍♂️

  It will accomplish the task (small or big), but likely not the way you want it.

  _Tip_: <span v-mark="{ at: 4 }">Use Plan mode!</span> So you can see what components/libraries the model wants to use and you can iterate on it much more easily.
</v-clicks>

<!--
Explain what Plan mode is
-->

---
layout: image-right-wide-text
image: /assets/ie.png
backgroundSize: 100% auto
---

## Other tips: Docs beat vague prompts

Giving the model **real material** (e.g. **Tenda AP** manuals / PDFs) works wonders. 


## Other tips: Use newer models

**Older or weaker models** on heavy work feel a bit like a brand new **Ember.js** project in 2025

It can work but please don't.

1 year ago in AI-time is ~5 years ago in conventional time.

<!--
Habits > unstructured prompting.
-->

---
layout: end
---

# Thanks!

<!--
Optional follow-up: licenses, pairing, internal guidelines — whatever fits the org.
-->
