# z33z notes on a set

Personal DJ blog by Kat (z33z). Plain static HTML — no build step, no framework.
Live at https://z33z.online via Vercel: pushes to `main` deploy production,
pushes to any other branch create a preview deployment (preview URLs require
Vercel login; the custom domain is public).

## Structure

- `index.html` — all posts on one page, newest first
- `lore.html` — about page
- `style.css` — single stylesheet; mobile adjustments live in `@media (max-width: 600px)`
- `img/` — web-compressed JPEGs actually used by pages
- root `*.png` / `umami-sf.jpeg` — original full-size photos, kept in git but
  excluded from deploys via `.vercelignore`

## Voice

Posts are lowercase, casual, stream-of-thought. NEVER rewrite, correct, or
"clean up" Kat's words — spelling quirks, slang, and emoji are intentional.
Formatting into HTML is fine; editing prose is not.

## Adding a new post

Insert as the first `.post` in `index.html` (posts are newest-first), following
this exact shape:

```html
<div class="post" id="kebab-slug">
  <div class="post-meta">month dd yyyy — city, st</div>
  <h2 class="post-title">the ___ set</h2>
  first paragraph...
  <br /><br />
  next paragraph...
</div>
```

Conventions:

- Paragraph breaks are `<br /><br />` on their own line — not `<p>` tags.
- `post-meta` uses an em dash (`—`) between date and place, all lowercase.
- Add a matching nav link inside `#link-bar` under "notes", keeping the links
  in the same order as the posts: `<a href="#kebab-slug">the ___ set</a>`
- Photos: compress the original to ~1000px JPEG before embedding:
  `sips -Z 1000 -s format jpeg -s formatOptions 78 original.png --out img/name.jpg`
  then embed with `<img loading="lazy" src="img/name.jpg" alt="..." />`.
  Keep the original at repo root (`.vercelignore` already excludes it from deploys).
- SoundCloud player + credit line:

```html
<iframe
  width="100%"
  height="166"
  scrolling="no"
  frameborder="no"
  loading="lazy"
  allow="autoplay"
  src="https://w.soundcloud.com/player/?url=https%3A//api.soundcloud.com/tracks/TRACK_ID&color=%23ff5500&auto_play=false"
></iframe>
<div class="sc-credit">
  <a href="https://soundcloud.com/z33z-music" target="_blank">z33z</a>
  ·
  <a href="https://soundcloud.com/z33z-music/SET-SLUG" target="_blank">SET TITLE</a>
</div>
```

## Draft → preview → publish workflow

When Kat sends a new post (usually from her phone):

1. Format it per the conventions above on a new branch (`post/<slug>`), push.
2. Give her the Vercel preview URL for the branch so she can check it on her phone.
3. Only merge to `main` (which publishes to z33z.online) after she approves.

## Verifying changes

Check layout at true phone width (390px) before shipping. Note: plain headless
Chrome silently clamps windows to ~500px wide — use puppeteer-core with
`page.setViewport({ width: 390, isMobile: true })` and compare
`document.documentElement.scrollWidth` to `innerWidth` to catch horizontal
overflow.
