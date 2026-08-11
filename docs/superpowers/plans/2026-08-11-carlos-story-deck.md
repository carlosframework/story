# The Story of CARLOS Deck Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** A single self-contained `index.html` slide deck telling the CARLOS
origin story, deployed as a static CARLOS app at story.carlosframework.com.

**Architecture:** One HTML file: inline CSS design system (Keynote noir),
inline vanilla-JS slide engine (hash-addressable, keyboard/click/swipe), 28
`<section class="slide">` beats. No build step, no external requests. Deployed
via `carlos deploy -kind static` to app `story` in account `carlos` on the
flagship console.

**Tech Stack:** Hand-written HTML/CSS/JS. `carlos` CLI for deploy. No
dependencies, no framework, no bundler.

## Global Constraints

- Repo: `~/github.com/carlosframework/story`. The artifact is `index.html` at
  the repo root; specs/plans live under `docs/superpowers/`.
- ZERO external requests: no `src=` attributes, no `@import`, no webfonts, no
  `fetch(`. The only `http` strings allowed are product-link `href`s (plain
  anchors) and the favicon `data:` URI. It may be presented offline from a Pi.
- System font stack only. Mono stack for prompt cards:
  `ui-monospace,"SF Mono",SFMono-Regular,Menlo,Consolas,monospace`.
- Palette (the whole palette — introduce no other colors):
  bg `#0b0b0f`, panel `#111118`, border `#26262f`, ink `#f4f4f2`,
  muted `#8f8f9a`, faint `#55555f`, accent `#ffb454`, term text `#d8d8e2`.
- Exactly 28 slides; slide N is deep-linkable as `#N` (1-based).
- The two prompts are quoted VERBATIM as given in the spec — do not edit
  their wording, spelling, or punctuation ("scaleable" stays).
- Deploy identity: console `https://console.carloku.com`, account `carlos`,
  app `story`, host `story.carlosframework.com`. Flagship box IP for
  `--resolve` checks: `99.81.104.219`.
- Every commit lands with `Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>`.

---

### Task 1: Deck scaffold — design system, slide engine, slides 1–2

**Files:**
- Create: `index.html`

**Interfaces:**
- Produces: the `<main id="deck">` container Task 2 inserts slides into
  (new slides go before `</main>`), the CSS classes Task 2's slides use
  (`slide`, `kicker`, `big`, `interstitial`, `product`, `name`, `url`,
  `term`, `term-title`, `dot`, `ps1`, `roll`), and the slide engine, which
  needs no changes when slides are added (it counts `.slide` at load).

- [ ] **Step 1: Write `index.html`** with exactly this content:

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>The Story of CARLOS</title>
<meta name="description" content="From a 10-second cold start to a global deployment platform: how CARLOS happened.">
<link rel="icon" href="data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 100 100'%3E%3Crect width='100' height='100' rx='18' fill='%230b0b0f'/%3E%3Ctext x='50' y='68' font-size='58' text-anchor='middle' fill='%23ffb454' font-family='monospace'%3EC%3C/text%3E%3C/svg%3E">
<style>
:root{
  --bg:#0b0b0f; --panel:#111118; --border:#26262f; --ink:#f4f4f2;
  --muted:#8f8f9a; --faint:#55555f; --accent:#ffb454; --term:#d8d8e2;
  --mono:ui-monospace,"SF Mono",SFMono-Regular,Menlo,Consolas,monospace;
}
*{margin:0;padding:0;box-sizing:border-box}
html,body{height:100%}
body{background:var(--bg);color:var(--ink);overflow:hidden;
  font-family:system-ui,-apple-system,"Segoe UI",Roboto,sans-serif}
.slide{position:absolute;inset:0;display:none;flex-direction:column;
  justify-content:center;align-items:flex-start;padding:8vmin 10vmin;gap:3vmin}
.slide.current{display:flex;animation:appear .35s ease}
@keyframes appear{from{opacity:0;transform:translateY(1vmin)}to{opacity:1;transform:none}}
.kicker{font-size:clamp(13px,1.8vmin,20px);letter-spacing:.18em;
  text-transform:uppercase;color:var(--accent);font-weight:600}
h1{font-size:clamp(40px,9vmin,110px);line-height:1.02;letter-spacing:-.02em}
h2{font-size:clamp(28px,6vmin,80px);line-height:1.08;letter-spacing:-.02em;max-width:26ch}
p{font-size:clamp(17px,2.9vmin,32px);line-height:1.45;color:var(--muted);max-width:40ch}
p strong{color:var(--ink)}
.big{font-size:clamp(28px,5vmin,64px);color:var(--ink);font-weight:700;max-width:none}
blockquote p{font-size:clamp(20px,3.6vmin,44px);line-height:1.35;color:var(--ink);
  border-left:4px solid var(--accent);padding-left:2.5vmin;max-width:34ch}
