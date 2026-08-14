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

(2026-08-14: the deck leads with the Tito hook — slide 2, right after the
title: Tito stands to save $7,000/month in AWS fees and ends up with a more
robust system; then the story rewinds to lpay. After the Carloku demo the
hook pays off: Tito as the first third-party CARLOS deployment, then the
itemized "what CARLOS replaces" slide — load balancers, NAT gateways,
managed DBs/caches, orchestration, log stacks, deploy pipelines → two
boxes, one binary and a bucket, ~$60/month of EC2. 32 slides.)

(2026-08-12: a third prompt card — the Amadan prompt, quoted verbatim —
sits between the final "One more thing…" interstitial and the Amadan
slide, bringing the deck to 29 slides.)

(Exact slide count may drift ±2 as copy is written; the beats and their
order are the contract. The final "That's actually it." may be its own
closing slide if the reveal reads better bare.)

## Live site panels (added 2026-08-12, Paul's request)

Slides that present a site gain a build step: the first Next press slides a
pseudo-browser in from the right (rounded sheet, dot-traffic-lights header,
URL pill, live iframe below); the second press advances. Back closes the
panel first; backing into a paneled slide shows the panel already open
(Keynote build semantics). Hash addressing stays `#N` — panel state is not
in the URL.

- Paneled slides: Eleven, 11factor.org, status.elevenmessenger.com,
  Keymail, Slopbox, Woodstar, Kass, Rastrillo, Carloku, Amadan (declared
  via `data-site` on the section). The seapoint/vi.to slide and the closing
  roll-call get no panel.
- `data-site` is a space-separated list; each Next press steps to the next
  site in the same panel before advancing, Back steps backward through
  them. Amadan has two steps: amadan.net, then amadan.net/amadan/amadan —
  Amadan hosting its own source (2026-08-12, Paul).
- The closing roll-call lists 11 projects — go.tito.io joins between
  woodstar.app and kass.training (2026-08-12, Paul).
- The iframe pre-loads when its slide is entered (before the panel opens),
  one shared panel/iframe for the whole deck.
- **Offline constraint, amended:** the deck's own assets remain fully
  self-contained; site panels are the one deliberate runtime exception —
  live iframes that need network and degrade to chrome + URL pill when
  unreachable. The mechanical no-external-asset check is `src="` (attribute
  form), which stays at zero; iframe src is assigned only at runtime.
- **Keymail exception:** keymail.dev sends `frame-ancestors 'none'`, so its
  panel renders a built-in sample email page instead (inline `<template
  id="mock-keymail">` injected via iframe `srcdoc` — no network involved),
  with the URL pill still reading keymail.dev. Mock content is content, so
  it may use its own light email-client colors and a lock emoji, exempt
  from the deck-chrome palette rule like real framed sites are.

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
