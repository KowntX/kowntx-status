# KowntX status

Live page: https://kowntx.github.io/kowntx-status/

A GitHub Action (`.github/workflows/status.yml`) runs `check.mjs`, which asks
each service in `services.json` whether it is answering every 5 minutes for
almost six hours, saving to `data/` after each round; the page reads `data/`
straight from this repo. GitHub only starts scheduled jobs on a quiet repo every
few hours, so one long run is what keeps the checks every 5 minutes. Free: public repo, GitHub Actions
and GitHub Pages.

- **Down for two checks in a row** → an issue labelled `outage` opens here, so
  the repo owner gets an email. It closes itself when the service is back.
- **The app reads** `https://kowntx.github.io/kowntx-status/data/status.json`
  to show an outage banner.
- **Add or change a service:** edit `services.json`. `"planned": "why"` shows it
  as "Not live yet" without checking it or raising alerts; remove that line
  when it goes live.

No keys or secrets are needed or stored: an answer below 500 (including
"not signed in") counts as working.

## The page

`site/` is a plain static page in the KowntX design (Kountour colours and type,
the Gold coin wordmark on the plum bar, light and dark following the device).
Its fonts and logo sit next to it in `site/fonts` and `site/brand`, with no
libraries or CDNs, so it stays up when everything else is down. It works on
any static host (GitHub Pages or Cloudflare): it reads the results from this
repo on raw.githubusercontent.com and falls back to the `data/` copied beside it.