cite{font-style:normal;color:var(--muted);font-size:clamp(14px,2vmin,22px)}
.interstitial{align-items:center;text-align:center}
.interstitial h2{max-width:none}
.product .name{font-size:clamp(42px,10vmin,128px)}
a{color:var(--accent);text-decoration:none}
a:hover{text-decoration:underline}
.url{font-family:var(--mono);font-size:clamp(15px,2.6vmin,30px)}
.term{background:var(--panel);border:1px solid var(--border);border-radius:12px;
  max-width:min(88ch,100%);box-shadow:0 2vmin 6vmin rgba(0,0,0,.5)}
.term header{display:flex;align-items:center;gap:.8vmin;
  padding:1.6vmin 2.4vmin;border-bottom:1px solid var(--border)}
.dot{width:1.4vmin;height:1.4vmin;min-width:8px;min-height:8px;
  border-radius:50%;background:var(--border)}
.term-title{margin-left:1.2vmin;color:var(--muted);
  font-family:var(--mono);font-size:clamp(12px,1.8vmin,18px)}
.term pre{white-space:pre-wrap;font-family:var(--mono);color:var(--term);
  font-size:clamp(14px,2.4vmin,26px);line-height:1.55;padding:3vmin}
.ps1{color:var(--accent)}
ul.roll{list-style:none;display:flex;flex-wrap:wrap;
  gap:1.2vmin 3vmin;font-family:var(--mono);font-size:clamp(15px,2.4vmin,26px)}
#counter{position:fixed;right:2.5vmin;bottom:2vmin;color:var(--faint);
  font-family:var(--mono);font-size:clamp(11px,1.6vmin,16px)}
</style>
<noscript><style>
body{overflow:auto}
.slide{position:static;display:flex;min-height:100vh}
#counter{display:none}
</style></noscript>
</head>
<body>
<main id="deck">

<section class="slide">
  <p class="kicker">carlosframework.com</p>
  <h1>The Story of&nbsp;CARLOS</h1>
  <p>From a ten-second cold start to a global deployment platform.</p>
  <p class="hint" style="color:var(--faint)">&rarr; or space to begin</p>
</section>

<section class="slide">
  <p class="kicker">Where it started</p>
  <h2>lpay.fly.dev</h2>
  <p>A Rails app on Fly. Ten seconds to wake from hibernation.</p>
  <p>But it worked: armed with a Stripe terminal, <strong>the kid made a few
  hundred euro</strong> selling 3D prints.</p>
  <p class="big">Score.</p>
</section>

</main>
<div id="counter"></div>
<script>
(() => {
  const slides = [...document.querySelectorAll('.slide')];
  const counter = document.getElementById('counter');
  let cur = -1, swiped = false;
  function show(n) {
    n = Math.max(0, Math.min(slides.length - 1, n));
    if (n === cur) return;
    if (cur >= 0) slides[cur].classList.remove('current');
    cur = n;
    slides[cur].classList.add('current');
    counter.textContent = (n + 1) + ' / ' + slides.length;
    history.replaceState(null, '', '#' + (n + 1));
  }
  const fromHash = () => {
    const n = parseInt(location.hash.slice(1), 10);
    show(Number.isFinite(n) ? n - 1 : 0);
  };
  addEventListener('hashchange', fromHash);
  addEventListener('keydown', e => {
    if (e.key === 'ArrowRight' || e.key === ' ' || e.key === 'PageDown') show(cur + 1);
    else if (e.key === 'ArrowLeft' || e.key === 'PageUp') show(cur - 1);
    else if (e.key === 'Home') show(0);
    else if (e.key === 'End') show(slides.length - 1);
    else return;
    e.preventDefault();
  });
  addEventListener('click', e => {
    if (swiped) { swiped = false; return; }
    if (e.target.closest('a')) return;
    (e.clientX < innerWidth / 3) ? show(cur - 1) : show(cur + 1);
  });
  let tx = null;
  addEventListener('touchstart', e => { tx = e.changedTouches[0].clientX; }, { passive: true });
  addEventListener('touchend', e => {
    if (tx === null) return;
    const dx = e.changedTouches[0].clientX - tx;
    tx = null;
    if (Math.abs(dx) > 40) { swiped = true; dx < 0 ? show(cur + 1) : show(cur - 1); }
  }, { passive: true });
  fromHash();
})();
</script>
</body>
</html>
```

- [ ] **Step 2: Mechanical checks**

Run: `cd ~/github.com/carlosframework/story && grep -c '<section class="slide' index.html && grep -c 'src=' index.html; grep -cE '@import|fetch\(' index.html`
Expected: `2`, then `0` from the `src=` grep (grep exits 1 — that is the
pass), then `0`/exit 1 from the `@import|fetch` grep.

- [ ] **Step 3: Browser check**

Open `index.html` in a browser (`file://` is fine — nothing is fetched).
Verify: dark stage, title slide renders, → advances to lpay, ← returns,
URL bar shows `#2` after advancing, reload on `#2` lands on lpay, counter
reads `2 / 2`, clicking left third goes back.

