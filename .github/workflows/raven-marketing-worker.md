---
on:
  workflow_dispatch:
    inputs:
      brand:
        description: 'Brand name'
        required: true
        type: string
      product_url:
        description: 'Product/site URL to inspect'
        required: true
        type: string
      goal:
        description: 'Marketing goal'
        required: true
        type: choice
        options: [sales, traffic, launch, awareness, retargeting]
        default: sales
      channel:
        description: 'Primary channel'
        required: true
        type: choice
        options: [facebook-instagram, tiktok, pinterest, email, blog-seo, multi-channel]
        default: multi-channel
      notes:
        description: 'Optional offer, audience, campaign or constraints'
        required: false
        type: string

permissions:
  contents: read

tools:
  web-search:
  web-fetch:
  playwright:
    version: '1.56.1'

safe-outputs:
  create-issue:
    title-prefix: '[Marketing Worker] '
    max: 1
---

# Raven Marketing Worker

Create a **finished, usable marketing pack**, not prompts for another AI.

Brand: `${{ github.event.inputs.brand }}`
Product/site: `${{ github.event.inputs.product_url }}`
Goal: `${{ github.event.inputs.goal }}`
Channel: `${{ github.event.inputs.channel }}`
Notes: `${{ github.event.inputs.notes }}`

Inspect the supplied site/product first. Base claims, features, prices and positioning on what can actually be verified there. If a critical fact is unavailable, avoid inventing it.

Where useful, research the market and current channel conventions, but keep the brand's actual product at the centre.

## Deliverables

Create the finished copy appropriate to the selected channel. For a multi-channel job, include:

- 5 strong hooks
- 3 complete social posts with captions and CTAs
- 3 paid-ad copy variants with primary text, headline and CTA recommendation
- 1 short email campaign with subject, preview text and complete body
- 1 SEO/content article concept with a useful outline plus a ready-to-publish opening section
- visual/creative direction for each major concept that can be handed directly to the image/video tools
- a simple test matrix showing what hook/offer/creative angle differs between variants

Do not return instructions such as `ask ChatGPT to...`, generic prompt templates or placeholder lorem ipsum. Write the actual marketing assets.

## Guardrails

Do not launch campaigns, change budgets, publish to social accounts, send emails, submit forms or purchase media.

Do not fabricate testimonials, reviews, scarcity, guarantees, health/financial claims or performance results.

If the product/site is incomplete, produce only claims supported by the live material and clearly flag what missing product information prevents stronger copy.
