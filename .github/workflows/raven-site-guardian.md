---
on:
  schedule: daily
  workflow_dispatch:

permissions:
  contents: read

tools:
  playwright:
    version: '1.56.1'
  web-fetch:
  web-search:

safe-outputs:
  create-issue:
    title-prefix: '[Site Guardian] '
    max: 3
---

# Raven Site Guardian

Audit the live Raven Sharp / Ascension Digital Group sites below. This is a browser-first health and revenue-blocker check, not a generic SEO essay.

## SaaS sites

- https://cleaner.raven-sharp.com/
- https://books.raven-sharp.com/
- https://ads.raven-sharp.com/
- https://content.raven-sharp.com/
- https://ravensharppod.pages.dev/pipeline
- https://raven-sharp-image-optimiser-and-upscaler.pages.dev/login

## AdSense sites

- https://wheelnamepicker.com.au/
- https://mycalendartools.net/
- https://mycalctools.net/

Expected ads.txt record:

`google.com, pub-1904958390525375, DIRECT, f08c47fec0942fa0`

## Checks

Use Playwright on every public page you inspect. Prefer direct observation over assumptions.

For the SaaS sites, check:
- page loads and obvious HTTP/browser failures
- browser console errors and failed network requests that affect the page
- main heading presence and sensible heading hierarchy
- important buttons/links that are visibly broken or dead
- obvious mobile horizontal overflow or unusable layout
- login/public landing page state where authentication blocks deeper inspection

For each AdSense site, additionally fetch `/ads.txt` from the canonical root domain and record:
- HTTP status
- whether the response is plain readable text rather than HTML/error content
- whether the expected publisher record is present exactly
- whether redirects prevent the crawler from reaching a successful final response

Do not create issues for minor taste preferences. Create an issue only for a concrete defect, revenue blocker or meaningful regression. Group related findings by site so the repo does not fill with noise.

If all checked sites are healthy, use the no-op output and create no issue.

Do not log in with guessed credentials, change production settings, publish content, spend money or submit third-party forms.
