---
name: "social_crosspost"
description: "Turn one approved asset (image/video) plus a topic into a staged per-platform caption pack — Instagram, TikTok, YouTube Shorts, Facebook, Threads, Pinterest — each in that platform's correct link format. Use after an asset is approved; output is drafts only, never posted."
---

# Social Crosspost

## Purpose
Film once, publish everywhere: take a single approved asset and topic and produce the full per-platform caption pack, graded and staged as drafts.

## Workflow
1. Load the `social-brand-playbook` skill first. Its platform rules and grader checklist are the rule source and QA gate for everything below.
2. Collect inputs: the approved asset (file path), the topic or key message (1–2 sentences), and any must-include facts (price, link, date — sourced, never invented).
3. Draft one caption per platform:
   - **Instagram:** hook + value + hashtags; ends with the link-in-bio CTA; no raw URLs.
   - **TikTok:** short, hook-first caption + hashtags; note the native-audio plan ("Use audio" at post time); no raw URLs.
   - **YouTube Shorts:** title + description with the direct URL.
   - **Facebook:** post copy with the direct clickable URL in the body.
   - **Threads:** concise copy with the direct URL.
   - **Pinterest:** pin title + description + destination URL attached.
4. Run every caption through the playbook grader. Fix all hard fails; apply soft-fix suggestions where they improve the draft.
5. Stage the pack as drafts (see Output Contract) and report the grader verdicts.
6. Hand the staged pack to the existing schedulers only after human approval of each item. Never auto-post or auto-schedule.

## Output Contract
A caption pack containing, per platform: platform name, caption/title text, hashtags, link (the URL or the marker `link-in-bio`), media file reference, audio plan (video), grader verdict (pass/fail plus fixes applied), and status `draft`. Deliver as one markdown file or JSON — one pack per asset.

## Operating Rules
1. The input asset must already be approved. This skill writes captions, not creative direction.
2. Every caption is graded before staging. A caption that fails a hard check is revised, not shipped.
3. Never invent facts: prices, dates, URLs, and claims come from the input or a verified source.
4. Link formats follow the playbook buckets exactly — no raw URLs on Instagram/TikTok, no "link in bio" phrasing on direct-URL platforms.
5. This skill never posts, schedules, or publishes. Staging is the final step; a human approves and triggers schedulers.
