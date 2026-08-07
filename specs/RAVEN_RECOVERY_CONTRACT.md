# Raven Sharp SaaS Recovery Contract

This file is the anti-regression source of truth for Raven Sharp recovery work.

The goal is not to make a page look finished. The goal is to preserve and restore the intended end-to-end product jobs so the suite can do useful work without constant babysitting.

## Non-negotiable definition of done

A feature is **not done** because a page, card, button, prompt, mock response or placeholder exists.

A feature is done only when:

1. the real user flow can be completed end-to-end;
2. the expected output is produced, saved or handed off as intended;
3. errors are surfaced honestly rather than replaced with fake-success UI;
4. existing working capabilities still pass after the change;
5. the implementation is connected to the real backend/provider where the product requires one;
6. any unavailable third-party dependency is identified explicitly instead of being silently stubbed;
7. production publish/deploy/spend/destructive actions remain behind approval gates.

**Never remove or shrink a working capability merely to make a build, test or UI check pass.** If a refactor would reduce scope, stop and report it as a regression risk.

## Source-of-truth order

When current code disagrees with earlier product intent, use this order:

1. explicit user-approved requirements and current business goal;
2. repository PRDs/specs and feature documents;
3. historical commits that implemented a real end-to-end capability;
4. current source code and tests;
5. marketing/public-page copy.

Marketing copy alone must never be treated as proof that a capability works.

Before a significant repair/refactor, inspect `git log --all --oneline`, relevant historical diffs, PRDs, READMEs and tests. A later shell is not automatically more correct than an earlier functional implementation.

## Fleet contracts

### Raven Sharp Content Creator / Video Creator

Repository: `nightowlhoothoot83-create/Raven-Sharp-Content-Creator`

Historical/current evidence establishes a product intended to create **finished brand content**, not merely prompts. The recovery target includes:

- per-brand production profiles/presets;
- finished social rendering;
- carousel ZIP output;
- video generation / economy animation path;
- TTS/audio where supported;
- caption composition;
- Book Creator handoff/import;
- generation-provider integration with honest provider availability;
- user auth, storage and billing where the deployed architecture expects them.

Important known-good implementation references:

- `a23ce258` — finished brand production engine, rendering, carousel ZIPs, video/TTS, captions and Book Creator handoff;
- `1aaba0f` — finished-output production engine;
- `5c197cd` — current Zyia / Feed the Feed / Spew Crew production presets;
- `851a37d` — brand production studio as the primary experience.

Current README also documents that the frontend/backend wiring and provider deployment were incomplete at that point. Do not mistake an attractive frontend shell for a deployed content engine.

**Acceptance:** given a brand profile and content request, the app must produce a usable finished asset/output, not just instructions for another tool.

### Raven Sharp Book Creator

Repository: `nightowlhoothoot83-create/Raven-Sharp-Book-Creator`

Recovery target:

- generic brand blueprint/profile ingestion;
- reference-image workflow;
- AI-generated book text and images;
- KDP-ready export presets/output;
- title sanitising and book project flow;
- one-use handoff into the content/video production workflow;
- auth, storage and billing where the deployed architecture expects them.

Important references:

- `86342d5` — generic brand blueprints, reference images, title sanitising, Book Studio v2 and video handoff;
- `2fa9464` — brand-first Book Creator backend;
- current README documents Gemini text/image generation, KDP-ready presets, JWT auth, Stripe, MongoDB and R2 architecture.

**Acceptance:** a user can progress from a book brief/brand to a generated book package/export. A studio page without a functioning generation/export chain is not done.

### Raven Sharp POD Automation

Repository: `nightowlhoothoot83-create/Raven-Sharp-POD-Automation`

Recovery target is the existing automated POD pipeline, not a new single-provider mockup toy.

Required preserved capabilities include:

- platform-neutral pipeline until final destination selection;
- AI image/design analysis and listing metadata generation;
- provider/product selection logic;
- real provider drafts where supported;
- **authentic provider mockups/previews before a draft is considered ready**;
- asynchronous provider mockup polling;
- review queue and explicit final destination selection;
- approved-only final export/handoff;
- existing multi-provider / marketplace paths must not be silently removed.

Important references:

- `a653223` — create provider drafts with authentic POD mockups;
- `1e217df` — async polling for provider mockups;
- `637891d` — require authentic mockups before drafts are ready;
- `8fd6123` — show provider mockup readiness in review;
- `c80ef72` — platform-neutral pipeline until final publish;
- `50b4a1c` — final destination selection in review queue;
- `d6c4d52` / `64f0493` — approved-only final CSV/export flow.

**Acceptance:** a POD run reaches a reviewable product/listing with authentic preview/mockup evidence where the provider supports it. Missing mockups are a regression/blocker, not a cosmetic issue.

