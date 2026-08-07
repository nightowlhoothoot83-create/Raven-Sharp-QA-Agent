---
on:
  schedule: daily
  workflow_dispatch:
  push:
    branches: [main]
    paths:
      - '.github/workflows/raven-site-guardian.lock.yml'

engine: codex

permissions:
  contents: read

tools:
  bash:
    - 'playwright-cli:*'
    - 'cat:*'
  playwright:
    version: '0.1.17'
    mode: cli
  web-fetch:

safe-outputs:
  create-issue:
    title-prefix: '[Site Guardian] '
    max: 3
---

# Raven Site Guardian

Audit the live Raven Sharp / Ascension Digital Group sites below. This is a browser-first **core-function, regression and revenue-blocker check**, not a generic SEO essay.

Before testing, read `${{ github.workspace }}/specs/RAVEN_RECOVERY_CONTRACT.md`. Use it to distinguish a real product from a page that merely looks complete. Public-page checks cannot prove authenticated functionality, so never report a product as fully healthy when the core job was not actually testable.

## Raven hub

- https://raven-sharp.com/

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

Use `playwright-cli` on every public page you inspect. Prefer direct observation over assumptions.

For the Raven Hub, check:
- each SaaS can be identified quickly by its actual job, not vague/generic wording;
- tool names are not duplicated/confusing;
- product links lead to the intended live product;
- pricing/status claims are internally consistent and do not promise an obviously unavailable product path;
- mobile layout keeps product purpose, CTA and pricing legible.

For the SaaS sites, check:
- page loads and obvious HTTP/browser failures;
- browser console errors and failed network requests that affect the page;
- main heading presence and sensible heading hierarchy;
- important buttons/links that are visibly broken or dead;
- obvious mobile horizontal overflow or unusable layout;
- login/public landing page state where authentication blocks deeper inspection;
- whether the public UI appears to expose the contracted core job rather than only a marketing shell;
- obvious regression signals, such as a previously expected workflow replaced by a generic landing page, missing sign-in control, missing product preview/review state, or a CTA that cannot reach the app.

Classify each SaaS finding as one of:
- **PASS-PUBLIC** — public/login surface is healthy, but authenticated core flow not proven;
- **PARTIAL** — some expected workflow is visible/working but a meaningful path is missing or blocked;
- **BROKEN** — access/core entry path fails or a concrete revenue/core-function blocker exists;
- **UNVERIFIED-AUTH** — deeper testing requires safe authenticated test access.

Do **not** call a SaaS fully healthy simply because its homepage returned 200.

For each AdSense site, additionally fetch `/ads.txt` from the canonical root domain and record:
- HTTP status;
- whether the response is plain readable text rather than HTML/error content;
- whether the expected publisher record is present exactly;
- whether redirects prevent the crawler from reaching a successful final response.

Do not create issues for minor taste preferences. Create an issue only for a concrete defect, revenue blocker, meaningful regression, misleading product status/pricing, or missing access path. Group related findings by site so the repo does not fill with noise.

If all checked public surfaces are healthy, use the no-op output and create no issue. Do not overclaim authenticated/core-function health when it was not tested.

Do not log in with guessed credentials, change production settings, publish content, spend money or submit third-party forms.
