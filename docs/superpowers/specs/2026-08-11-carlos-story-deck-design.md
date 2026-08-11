# The Story of CARLOS — presentation deck at story.carlosframework.com

2026-08-11. Approved direction: Keynote-noir slide deck, dual-use (Paul
presents it live; visitors click through it self-guided), deployed as a
static CARLOS app.

## What it is

A single self-contained `index.html` telling the story of building CARLOS:
from a Rails app on Fly, through Eleven Messenger, to the 11factor
philosophy, the proof apps, the Rastrillo framework, the Carloku platform,
Amadan — and the Raspberry Pi reveal.

One file, no build step, no external requests: inline CSS and JS, system
font stack, any icons as inline SVG. Works offline once loaded (it may be
presented from a pocket Pi over venue Wi-Fi; nothing may depend on a CDN).

## Format and interaction

- One beat per slide, no per-slide build animations. Robust live, readable
  self-guided.
- Navigation: → / ← arrows, space (next), click or tap on the right/left
  third of the screen, swipe on touch devices.
- Every slide hash-addressable: `#12` deep-links and survives reload.
  Landing with no hash shows slide 1.
- Quiet `n / N` counter bottom-right. No progress dots, no menu.
- Responsive: type scales with viewport (clamp/vw); the deck must read well
  on a phone and project well full-screen.

## Visual direction: Keynote noir + prompt cards

- Near-black stage (`#0b0b0f` territory), huge type, high contrast,
  generous whitespace. One accent color used sparingly.
- The two pivotal prompts (Jun 27, Jul 9) render as monospace terminal
  cards — visually distinct artifacts, quoted verbatim.
- "One more thing…" interstitials: bare, centered, oversized text on an
  empty stage. They are the rhythm of the deck.
- Product beats share a consistent slide shape: product name huge, live URL,
  one-line identity.

## Slide map (~27 slides, in order)

1. **Title** — The Story of CARLOS.
2. **lpay.fly.dev** — a Rails app on Fly; 10 seconds to wake from
   hibernation, but it worked: the kid made a few €100 in sales with a
   Stripe terminal selling 3D prints. Score!
3. **The iMessage problem** — same kid's group chat excluded Android
   friends. After the lpay success: "I'm starting to think you could do the
   chat app after all."
4. **Jun 27 — the first prompt** (terminal card, verbatim): kid-friendly
   chat app, no login, secret link + screen name, single server, sqlite?,
   E2EE so a compromised server yields nothing.
5. **Three weeks later** — elevenmessenger.com.
6. **The pivot** — people kept focusing on the chat app; the interesting
   session was July 9.
7. **Jul 9 — the second prompt** (terminal card, verbatim): "How do we make
   deploying home and the oauth broker apps essentially infinitely
   scaleable?"
8. **Out of that** — 11factor.org and carlosframework.com.
9. **Operations in the open** — status.elevenmessenger.com: open ledgers —
   an append-only, verifiable record of what the service is actually doing.
   A direct consequence of the 11factor philosophy.
10. **The flip side** — when highly-available apps are this cheap and this
    simple to run, on-prem stops being a compromise: your hardware, your
    data, same availability. (Plants the seed the Pi reveal pays off.)
11. **The skeptics** — people didn't see the point; the answer is to prove
    it across a wide variety of app shapes: E2EE, single-process,
    single-database, multi-region, multi-platform.
12. **Keymail** — keymail.dev. Email, but secure.
13. **Suddenly feasible** — secure, affordable alternatives to SaaS we were
    spending real money on.
14. **Slopbox** — slopbox.cloud. Like OG Dropbox, no upsells. Simple, fast,
    encrypted.
15. **Woodstar** — woodstar.app. Mastodon too nerdy, BlueSky has URLs
    instead of handles: each profile its own database, feels like OG
    Twitter.
16. **Meanwhile** — Tito being rebuilt in this shape; Vicky (product
    manager) unlocking unprecedented productivity building UX-first on a
    resilient, dynamic stack that allows extremely fast iteration.
17. **Kass** — kass.training. Gym sessions logged and shared securely with
    a personal trainer, instead of handing the data to a US firm.
18. **Extraction time** — with examples in hand: a web framework. Rastrillo
    is born — rastrillo.org.
19. **Seapointish** — E2EE self-hostable financial reporting (seapoint.co's
    founders make a good demo audience). And thinking starts on porting
    Vito — vi.to.
20. **The tally** — Eleven built in three weeks; Keymail, Slopbox,
    Woodstar, Kass the week after. A philosophy for using LLMs responsibly,
    an architecture built on it, a reusable web framework on top. Apps
    hosted for pennies per month — cloud or on-prem, the architecture
    doesn't mind.
21. *"But there's one more thing…"* (interstitial)
22. **The platform question** — instead of a new AWS account per app: a
    properly global deployment platform. Deploy in seconds. BYOB — bring
    your own box. Instances placed anywhere in the world, per tenant rather
    than per app.
23. **carloku.com** — the CARLOS platform flagship. Inspired by Heroku, but
    all the things Heroku wasn't: true multi-region; host apps yourself and
    have Carloku orchestrate; or run your own copy of the whole platform.
24. *"One more thing…"* (interstitial)
25. **Amadan** — amadan.net. Doesn't GitHub kinda suck these days? Bloated,
    slow, AI bolted on without getting better. Amadan: blazing fast, cloud
    or self-hosted first-class, E2EE optional, similar concepts with fresh
    ideas (multiple conversations around a single branch — shock, horror).
26. *"One more thing…"* (interstitial)
27. **The Pi reveal** — all of the demos — Eleven, Keymail, Slopbox,
    Woodstar, Amadan — every demo account instance has been running on a
    Raspberry Pi in Paul's pocket this whole time. Then: **"That's actually
    it."** — with the roll-call of links.

(Exact slide count may drift ±2 as copy is written; the beats and their
order are the contract. The final "That's actually it." may be its own
closing slide if the reveal reads better bare.)

## Source and deploy

- Source: this repo, `carlosframework/story`. `index.html` at the root.
- Platform: account `carlos` on the flagship console
  (`https://console.carloku.com`), app `story`.
- Host: `story.carlosframework.com` — DNS A record → 99.81.104.219 added
  in DNSimple 2026-08-11 (manual; the platform's SSM DNSimple token is
  scoped to account 74881, which does not hold the carlosframework.com
  zone).
- Deploy path, all through the CLI per the no-infra rule:
  1. Ensure account `carlos` exists (`carlos accounts create -name carlos`
     through the flagship console if missing).
  2. `carlos apps create -name story -account carlos`.
  3. Route/instance for the host, then
     `carlos deploy -kind static -host story.carlosframework.com`.
  4. Verify via the `X-Carlos-Version` header on the live URL
     (`curl -sI --resolve` until DNS propagates everywhere).

## Out of scope (v1)

- Speaker notes / presenter view.
- Analytics.
- Screenshots or imagery — the deck is type-only in v1; product screenshots
  are a possible follow-up.
- A separate scroll/essay mode.

## Testing

- Static artifact: correctness is visual plus mechanical. Mechanical
  checks: every slide reachable by arrow-keying to the end; hash deep-link
  to a middle slide lands on it; no external network requests (verifiable
  by grepping for `http` in the artifact's src/href attributes beyond the
  product links, which are plain anchors).
- Deploy verification: `X-Carlos-Version` reports the shipped build over
  2xx on the live host.
