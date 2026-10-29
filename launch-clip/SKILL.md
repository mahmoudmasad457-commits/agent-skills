---
name: Launch Clip
description: Produce a sellable 20-second launch clip for a real product feature — from state, to press, to on-screen proof — with a frame-numbered brief, approval gates, and per-event sound. Use when the user wants a product promo/release clip, a 20s feature demo video, or asks to "make a launch video" for a feature they can screenshot or record.
---

# Launch Clip — 20-second launch video, one action, provable

The product: a 20-second clip showing **one action** in a real product: initial state → press → result readable at a comfortable pace. Not a long video, not an AI-slideshow.

## The offer (adapt and send)

```
Launch Clip | $150 pilot
Send your screen recordings + logo + the feature you're launching.
I turn one product action into a 20-second launch clip.
Includes: short script approved before production · 1080×1080 MP4 + cover image ·
one review round inside the approved script and visual direction.
Formats, extra scenes, and source files priced separately.
```

## The rule that prevents "cheap AI video": one reference, one job

- Pick one reference for one thing: a card entrance, a transition, or cursor motion.
- Assign frames: `entrance / read-hold / press / result`.
- Example (30fps): `120 card static · 195 press · 210 file appears · 234 result fully readable · 420 closing scene`.

## Per-client brief template

```
GOAL: show how <product> does <action>.  AUDIENCE: <who>.  OPENER: <result, ≤8 words>.  CLOSER: <next action + destination>.
DELIVERY: 1080×1080 · 30fps · 600 frames.  Assets under public/.
LAYOUT: 64px margins; title x96,y112; product card x120,y350,w840.
OPENING (0–119): title in 0→18, card 18→42, ease-out.
PRODUCT (120–419): cursor 150→180, press 195, reveal 210, readable until 419.
CLOSING (420–599): logo + closer.
SOUND: click at 195, reveal at 210 — real event frames, not estimates.
```

## Production steps

1. Reference stills: `ffmpeg -i reference.mp4 -vf "fps=2,scale=960:-2" frames/ref-%04d.jpg`
2. Mechanical first, semantic second.
3. First build: opening scene only + stills at `0,18,42,60,119` → **approval before the rest**.
4. Three stills steer each round: opening / result / closing — "Is the text readable? Is the result visible? Is anything cropped?"
5. Longest-text test: if the text wraps to three lines — **stop**, shorten or widen.
6. Reading window counted from where letters become readable; read aloud at natural speed.
7. Feedback in four fields per line: `location / observed defect / change / what stays fixed`.
8. Delivery check: `ffprobe` → 1080×1080, `30/1`, ~20s.

## Marketing + pricing honesty

- Make the sample first (own product or an owner who approves) — no offer without a sample.
- Find a buyer with a **real upcoming launch** and a working feature.
- Review notes anchored to frame numbers → a fix list, not a discussion.
- Measure actual hours (assets + review + render + delivery) across the first 3 contracts → if real hourly pay isn't ≥ 2× the $150-equivalent, raise the price.
- Reject: unlimited revisions (one approved round, rest priced), Product Hunt as a channel.
- Verify Remotion license terms for your build before first sale.
