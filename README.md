# KowntX status

Live page: https://status.kowntx.com/ (also https://kowntx.github.io/kowntx-status/)

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
- **Add or change a service:** edit `services.json`. A service can send its own `"headers"` (the "Emails and timed jobs" check sends the app's public key to ask the database whether timed jobs and emails are moving). `"planned": "why"` shows it
  as "Not live yet" without checking it or raising alerts; remove that line
  when it goes live `"untilLive": "why"` does the same only while the
  address answers 404 (its server part is not deployed yet), then counts by
  itself.
- **Follow by email.** The form under "Every part" calls the database's
  `status_follow` (app migration 768). The person confirms from an email; then
  the database reads `data/status.json` every 5 minutes and emails them when a
  part goes down and when it is back. `?follow=<token>` and `?unfollow=<token>`
  are the two links in those emails. Before 768 is live the form says emails
  start with the next update. When the database itself is down these emails
  cannot go out; the page still shows it.
- **Billing (test shop)** asks the database's `robot_health` (app migration
  767): every 15 minutes the live database makes a cash bill, a khata bill and
  a payment in a shop of its own, checks stock and khata, and takes it all
  back. Down means the last run failed, or none ran in 40 minutes.

No keys or secrets are needed or stored: an answer below 500 (including
"not signed in") counts as working.

## The page

`site/` is a plain static page built from the website's own parts, so it reads as
part of kowntx.com: the plum ground, the glass nav, the night header with a lime
eyebrow and a two-tone headline, an isometric map of every part, named, from the
shop's counter at the front to the background jobs at the back (wires flow lime
while a part answers and turn coral when it stops; a part that isn't live yet is
only a dashed outline; the sale runs from the till to the app and the database
and back), the Trust
center's list layout, outages on a blush band torn like a receipt, and the
receipt footer. The logo is the website's current one and never moves.
Its fonts and logo sit next to it in `site/fonts` and `site/brand`, with no
libraries or CDNs, so it stays up when everything else is down. It works on
any static host (GitHub Pages or Cloudflare): it reads the results from this
repo on raw.githubusercontent.com and falls back to the `data/` copied beside it.