### Raven Sharp Smart AI Cleaner

Repository: `nightowlhoothoot83-create/Raven-Sharp-Smart-AI-Cleaner`

The repository PRD defines a cross-device AI file scanner/cleaner. Required preserved capabilities include:

- signup/login/me auth flow;
- local/internal file registration/scan paths;
- Google Drive and Dropbox source connections/scans;
- OneDrive and Google Photos support added later, subject to provider API restrictions;
- exact/content/perceptual duplicate detection;
- real Claude vision analysis for remotely re-fetchable images;
- AI rename suggestions with user approval/edit/reject;
- cross-source delete/rename where the provider allows it;
- dashboard/source statistics;
- Stripe tier enforcement where configured.

Important references:

- repository `memory/PRD.md` — explicit v1.1 feature contract;
- `d73991f` — portable Anthropic integration plus OneDrive/Google Photos support;
- `7ae06b3` — real perceptual image dedup;
- `d1eee13` — real Claude Vision instead of fake image analysis;
- `dd57b0d` — Stripe billing.

**Acceptance:** login succeeds and a user can connect/register a supported source, scan it, review real duplicate/rename results, and apply a supported action. A dashboard alone is not the product.

### Raven Sharp Image Optimiser & Upscaler

Repository: `nightowlhoothoot83-create/Raven-Sharp-Image-Optimiser-Upscaler`

This is currently the strongest working baseline and should be protected from unnecessary rewrites.

Preserve:

- multi-image intake/batch processing;
- resize/preset outputs including POD/social/KDP sizes;
- format/quality/DPI/compression controls;
- sharpening/brightness/contrast/saturation controls;
- optional local background removal;
- before/after preview;
- single and ZIP download outputs;
- job/history metadata where backend integration is available.

The current source contains real local processing and download logic. Do not replace it with a server prompt or upload-only shell.

**Acceptance:** a real image can be processed and downloaded in the requested format/size with the selected settings.

### Raven Sharp Ad Manager

Repository: `nightowlhoothoot83-create/Raven-Sharp-Ad-Manager`

The current README explicitly says the app still uses temporary memory/browser storage and that authentication, durable persistence and Stripe webhook access control were not implemented at that snapshot. Historical commits show a real login/register UI was wired to backend auth endpoints before later public-readiness work.

Recovery requirements:

- preserve/restore real auth instead of decorative sign-in UI;
- brand/campaign records must use durable persistence before the product is considered production-ready;
- ad generation must output usable campaign/creative variants rather than prompt text;
- billing must have webhook-based access control before paid production use;
- never auto-launch paid campaigns or spend money without approval.

Important references:

- `7555c05` — real login/register modal wired to backend auth endpoints;
- `625067e` — restored missing Sign In control;
- README current-limitations section — persistence/auth/webhooks still require verification and completion.

**Acceptance:** a user can authenticate, create/save a brand/campaign, generate usable ad output, leave and return without losing the work. Paid-platform launch remains an approval step.

### Raven Sharp Hub / SaaS homepage

Repository: `nightowlhoothoot83-create/Raven-Sharp-Hub`

The Hub is a directory/storefront for the suite. Its job is clarity and truthful routing, not visual decoration alone.

Required:

- each SaaS card clearly says **what job the product actually performs**;
- each card points to the correct live product URL;
- product status must not imply a backend/provider is working when it is not;
- pricing displayed on the Hub must match the product's real billing configuration;
- no duplicate/conflicting product names that make it impossible to tell the tools apart;
- mobile layout must keep product purpose, CTA and price legible;
- affiliate/utility content must not bury the primary SaaS products.

The Hub has historical commits for published pricing and expanded ecosystem navigation. Any homepage rewrite must preserve clarity and correct destinations rather than merely matching a shared visual template.

## Required regression workflow for every repair

For any target repository:

1. **Archaeology:** inspect PRD/docs and relevant history before editing.
2. **Current-state proof:** reproduce the actual live/local defect.
3. **Contract match:** identify which acceptance requirement is failing.
4. **Small repair:** restore/fix the missing path without removing unrelated capability.
5. **Regression run:** build/tests plus the previously working core flows relevant to the change.
6. **Evidence:** record before/after behaviour, commands/tests and any remaining external dependency.
7. **Review gate:** code changes end as a draft PR unless the requested task is limited to this QA repository. Production deploy/merge/publish/spend/destructive actions require approval.

## Current recovery priority

1. Make the fleet observable and truthful.
2. Restore login/core access where broken.
3. Restore end-to-end core jobs.
4. Protect known-good behaviour with acceptance/regression tests.
5. Only then polish the Hub/pricing and resume broader automation/monetisation work.
