# [Slidev](https://github.com/slidevjs/slidev) decks

## Setup

- `pnpm install`
- Develop: hub with `pnpm dev`, or a nested deck with `pnpm dev:tutorial` / `pnpm dev:cursor-ad`

## Deploy (pick one deck)

Each `build:*` writes a **standalone** site to `dist/` (root base `/`). Choose which deck to ship:

| Script | Deck |
|--------|------|
| `pnpm build:tutorial` | Tutorial ([`presentations/tutorial/slides.md`](./presentations/tutorial/slides.md)) |
| `pnpm build:cursor-ad` | Cursor talk ([`presentations/cursor-ad/slides.md`](./presentations/cursor-ad/slides.md)) |

**Vercel / Netlify:** set the build command to `pnpm run build:tutorial` or `pnpm run build:cursor-ad` (defaults in [`vercel.json`](./vercel.json) and [`netlify.toml`](./netlify.toml) point at `build:tutorial`; change there when you switch decks).

## Layout

- **Hub** — root [`slides.md`](./slides.md) (optional; links assume separate routes if you host multiple decks).
- **Shared** — [`components/`](./components/) and [`layouts/`](./layouts/) at the repo root; nested decks use `addons: [..]` in frontmatter.
- **Per deck** — under [`presentations/<name>/`](./presentations/): `slides.md`, optional `pages/` and `snippets/`, static files in `public/` (e.g. `public/assets/` for `/assets/...` in frontmatter).

## Export (PDF, etc.)

- `pnpm export:tutorial` / `pnpm export:cursor-ad`

More detail: [Slidev docs](https://sli.dev/).
