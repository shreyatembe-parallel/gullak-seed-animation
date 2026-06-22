# Gullak's Message — Envelope Reveal

A self-contained prototype of the postcard a Gullak user receives when the app
launches during **Golden Thank You Week**. A dummy app homepage sits in the
background; a dark overlay rises with a gold envelope that the user taps to open,
revealing Gullak's thank-you postcard.

This is the **"Gullak's Message"** variant: the postcard's message and sign-off
("– The 'Gullak' team") are baked into the card art, so the **only dynamic field
is the recipient name ("To …")**.

## Run locally

```bash
node server.js
# → serving on http://localhost:4601
```

Any static file server works too — it's plain HTML/CSS/JS with no build step.

## Animation sequence

1. **"To" modal** — a one-field modal captures the recipient name (demo only).
2. **Homepage** — the bare dummy homepage (`dummy-bg.jpg`) shows for a beat.
3. **Overlay rises** — a black 80% scrim + blur fades in with a red glow behind
   a closed gold envelope, the heading, and a **"Tap to open"** CTA.
4. **Tap to open** — the flap swings open and the postcard slides up out of the
   pocket.
5. **Settle** — the envelope shrinks and drops lower; the postcard enlarges,
   tilts slightly, and overlaps the envelope.
6. **Final CTA** — **"Explore 'Golden Thank You Week'"** fades in.

Tap the final CTA to replay with a new name.

## Integrating into the real app

The demo modal is for previewing only. In production the recipient is already
known, so just:

```js
renderPostcard({ to: 'Anjali didi' });  // paints "To Anjali didi," on the card
playReveal();                            // runs the tap-to-open sequence
```

Both functions live in the inline `<script>` in `index.html`. Set the phone to
`data-phase="idle"`, call `renderPostcard(...)`, then call `playReveal()` on tap.

## Files

| File | Purpose |
|------|---------|
| `index.html` | Markup, styles, and animation logic (everything is here) |
| `gullak-seed-postcard.png` | Postcard art with the fixed message + sign-off baked in |
| `dummy-bg.jpg` | Stand-in app homepage for the background |
| `Back.svg` `Middle.svg` `Bottom.svg` | Envelope body layers |
| `Top-Close.svg` `Top-Open.svg` | Envelope flap (closed front / open back) |
| `server.js` | Tiny zero-dependency static server for local preview |
| `.nojekyll` | Tells GitHub Pages to serve files as-is (no Jekyll processing) |

> **Note:** asset filenames are case-sensitive on GitHub Pages. The references in
> `index.html` match the casing of the files in this folder exactly — keep them in
> sync if you rename anything.

## Tuning knobs (in `index.html`)

- **`.pc-to`** — position/size of the dynamic "To" line over the card.
- **`--env-top` / `--env-left` / `--env-scale`** — envelope placement.
- **`.card` phase rules** (`emerging` / `center` / `done`) — how the postcard rises,
  grows, and tilts.
- **`OPEN_PAUSE`** and the `setTimeout` offsets in `playReveal()` — reveal timing.
- **`.scrim`** (black 80% + blur) and **`.glow`** (`#CB002F` at 40%) — the overlay look.
