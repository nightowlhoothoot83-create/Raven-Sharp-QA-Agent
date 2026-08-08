---
on:
  schedule: daily
  workflow_dispatch:
  push:
    branches: [main]
    paths:
      - '.github/workflows/raven-site-guardian.lock.yml'

engine: codex
strict: false

network:
  allowed:
    - defaults
    - github
    - playwright
    - chrome
    - fonts
    - "raven-sharp.com"
    - "pages.dev"
    - "up.railway.app"
    - "wheelnamepicker.com.au"
    - "mycalendartools.net"
    - "mycalctools.net"

permissions:
  contents: read

tools:
  bash: ['*']
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

Audit the live Raven Sharp / Ascension Digital Group sites below. This is a browser-first **core-function, regression, indexability and revenue-blocker check**, not a generic SEO essay.

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

Use `playwright-cli` on every public HTML page you inspect. Prefer direct observation over assumptions.

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

For each AdSense site, perform a concrete indexability crawl in addition to the browser review:
- fetch `/robots.txt` and confirm Google is not accidentally blocked;
- fetch `/sitemap.xml` and parse every listed URL;
- verify each sitemap URL reaches a successful final response without a redirect loop, 403, 404 or 5xx;
- verify indexable pages do not contain an accidental `noindex` directive;
- inspect each page's canonical URL and confirm it points to the intended final canonical page;
- flag canonicals that point to a 404, a redirect loop, a different unintended page, or a non-indexable URL;
- flag sitemap entries that are themselves redirects instead of final canonical URLs;
- check internal links from the homepage and major tool pages for broken 404/5xx destinations;
- distinguish intentional redirects and intentional canonical alternatives from genuine defects rather than treating every non-indexed Search Console category as an error.

For each AdSense site, also fetch `/ads.txt` from the canonical root domain and record:
- HTTP status;
- whether the response is plain readable text rather than HTML/error content;
- whether the expected publisher record is present exactly;
- whether redirects prevent the crawler from reaching a successful final response.

When reporting an indexability finding, classify it as one of:
- **SITE-DEFECT** — our site is causing the problem, such as accidental 403, 404, 5xx, redirect loop, broken canonical, noindex, robots block, or bad sitemap entry;
- **INTENTIONAL** — redirect/canonical behaviour is deliberate and correct;
- **GOOGLE-STATE** — Google has discovered/crawled but chosen not to index and no concrete site-side crawl/indexability defect is observable.

Do not claim that `Discovered - currently not indexed` or `Crawled - currently not indexed` is automatically repairable unless a concrete site-side defect is found. Search Console can lag behind live fixes until Google recrawls.

At the end, include one compact fleet-status table in the run summary with each Raven SaaS URL, classification, and the single most important observed blocker or proof point. Also include a compact AdSense indexability table with each site, crawl/indexability status, and the highest-priority concrete site defect if any. This summary is for recovery triage, not marketing.

Do not create issues for minor taste preferences. Create an issue only for a concrete defect, revenue blocker, meaningful regression, misleading product status/pricing, missing access path, or SITE-DEFECT indexability problem. Group related findings by site so the repo does not fill with noise.

If all checked public surfaces are healthy, use the no-op output and create no issue. Do not overclaim authenticated/core-function health when it was not tested.

Do not log in with guessed credentials, change production settings, publish content, spend money or submit third-party forms.