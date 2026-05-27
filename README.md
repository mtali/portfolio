# Portfolio

Personal portfolio for Emmanuel Mtali, Lead Mobile Engineer.

**Live:** https://mtali.github.io/portfolio/
**Analytics:** https://mtali.goatcounter.com

## What it is

A single static HTML page. No build step, no framework, no dependencies. All the
CSS and JavaScript live inline inside `index.html`. You edit the file, push to
`main`, and GitHub Pages publishes it.

Design is a dark navy + teal theme inspired by Brittany Chiang's v4 portfolio
(numbered nav, fixed social rails, numbered section headers, alternating featured
projects, experience tabs, noteworthy-projects grid).

## File structure

```
portfolio/
├── index.html              # the entire site: markup + CSS + JS, all inline
├── README.md               # this file
└── assets/
    ├── avatar.png          # profile photo (hero, About section, favicon, OG image)
    ├── starapp.png         # app icon (not currently shown, kept for reference)
    ├── siteapp.png         # app icon (not currently shown, kept for reference)
    ├── gopods-icon.png     # app icon (not currently shown, kept for reference)
    └── screens/
        ├── star-demo.mp4   # StarApp walkthrough video (featured project)
        ├── site-home.png   # SiteApp screenshot
        ├── site-survey.png # SiteApp screenshot
        ├── gopods.gif      # GoPods demo
        └── dstv-ussd.png   # MyDStv USSD screen (not currently shown, kept for reference)
```

## Preview locally

Just open the file in a browser:

```bash
open index.html
```

Or serve it (so relative asset paths behave exactly like production):

```bash
python3 -m http.server 8000
# then visit http://localhost:8000
```

## How to edit content

Everything is in `index.html`. The sections, in order:

| Section | Where to look | What's there |
|---|---|---|
| Nav | `<nav class="bar">` | logo, numbered links, Resume button |
| Hero | `<section class="hero">` | name, tagline, intro paragraph, CTA |
| About | `id="about"` | bio paragraphs + the `.tech-list` of technologies |
| Experience | `id="work"` | the tab buttons + each `.panel` (one per company) |
| Projects | `id="projects"` | the featured `.project` blocks (StarApp, SiteApp, GoPods) |
| Other | the `.grid` of `.ncard` | GitHub repos + the Medium article |
| Contact | `id="contact"` | closing pitch + email CTA |

To update text, edit it in place. To **add an experience tab**, copy one `<button data-tab="tN">` in `.tabs` and its matching `<div class="panel" id="tN">`, and give it a new unique `tN` id.

To **add a featured project**, copy a `.project` block. Add the class `rev` to alternate it to the mirrored (right-aligned) layout.

## Replacing the project video or screenshots

The StarApp video is a web-optimized MP4 made from a phone screen recording with
`ffmpeg`. To regenerate it from a new recording:

```bash
# H.264, 360px wide, 30fps, no audio, fast-start for web streaming.
# Produces a small (~1-2 MB), sharp, autoplay-friendly clip.
ffmpeg -i NEW_RECORDING.mp4 -an \
  -vf "fps=30,scale=360:-2:flags=lanczos" \
  -c:v libx264 -crf 28 -preset slow -pix_fmt yuv420p -movflags +faststart \
  assets/screens/star-demo.mp4
```

Notes:
- Keep it muted. The `<video>` tag uses `autoplay muted loop playsinline`, which
  only autoplays if there is no audio track (`-an` strips it).
- Lower `-crf` (e.g. 24) for higher quality and a bigger file; higher (e.g. 30)
  for smaller. 28 is a good balance.
- For a static screenshot instead, just drop a PNG into `assets/screens/` and
  point the `<img>` at it.

To **trim** a recording to a segment before encoding, add `-ss START -t SECONDS`
before `-i`, e.g. `-ss 44 -t 15` for 15 seconds starting at 0:44.

## Analytics (GoatCounter)

Pageviews and click events are tracked with GoatCounter (free, privacy-friendly,
no cookie banner). The snippet is at the bottom of `index.html`:

```html
<script data-goatcounter="https://mtali.goatcounter.com/count"
        async src="//gc.zgo.at/count.js"></script>
```

Click events use a `data-goatcounter-click="name"` attribute on links. Current
events: `cv`, `email`, `github`, `linkedin`, `play-starapp`, `play-siteapp`,
`play-gopods`, `github-gopods`, `repo-bolt`, `repo-stock`, `repo-tigopesa`,
`repo-monsterdex`, `writing-medium`.

To track a new link, add the attribute:

```html
<a data-goatcounter-click="some-name" href="...">Link</a>
```

Links sharing the same name aggregate into one event (that's why every email
link uses `email`). Your own visits are not counted while you're logged into
GoatCounter in that browser. View stats at https://mtali.goatcounter.com.

## Deployment

GitHub Pages serves from the `main` branch root. Any push to `main` redeploys
(usually live within a minute).

```bash
git add -A
git commit -m "Update portfolio"
git push origin main
```

Verify it went live:

```bash
curl -s "https://mtali.github.io/portfolio/?cb=$(date +%s)" | grep -q "I build Android apps" && echo "live"
```

## Design notes

- **Fonts:** Inter (body) and JetBrains Mono (numbers, labels, accents), loaded
  from Google Fonts.
- **Colors** are CSS variables in `:root` at the top of the `<style>` block:
  `--navy #0a192f`, `--light-navy #112240`, `--green #64ffda` (the teal accent),
  and the slate text shades. Change the theme there.
- **Section numbering** is automatic via a CSS counter on `.numbered`. Reordering
  sections renumbers them; no manual edits needed.
- **Motion:** sections fade in on scroll (`.reveal` + IntersectionObserver). The
  nav hides on scroll-down and reappears on scroll-up. Experience tabs swap via a
  small click handler. All in the `<script>` at the bottom.

### Writing style (keep these when editing copy)

- No em-dashes. Use periods, commas, or restructure.
- First person ("I built", "I lead").
- Numbers up front, short sentences, vary the rhythm.
- Be honest: Android-focused, "exploring Kotlin Multiplatform" (don't claim iOS
  or Flutter as shipped).
