# [Slidev](https://github.com/slidevjs/slidev) decks

## Setup

- `pnpm install`
- Develop: hub with `pnpm dev`, or a nested deck with `pnpm dev:tutorial` / `pnpm dev:cursor-ad`

## Deploy (pick one deck)

Each nested-deck build writes a **standalone** site to the repo-root **`dist/`** (base `/`). Slidev resolves `-o dist` relative to the deck folder, so we use **`-o ../../dist`** so Vercel’s **Output Directory `dist`** matches the real output (see [Vercel “Missing public directory”](https://vercel.com/docs/errors/error-list#missing-public-directory)).

| Script | Deck |
|--------|------|
| `pnpm build` | Default for **Vercel** ([`vercel.json`](./vercel.json) runs this)|
| `pnpm build:tutorial` | Tutorial ([`presentations/tutorial/slides.md`](./presentations/tutorial/slides.md)) |
| `pnpm build:cursor-ad` | Cursor talk ([`presentations/cursor-ad/slides.md`](./presentations/cursor-ad/slides.md)) |

To deploy a different deck, either change the **`build`** script in [`package.json`](./package.json) or override the build command in the Vercel project settings (e.g. `pnpm run build:tutorial`).

## Layout

- **Hub** — root [`slides.md`](./slides.md) (optional; links assume separate routes if you host multiple decks).
- **Shared** — [`components/`](./components/) and [`layouts/`](./layouts/) at the repo root; nested decks use `addons: [..]` in frontmatter.
- **Per deck** — under [`presentations/<name>/`](./presentations/): `slides.md`, optional `pages/` and `snippets/`, static files in `public/` (e.g. `public/assets/` for `/assets/...` in frontmatter).

## Export (PDF, etc.)

- `pnpm export:tutorial` / `pnpm export:cursor-ad`

### Vercel troubleshooting

If you see **“No Output Directory named dist”** after a successful build log:

1. **Output path** — Nested entries must emit **`dist/` at the repo root** (this repo uses `-o ../../dist` in `build:*`). If output were only under `presentations/.../dist`, Vercel would not find it.
2. **Project settings** ([docs](https://vercel.com/docs/deployments/configure-a-build#output-directory)) — **Root Directory** should be the repo root (unless this app lives in a subfolder). **Output Directory** should be **`dist`** (or turn overrides off so [`vercel.json`](./vercel.json) applies).
3. **`framework`: `null`** in `vercel.json` avoids the “Other” preset default of serving **`public`** or **`.`** instead of your Slidev output.
4. Run **`pnpm run build`** locally and confirm **`dist/index.html`** exists at the **repository root**.

`dist/` stays in [`.gitignore`](./.gitignore); it is produced on each build and is not required in Git.

More detail: [Slidev docs](https://sli.dev/).