- [ ] **Step 4: Commit**

```bash
cd ~/github.com/carlosframework/story && git add index.html && git commit -m "deck: scaffold — noir design system, slide engine, first two beats

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>"
```

---

### Task 2: Slides 3–28 — the full story

**Files:**
- Modify: `index.html` (insert all sections below, in order, immediately
  before `</main>`)

**Interfaces:**
- Consumes: Task 1's CSS classes and slide engine, unchanged. Adding
  sections is the whole task; the engine picks them up at load.

- [ ] **Step 1: Insert slides 3–28** — exactly this markup, this order:

```html
<section class="slide">
  <h2>The next problem was chat.</h2>
  <p>The same kid ran an iMessage group with his friends &mdash; and the
  Android kids couldn&rsquo;t join. I threatened to make an app.</p>
  <blockquote>
    <p>I&rsquo;m starting to think you could do the chat app after all.</p>
  </blockquote>
  <cite>&mdash; the kid, after lpay</cite>
</section>

<section class="slide">
  <div class="term">
    <header><span class="dot"></span><span class="dot"></span><span class="dot"></span>
      <span class="term-title">June 27 &mdash; the first prompt</span></header>
    <pre><span class="ps1">&gt; </span>I want to make a chat app. It's kid friendly. It doesn't require any kind of log in: just a secret link to initialise, and when I send someone the link, they get to choose a screen name. I can deploy it easily and run it on a single server. Should it use sqlite? I want all the messages to be encrypted, so that if someone compromises the server, they can't access anything.</pre>
  </div>
</section>

<section class="slide product">
  <p class="kicker">Three weeks later</p>
  <h1 class="name">Eleven</h1>
  <a class="url" href="https://elevenmessenger.com" target="_blank" rel="noopener">elevenmessenger.com</a>
  <p>Kid-friendly, end-to-end encrypted, no accounts. A secret link and a
  screen name.</p>
</section>

<section class="slide">
  <h2>Everyone kept looking at the chat&nbsp;app.</h2>
  <p>The interesting part was a session from July&nbsp;9.</p>
</section>

<section class="slide">
  <div class="term">
    <header><span class="dot"></span><span class="dot"></span><span class="dot"></span>
      <span class="term-title">July 9 &mdash; the second prompt</span></header>
    <pre><span class="ps1">&gt; </span>How do we make deploying home and the oauth broker apps essentially infinitely scaleable?</pre>
  </div>
</section>

<section class="slide">
  <p class="kicker">Out of that question</p>
  <h2>A philosophy, then an architecture.</h2>
  <p><a href="https://11factor.org" target="_blank" rel="noopener" class="url">11factor.org</a><br>
  a philosophy for building software responsibly with LLMs.</p>
  <p><a href="https://carlosframework.com" target="_blank" rel="noopener" class="url">carlosframework.com</a><br>
  the architecture built on it.</p>
</section>

<section class="slide">
  <p class="kicker">Run it in the open</p>
  <h2>Operations as an open&nbsp;ledger.</h2>
  <p><a href="https://status.elevenmessenger.com" target="_blank" rel="noopener" class="url">status.elevenmessenger.com</a></p>
  <p>An append-only, verifiable record of what the service is
  <strong>actually doing</strong> &mdash; not a status page that says
  &ldquo;all systems operational&rdquo;. A direct consequence of 11factor.</p>
</section>

<section class="slide">
  <h2>And run it anywhere.</h2>
  <p>When highly-available apps are this cheap and this simple to operate,
  on-prem stops being a compromise.</p>
  <p><strong>Your hardware. Your data. The same availability.</strong></p>
  <p>Remember this one.</p>
</section>

<section class="slide">
  <h2>People were skeptical.</h2>
  <p>&ldquo;What&rsquo;s the point?&rdquo; Fair.</p>
  <p>So: prove it across every shape of app &mdash; <strong>E2EE,
  single-process, single-database, multi-region,
  multi-platform</strong>.</p>
</section>

<section class="slide product">
  <p class="kicker">Proof, part one</p>
  <h1 class="name">Keymail</h1>
  <a class="url" href="https://keymail.dev" target="_blank" rel="noopener">keymail.dev</a>
  <p>Email, but secure.</p>
</section>

<section class="slide">
  <h2>Suddenly it seemed feasible to replace the SaaS we were
  paying&nbsp;for.</h2>
  <p><strong>Secure. Affordable. Ours.</strong></p>
</section>

<section class="slide product">
  <p class="kicker">Proof, part two</p>
  <h1 class="name">Slopbox</h1>
  <a class="url" href="https://slopbox.cloud" target="_blank" rel="noopener">slopbox.cloud</a>
  <p>Like OG Dropbox, with no upsells. Simple, fast, encrypted.</p>
</section>

<section class="slide product">
  <p class="kicker">Proof, part three</p>
  <h1 class="name">Woodstar</h1>
  <a class="url" href="https://woodstar.app" target="_blank" rel="noopener">woodstar.app</a>
  <p>Mastodon: too nerdy. Bluesky: URLs where handles should be.</p>
  <p>Woodstar: <strong>each profile its own database</strong> &mdash; and it
  feels like OG Twitter.</p>
</section>

<section class="slide">
  <p class="kicker">Meanwhile</p>
  <h2>Tito was being rebuilt in this&nbsp;shape.</h2>
  <p>Vicky, product manager, hit a productivity unlike anything I&rsquo;d
  ever seen &mdash; <strong>UX-first</strong> on a resilient, dynamic stack
  built for extremely fast iteration.</p>
</section>

<section class="slide product">
  <p class="kicker">Proof, part four</p>
  <h1 class="name">Kass</h1>
  <a class="url" href="https://kass.training" target="_blank" rel="noopener">kass.training</a>
  <p>My gym log, shared securely with my personal trainer &mdash; not with a
  US data firm.</p>
</section>

<section class="slide product">
  <p class="kicker">Time to extract</p>
  <h1 class="name">Rastrillo</h1>
  <a class="url" href="https://rastrillo.org" target="_blank" rel="noopener">rastrillo.org</a>
  <p>With the examples in hand: the web framework underneath all of them,
  pulled out and given a name.</p>
</section>

<section class="slide">
  <p class="kicker">And keep proving it</p>
  <h2>Seapointish</h2>
  <p>E2EE, self-hostable financial reporting &mdash; we know the
  <a href="https://seapoint.co" target="_blank" rel="noopener" class="url">seapoint.co</a>
  founders, so it makes a good demo.</p>
  <p>Next on the list: porting Vito
  (<a href="https://vi.to" target="_blank" rel="noopener" class="url">vi.to</a>).</p>
</section>

<section class="slide">
  <p class="kicker">The tally</p>
  <h2>Six weeks in:</h2>
  <p><strong>Eleven</strong> built in three weeks. <strong>Keymail, Slopbox,
  Woodstar, Kass</strong> the week after.</p>
  <p>A philosophy. An architecture built on the philosophy. A reusable web
  framework on top of it all.</p>
  <p>Apps that cost <strong>pennies per month</strong> &mdash; in the cloud
  or on-prem. The architecture doesn&rsquo;t mind.</p>
</section>

<section class="slide interstitial">
  <h2>But there&rsquo;s one more&nbsp;thing&hellip;</h2>
</section>

<section class="slide">
  <h2>What if deployment was the product?</h2>
  <p>I&rsquo;d been creating a fresh AWS account for every app.</p>
  <p>What if a properly global platform orchestrated it all &mdash;
  <strong>deploys in seconds</strong>, bring your own box, instances placed
  anywhere in the world, <strong>per tenant rather than per app</strong>?</p>
</section>

<section class="slide product">
  <p class="kicker">The CARLOS platform, flagship</p>
  <h1 class="name">Carloku</h1>
  <a class="url" href="https://carloku.com" target="_blank" rel="noopener">carloku.com</a>
  <p>Inspired by Heroku &mdash; and everything Heroku wasn&rsquo;t:
  <strong>truly multi-region</strong>; your boxes, Carloku&rsquo;s
  orchestration; or run the whole platform yourself.</p>
</section>

<section class="slide interstitial">
  <h2>One more thing&hellip;</h2>
</section>

<section class="slide product">
  <p class="kicker">Doesn&rsquo;t GitHub kinda suck these days? Bloated. Slow. AI bolted on.</p>
  <h1 class="name">Amadan</h1>
  <a class="url" href="https://amadan.net" target="_blank" rel="noopener">amadan.net</a>
  <p><strong>Blazing fast.</strong> Cloud or self-hosted, first-class. E2EE
  optional. Familiar concepts, fresh ideas &mdash; multiple conversations
  around a single branch. <em>Shock, horror.</em></p>
</section>

<section class="slide interstitial">
  <h2>One more thing&hellip;</h2>
</section>

<section class="slide">
  <p class="kicker">Eleven &middot; Keymail &middot; Slopbox &middot; Woodstar &middot; Amadan</p>
  <h2>Every demo you just saw has been running on a Raspberry&nbsp;Pi in my
  pocket this whole&nbsp;time.</h2>
</section>

<section class="slide">
  <h2>That&rsquo;s actually&nbsp;it.</h2>
  <ul class="roll">
    <li><a href="https://elevenmessenger.com" target="_blank" rel="noopener">elevenmessenger.com</a></li>
    <li><a href="https://keymail.dev" target="_blank" rel="noopener">keymail.dev</a></li>
    <li><a href="https://slopbox.cloud" target="_blank" rel="noopener">slopbox.cloud</a></li>
    <li><a href="https://woodstar.app" target="_blank" rel="noopener">woodstar.app</a></li>
    <li><a href="https://kass.training" target="_blank" rel="noopener">kass.training</a></li>
    <li><a href="https://rastrillo.org" target="_blank" rel="noopener">rastrillo.org</a></li>
    <li><a href="https://11factor.org" target="_blank" rel="noopener">11factor.org</a></li>
    <li><a href="https://carlosframework.com" target="_blank" rel="noopener">carlosframework.com</a></li>
    <li><a href="https://carloku.com" target="_blank" rel="noopener">carloku.com</a></li>
    <li><a href="https://amadan.net" target="_blank" rel="noopener">amadan.net</a></li>
  </ul>
</section>
```

