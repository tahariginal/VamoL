# VamoL

Landing page for [Vamo](https://github.com/tahariginal/vamo), the football
portfolio app.

A single static page. No build step, no dependencies, no framework. Open
`index.html` in a browser and that is the site.

## What is here

```
index.html      the whole page: markup, styles and script inline
favicon.svg     brand dot on the ground colour
vercel.json     clean URLs, asset caching, basic security headers
assets/         four app screens (webp, 560px wide) plus the open-graph card
fonts/          Lato 400/700/900, subset and self-hosted
```

Total payload is about 300 KB, and every byte is served from this origin. No
CDN, no font service, no analytics, nothing third-party.

## Type

Lato, self-hosted. Black (900) for display, Regular for body, with the system
mono stack for labels, data and anything numeric.

The three weights are subset to the characters this page actually uses, which
takes them from roughly 120 KB each as TTF down to about 17 KB each as woff2,
50 KB for the set. Regenerate with `pyftsubset` if the copy ever needs
characters outside basic latin. Lato is licensed under the SIL Open Font
License, so shipping it in this repo is fine.

Do not swap this for a Google Fonts link. Self-hosting is why the page has no
third-party requests and no layout shift on load.

## The palette is not decorative

Both colours come from the app's own `tailwind.config.js`, not from this repo:

| Token | Value | Role |
|---|---|---|
| ground | `#0a0c0b` | the near-black the whole product sits on |
| brand | `#16c172` | the single green accent |

The page is deliberately dark only. The ground is half the brand, so a light
mode would be a different product. If you change either value, change it in the
app first and copy it here, never the other way round.

## The argument the page makes

It leads with the portfolio, then explains matches as how the record gets
filled. That ordering is the product thesis and it is not cosmetic: a
match-finding app is a utility, a football identity layer is infrastructure that
academies and clubs can use. Any edit that puts discovery first is a regression.

Two sections exist to stay honest, and should not be quietly dropped:

- **"Goals, assists, ratings: never recorded."** There is no per-player
  performance data anywhere in the schema. The page says so.
- **"Built, not launched."** No store release, no paying venue, no revenue.

## The background is a pitch, not a grid

The hero ground is an actual football pitch drawn to regulation proportions,
105 by 68 metres, as inline SVG: touchlines, halfway line, centre circle at
9.15m, penalty areas, six-yard boxes, penalty spots and arcs, corner arcs. It is
laid flat with `rotateX` and tilts a few degrees as you scroll.

It replaced a generic perspective grid with animated light rays. That version
read as retro-wave wallpaper, it was brighter than the headline sitting on it,
and the rays looked like scratches rather than light. Real pitch geometry is
quieter, and it belongs to the subject.

Two things to watch if you edit it. The far touchline has to stay inside the
transparent part of the mask, otherwise it reads as a hard horizontal seam
across the page. And any full-width gradient layer needs its own bottom fade,
because a radial gradient anchored at the bottom of a fixed-height box gets cut
off at that edge and draws a visible line.

## The pipeline rig

The pinned diagram beside the three steps is the same pitch blueprint as the
hero, viewed flat, with fourteen player marks laid out as a real 7 v 7: keeper,
two at the back, three across the middle, one up top, mirrored and staggered so
it reads as two teams rather than a symmetrical diagram.

It fills as you scroll. Five players at step 01, eleven at 02, all fourteen at
03, where the geofence ring goes solid and the ledger ticks from 5 to 6. That
tick is the argument the whole page makes: arriving is what moves the number.

The active step is computed from scroll position, picking whichever step's
midpoint is nearest the middle of the viewport. It used to use an
IntersectionObserver with a thin `rootMargin` band, which silently failed:
an element taller than the band enters once and never emits again, so steps 2
and 3 never activated. Do not reintroduce that pattern here.

## Interaction

Scroll drives the page. The hero card sinks and scales as it hands off, the
pipeline pins while its three steps advance a live pitch diagram, the device
gallery parallaxes, and counters run when the passport enters view.

Everything is transform and opacity only, rAF-throttled, with passive
listeners. `prefers-reduced-motion` disables all of it and forces every
revealed element visible.

## Deploying

Static, so any host works. On Vercel there is no build command and no output
directory to set; import the repo and it serves the root as-is.

## Screens

The four screenshots are cropped from `docs/screenshots/vamo-screens.png` in the
app repo. When the UI changes, re-crop from that file rather than taking new
shots at a different size, so the gallery stays consistent.
