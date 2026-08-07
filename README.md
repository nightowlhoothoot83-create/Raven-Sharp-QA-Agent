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

Those remain approval-gated.

## One-time GitHub setup

GitHub Agentic Workflows run in GitHub Actions, so no Docker/WSL is required on the laptop.

Two repository secrets are needed before the full fleet can run:

1. `COPILOT_GITHUB_TOKEN` — the token GitHub Agentic Workflows uses for the default Copilot engine.
2. `CROSS_REPO_PAT` — a fine-grained token scoped only to the Raven SaaS repositories, used by the Maintainer to check out target code and create draft repair PRs.

Never paste either secret into chat, issues or source code. Add them in **GitHub → Raven-Sharp-QA-Agent → Settings → Secrets and variables → Actions**.

The Markdown agent workflows in `.github/workflows/*.md` are compiled into hardened `.lock.yml` workflows by the compiler workflow in this repo.
