# Raven Sharp End-to-End Pipeline Contract

Branch-only specification. No merge, deploy, production mutation, billing action, publishing action, destructive file action, or ad spend is authorised by this file.

## Purpose

The QA Agent must test complete customer workflows rather than treating a successful page load or API response as a full pass.

## Core SaaS journey

For each Raven Sharp SaaS, test where applicable:

**Visit → Register → Login → Account Isolation → Create/Upload → Process/Generate → Save → Download/Export → Billing/Plan Limits → Logout → Login Again → Persistence**

Each stage records one of:

- Pass
- Partial
- Fail
- Blocked

A partial stage cannot produce a full-product green pass.

## Shared checks

- landing and app load on desktop/mobile;
- registration/login/logout;
- invalid auth handling;
- authenticated API access;
- two-account ownership isolation;
- database ownership filters;
- file storage ownership;
- third-party provider success/error paths;
- useful user-facing error states;
- save/persistence across a new session;
- download/export integrity where relevant;
- plan and quota enforcement where relevant;
- no production/destructive action without the required approval gate.

## Product-specific contracts

### Image Optimiser & Upscaler

- upload supported image;
- optimise/upscale;
- background removal;
- preview full image;
- preserve foreground detail;
- save/download;
- large-image handling;
- provider error handling.

Background removal quality is separate from whether the processing endpoint returned successfully.

### POD Suite

- original artwork generation;
- Production Ready quality gate;
- print-safe check;
- mockup generation as an independent feature;
- download/export;
- preserve working mockup behaviour when artwork generator changes.

### Content Creator

- owner/customer brand separation;
- create/select brand profile;
- generate each supported content format;
- asset persistence;
- export/download;
- no owner brand references or records exposed to normal customer accounts.

### Ad Manager

- two-account brand isolation;
- create/edit/delete own brand;
- owner-only preset handling;
- campaign draft workflow;
- no cross-account local-storage bleed;
- no publishing or ad spend during ordinary QA.

### Book Creator

- project creation;
- save/reopen;
- generation pipeline;
- export/download;
- any handoff to Content Creator uses authenticated, scoped data.

### Smart Cleaner

- SaaS authentication;
- cloud OAuth health separately from login health;
- connect Google Drive;
- scan/list;
- duplicate and near-duplicate analysis;
- organisation suggestions;
- preview mutations;
- perform only explicitly approved move/rename/archive/delete actions;
- verify result;
- history/undo where supported;
- disconnect/reconnect;
- token expiry/revocation handling;
- account isolation.

## Evidence

For failures and repairs capture, where available:

- screenshot before;
- screenshot after;
- test name and stage;
- relevant response/status without secrets;
- preview URL;
- commit SHA;
- exact changed files;
- retest result.

## Manager Hub handoff

Real failures should create or update a Manager Hub repair request. Repairs remain open until the relevant E2E stage passes again or the owner explicitly marks the item Already Fixed / Dismissed.

No QA result grants merge or deployment authority by itself.