- [ ] **Step 2: Mechanical checks**

Run: `cd ~/github.com/carlosframework/story && grep -c '<section class="slide' index.html && grep -c 'src=' index.html; grep -c 'rel="noopener"' index.html`
Expected: `28`; `0`/exit 1 for `src=`; `23` noopener anchors.

Run: `grep -o 'href="http[^"]*"' index.html | sort -u`
Expected: exactly the product/story URLs (11factor.org, amadan.net,
carloku.com, carlosframework.com, elevenmessenger.com, kass.training,
keymail.dev, rastrillo.org, seapoint.co, slopbox.cloud,
status.elevenmessenger.com, vi.to, woodstar.app) — nothing else.

- [ ] **Step 3: Browser check**

Open `index.html`: End key jumps to `28 / 28` (the roll-call), Home back to
1; reload at `#22` lands on the Carloku slide; the two prompt cards render
as terminal panels; clicking a product link opens the site in a new tab
WITHOUT advancing the slide; on a narrow window (phone-ish) no text
overflows.

- [ ] **Step 4: Commit**

```bash
cd ~/github.com/carlosframework/story && git add index.html && git commit -m "deck: the full story — 28 beats from lpay to the Pi in the pocket

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>"
```

---

### Task 3: README and GitHub remote

**Files:**
- Create: `README.md`

