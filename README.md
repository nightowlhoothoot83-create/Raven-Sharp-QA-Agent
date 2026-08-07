# Raven Sharp Agent Command Center

Cloud-first agent control repo for Raven Sharp and Ascension Digital Group.

The goal is simple: agents should **do the work**, not just write prompts or reports.

## Agent fleet

### 1. Raven SaaS Maintainer
Browser + coding agent for the Raven SaaS products. It can inspect a live page, reproduce problems, trace them to source, make code changes, run tests and open a **draft pull request** for review.

Modes:
- `repair` — fix broken behaviour, runtime errors, API/UI failures and regressions
- `polish` — headings, layout, mobile responsiveness, accessibility, consistency and visual cleanup
- `build` — complete or add requested functionality

Production deployment, publishing, spending money and destructive data changes are deliberately outside the agent's authority.

### 2. Raven Site Guardian
Scheduled browser agent for public-site health. It checks Raven SaaS sites plus the AdSense sites for obvious live problems, including browser errors, heading/structure problems, mobile overflow, broken navigation and `ads.txt` availability.

### 3. Raven Affiliate Scout
Research agent for finding complementary affiliate products and programs that fit Raven/ADG products and websites. It produces actionable opportunities, not random affiliate spam. It does not join programs or accept terms automatically.

### 4. Raven Marketing Worker
Creates finished content and ad-copy packs from a real brand/product/site. It is an interim execution worker while Raven Content Creator and Ad Manager are being completed. It does not launch paid ads or spend money.

### 5. Raven Art Marketplace Agent
Browser Use Cloud worker for the user's own photography and artwork on art marketplaces such as Fine Art America and ArtPal.

It uses a persistent Browser Use profile for logged-in marketplace sessions and a Browser Use workspace for photo files. It can inspect each image, upload it, write the finished title/description/keywords, choose relevant marketplace categories, configure sensible product options and prepare listings.

Modes:
- `prepare` — saves a private/draft listing where the marketplace supports it, or stops before the final public action
- `publish-approved` — publishes only the already-prepared listings named in the run after the user enters the exact `PUBLISH APPROVED` confirmation

It must not change account identity, passwords, payout/tax settings, subscription level, payment methods or accept new commercial/legal terms.

## SaaS targets

- POD Automation — `https://ravensharppod.pages.dev/pipeline`
- Image Optimiser / Upscaler — `https://raven-sharp-image-optimiser-and-upscaler.pages.dev/login`
- Ad Manager — `https://ads.raven-sharp.com/`
- Book Creator — `https://books.raven-sharp.com/`
- Content Creator — `https://content.raven-sharp.com/`
- Smart Cleaner — `https://cleaner.raven-sharp.com/`

## AdSense targets

- `https://wheelnamepicker.com.au/`
- `https://mycalendartools.net/`
- `https://mycalctools.net/`

Expected AdSense publisher line:

```text
google.com, pub-1904958390525375, DIRECT, f08c47fec0942fa0
```

## Safety model

Agents may browse, inspect, research, edit code in a checked-out target repo, run tests and prepare draft PRs/reports.

Agents may **not** autonomously:
- publish Etsy listings
- deploy to production
- merge repair PRs into production
- launch paid ads or change ad spend
- buy products or subscriptions
- join affiliate programs / accept legal terms
- delete production data
- expose or request credentials in issues or logs

Art-marketplace publication is a separate explicit approval path: `prepare` cannot publish, while `publish-approved` requires an exact confirmation in a manually triggered workflow and may publish only the named prepared batch.

## One-time GitHub setup

GitHub Agentic Workflows run in GitHub Actions, so no Docker/WSL is required on the laptop.

The core GitHub agent fleet needs these repository secrets:

1. `OPENAI_API_KEY` — used by the Affiliate Scout and Marketing Worker through the Codex engine. `CODEX_API_KEY` may be used instead if preferred.
2. `GH_AW_GITHUB_TOKEN` — a fine-grained GitHub PAT scoped only to the Raven SaaS repositories, used by the Maintainer for cross-repository checkout and draft repair PRs.

The SaaS Maintainer and Site Guardian use GitHub's recommended `copilot-requests: write` permission, so they do not require a separate Copilot PAT.

### Browser Use setup for the Art Marketplace Agent

The Art Marketplace Agent additionally needs:

**Repository secret**
- `BROWSER_USE_API_KEY` — Browser Use Cloud API key

**Repository variables**
- `BROWSER_USE_ART_PROFILE_ID` — Browser Use profile that contains the user's logged-in Fine Art America / ArtPal browser state
- `BROWSER_USE_ART_WORKSPACE_ID` — Browser Use workspace containing the photos/art files to list

Marketplace passwords should not be stored in this repo or passed to the agent. Browser Use profile state handles the authenticated session instead.

Never paste a secret into chat, issues or source code. Add secrets in **GitHub → Raven-Sharp-QA-Agent → Settings → Secrets and variables → Actions**.

The Markdown agent workflows in `.github/workflows/*.md` are compiled into hardened `.lock.yml` workflows. The compiler stages generated output for promotion because the default Actions app cannot itself write workflow files.

A separate deterministic `ads-txt-monitor.yml` checks the three AdSense sites every six hours and does not need an AI key.
