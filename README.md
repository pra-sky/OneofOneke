# One Of One KE

A mobile-first demo storefront for **One Of One KE**, a Y2K "pixie retro diary" vintage thrift shop in Nairobi that sells one-of-one pieces in timed drops.

This is a single-file, zero-build static site — a portfolio piece for pitching to thrift sellers, not a production e-commerce platform.

## What's in the box

- **[index.html](index.html)** — the entire site: HTML, CSS (in a `<style>` tag) and JS (in a `<script>` tag). Nothing else is required to run it.
- **products/** — real product photos. Any entry in the product data can point at a file here instead of a generated placeholder.
- No React, no npm, no build step, no backend. Just open the file in a browser.

## Features

- Intro screen with an animated pink arched door ("click to enter" / "skip")
- Drop countdown (days/hours/min/sec) on a retro LCD panel, flips to "DROP IS LIVE"
- Product grid styled like notebook paper (punched holes, pink margin line, slight tilt), 2 columns on mobile up to 4 on desktop
- Category tabs (tops / denim / accessories / bags) + an "available only" toggle
- "SOLD" stamp + disabled ordering for sold-out one-of-ones
- Product detail modal (retro window frame) with an image gallery, measurements and condition notes
- Ordering via a flip-phone illustration whose screen shows the item + price, with an "Order now" button that opens WhatsApp with a prefilled message
- A pixel-art mascot (drawn from inline SVG rects, no image files) with speech bubbles that change by section
- All decorative images — placeholders, hero art, logo accents — are generated inline as SVG data URIs; real photos automatically fall back to a placeholder if they fail to load

No cart, payments, accounts or backend — those are out of scope for this demo (M-Pesa/Daraja is a planned v2).

## Running it

Just open `index.html` in a browser. No server, no install.

- **Double-click the file** from a file manager (most reliable).
- **On WSL**, a Windows browser can't resolve a Linux path directly — either open it from Windows Explorer via `\\wsl.localhost\<distro>\path\to\index.html`, or use `file:///home/...` from a browser running inside WSL itself.

## Configuration

Edit the `CONFIG` block near the top of the `<script>` tag in `index.html`:

```js
const CONFIG = {
  SHOP_NAME: "One Of One KE",
  DROP_DATE: "2026-12-05T18:00:00+03:00", // ISO date/time, EAT (UTC+3)
  WHATSAPP_NUMBER: "254700000000",        // digits only, country code, no + or spaces
  INSTAGRAM_URL: "https://instagram.com/oneofoneke",
  TIKTOK_URL: "https://tiktok.com/@oneofoneke"
};
```

Mascot speech bubble text lives right below it, in `MASCOT_MESSAGES` (`hero`, `sold`, `footer`).

## Adding or editing products

Products are a plain JS array (`PRODUCTS`) a bit further down the same `<script>` tag. Each one is a single object:

```js
{
  id: "p13",
  name: "New Piece",
  category: "tops",       // tops | denim | accessories | bags
  price: 2000,
  size: "M",
  sold: false,
  measurements: "...",
  condition: "...",
  images: [makePlaceholder("New Piece", "#F9B8D0", "#CDB4FF"), makePlaceholder("Detail", "#CDB4FF", "#F9B8D0")]
}
```

Adding a product means adding one object to the array. Set `sold: true` to grey it out, stamp it "SOLD" and disable ordering.

## Swapping in real photos

1. Put your image files in a `products/` folder next to `index.html`.
2. In a product's `images` array, replace a `makePlaceholder(...)` call with the file path, e.g. `"products/baby-tee-1.jpg"`.
3. The first entry in `images` is the grid-card cover photo; the rest show up in the detail modal's gallery.

If a real photo fails to load (wrong path, not yet uploaded), the `<img onerror>` handler automatically falls back to the generated placeholder — the page never shows a broken image icon.

## Deploying

Any static host works, no build step:

- **Netlify Drop** — drag the project folder onto [app.netlify.com/drop](https://app.netlify.com/drop).
- **Vercel** — `vercel` CLI in this folder, or drag-and-drop at vercel.com/new.
- **GitHub Pages** — push the repo, then in Settings → Pages set the source to the branch root.

## Credits

Built by Prasky.