**Interfaces:**
- Consumes: nothing from prior tasks. Produces the public repo Task 4's
  deploy stages from (`git archive HEAD`).

- [ ] **Step 1: Write `README.md`:**

```markdown
# The Story of CARLOS

A slide deck: how a 10-second cold start on Fly turned into Eleven,
11factor, the CARLOS framework, a shelf of E2EE apps, Rastrillo, Carloku,
Amadan — and a Raspberry Pi in a pocket.

Live at <https://story.carlosframework.com>. Arrow keys, space, click or
swipe to navigate; every slide is deep-linkable (`#12`).

One self-contained `index.html`: no build step, no dependencies, no external
requests — it presents fine offline.

## Deploy

Static CARLOS app: account `carlos`, app `story`, on the flagship console.

    STAGE=$(mktemp -d) && git archive HEAD -- index.html | tar -x -C "$STAGE" \
      && carlos deploy -app story -account carlos -kind static \
           -host story.carlosframework.com \
           -version $(git rev-parse --short HEAD) \
           -console https://console.carloku.com "$STAGE"

The design spec and this plan live under `docs/superpowers/`.
```

- [ ] **Step 2: Commit and push**

```bash
cd ~/github.com/carlosframework/story && git add README.md && git commit -m "readme: what this is, how it deploys

