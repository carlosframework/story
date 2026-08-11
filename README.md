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