Co-Authored-By: Claude Fable 5 <noreply@anthropic.com>"
gh repo create carlosframework/story --public --source . --push
```

Expected: repo created at github.com/carlosframework/story, main pushed.
If org policy blocks `--public`, use `--private` and note it in the report.

---

### Task 4: Provision and deploy story.carlosframework.com

**Files:** none (CLI operations against the flagship console).

**Interfaces:**
- Consumes: the pushed repo state from Task 3 (deploys `HEAD` of main).

- [ ] **Step 1: Ensure account and app exist**

```bash
carlos apps create -app story -account carlos -console https://console.carloku.com
```

If it fails because account `carlos` does not exist:

```bash
carlos accounts create -name carlos -console https://console.carloku.com
carlos apps create -app story -account carlos -console https://console.carloku.com
```

Expected: app `story` claimed in account `carlos` (the logged-in terminal,
paul@keymail.dev, becomes owner). If `apps create` fails on a *naming*
collision instead, STOP and report — do not improvise a different app name.

- [ ] **Step 2: Create the instance/route**

```bash
carlos instances create -app story -account carlos -host story.carlosframework.com -console https://console.carloku.com
```

Expected: instance record accepted; the flagship box's reconcile pass
materializes the route within seconds (post-1fba18f convergence).

- [ ] **Step 3: Deploy** (stages `index.html` alone — never ship the repo
  root, or `docs/` becomes public URLs)

```bash
cd ~/github.com/carlosframework/story
STAGE=$(mktemp -d) && git archive HEAD -- index.html | tar -x -C "$STAGE"
carlos deploy -app story -account carlos -kind static \
  -host story.carlosframework.com \
  -version $(git rev-parse --short HEAD) \
  -console https://console.carloku.com "$STAGE"
```

Expected: ship → promote → watch, ending in `live` when
`X-Carlos-Version` on the URL reports this sha. If the watch times out on
DNS grounds (resolver still negative-caching the new record), go to Step 4
— the `--resolve` check is authoritative.

- [ ] **Step 4: Verify against the box directly** (bypasses DNS caches)

```bash
SHA=$(git -C ~/github.com/carlosframework/story rev-parse --short HEAD)
curl -sI --resolve story.carlosframework.com:443:99.81.104.219 https://story.carlosframework.com/ | grep -i x-carlos-version
curl -s  --resolve story.carlosframework.com:443:99.81.104.219 https://story.carlosframework.com/ | grep -c "The Story of"
```

Expected: version header equals `$SHA` (allow the ~60s static recheck
window and re-poll, per the deploy-verification rule: verify the thing you
changed, never a cached answer); content grep ≥ 1. Also confirm a clean
public path once DNS propagates: `curl -sI https://story.carlosframework.com/`
returns 200 with the same header.

- [ ] **Step 5: Confirm docs/ did not ship**

```bash
curl -s -o /dev/null -w '%{http_code}\n' --resolve story.carlosframework.com:443:99.81.104.219 https://story.carlosframework.com/docs/superpowers/specs/2026-08-11-carlos-story-deck-design.md
```

Expected: `404`.

- [ ] **Step 6: Report** — live URL, sha served, account/app identifiers,
and anything queued for Paul (none expected — DNS already landed).

---

## Self-Review

- Spec coverage: format/interaction → Task 1 (engine) + Task 2 Step 3
  (checks); visual direction → Task 1 CSS; all 27±2 beats → Task 2 (28
  slides, within tolerance, spec's closing-slide split exercised); source &
  deploy → Tasks 3–4; out-of-scope list respected (no notes, analytics,
  imagery, scroll mode — noscript fallback is degradation, not a mode);
  testing section → mechanical checks + browser checks + deploy header. ✓
- Placeholders: none — full file content, full slide markup, exact commands
  with expected outputs. ✓
- Consistency: class names used in Task 2 all defined in Task 1's CSS
  (`kicker`, `big`, `product`/`name`, `url`, `term`/`term-title`/`dot`/`ps1`,
  `interstitial`, `roll`); slide count 28 consistent across Task 2 checks
  and README copy; deploy identity identical in README and Task 4. ✓
