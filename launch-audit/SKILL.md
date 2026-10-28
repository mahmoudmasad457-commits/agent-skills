---
name: launch-audit
description: Pre-launch readiness audit for any web product or agent-built app. Use when the user is about to ship or launch something and wants a concrete go/no-go check, when an agent has finished building an app and "done" needs proving, or when the user asks to "audit before launch", "is this ready to ship", or "check for launch blockers". Covers secrets, rate limits, spend caps, backups, restore tests, meta/OG, privacy, contact path, and rollback — with evidence, not vibes.
---

# Launch Audit — 20-point go/no-go with evidence

Run this **before** anything ships. The rule that makes this skill different:
**every verdict needs evidence, and gaps are declared, never hidden.**

## Output contract (produce exactly this)

A table of 20 rows: `# | check | verdict | evidence`, plus a final line:
`X/20 pass · N partial · N gap-by-decision · N n/a`.

Verdicts are only: ✅ pass (with evidence) · ⚠️ partial · ❌ gap (declared or fixed) · ➖ n/a-by-design.
No percentages. Fractions only. No "should be fine".

## The three evidence piles

For every claim you make, tag it:
- **Observed** — you actually ran/fetched/read it (quote the command or the response).
- **Inferred** — you reasoned from observed facts (say from what).
- **Unknown** — could not verify (say why and what would verify it).

An audit with zero `Unknown` rows is suspicious. Go back and check harder.

## The 20 checks

Run each one; fix what is cheap and safe to fix, re-verify, and record what you changed.

1. **Secrets in frontend** — grep the client bundle/source for keys, tokens, passwords. Server must return only boolean flags (`apiKeySet`), never the secret.
2. **Leaked-and-committed keys** — check git history/exports; rotate anything that ever touched a repo.
3. **Rate limiting** — confirm one client cannot burn the machine. Verify by hammering an endpoint and counting 429s. **Known trap:** a global limit can throttle the app's own polling heartbeats (settings/approvals checks) — exempt cheap idempotent GETs the UI itself issues every second, then re-verify both directions: abuse gets 429, heartbeat gets none.
4. **Auth on every route, not just the UI** — test API paths directly (curl/fetch), bypassing the frontend. If none exists by design (single-user local tool), declare it as a decision, not an oversight — and state the blast radius.
5. **Data isolation** — confirm responses never leak other tenants' or raw secret fields.
6. **Input validation server-side** — every write route validates shape and size; reject oversized bodies.
7. **AI/provider spend cap** — a daily or per-run ceiling exists and is enforced before the call, not after the bill.
8. **No stack traces to users** — trigger a 500 path (or simulate); production errors must be sanitized.
9. **Errors surface before the user reports** — logs/activity feed capture failures automatically.
10. **Backup + tested restore** — a backup must exist AND be provably restorable: boot from the restored copy and verify it serves the same row counts. A backup you never restored is a hope, not a backup.
11. **Designed 404/500** — custom pages, no framework debug dumps.
12. **Cheap device** — verify at ~390px width with no horizontal overflow.
13. **Load time < 3s** — measure it; don't estimate.
14. **Meta + OG tags** — title, description, og:image (real file, 1200×630), twitter:card.
15. **Privacy + terms** — honest document: what is stored, what never happens, user rights, declared limits.
16. **Analytics** — decide consciously: privacy-first products may declare "zero third-party analytics" as the answer. The check is that a *decision* exists.
17. **Signup/payment/reset tested end-to-end** — n/a if no accounts; test what exists.
18. **Email deliverability** — n/a if no email; otherwise real-send test to your own addresses.
19. **Contact path** — a human knows where the human is: health endpoint, run receipt, or support panel.
20. **Rollback plan** — for data: versions + restore (check 10). For code: previous build retained or one-command rollback. A launch without a rollback story is a bet, not a plan.

## The re-verification bar

After any fix, re-run the full verification you already have (types, unit tests, build, e2e, visual capture with content checks) — a fix that breaks something else is not a fix. Report the bar as fractions: `typecheck pass · tests 9/9 · e2e 5/5 · capture 29/29`.

## Anti-gaming rules

- Never mark a check pass because code "looks right" — run the probe.
- Never widen a threshold to make a test green without recording the original failure and the reason.
- Time-dependent UI (greetings by hour of day) — assert the *family* of valid strings, not one variant, or your check fails at midnight.
- If your own tooling gets rate-limited by the app under test, wait out the window before re-testing, or your second result measures the limiter, not the app.

## Worked example (what a real audit looks like)

A local-first automation app was audited with this exact list: 14/20 direct pass, 2 partial, 2 gaps-by-decision, 2 n/a. Fixes made during the audit: global rate limiter (proven: 700 hammer hits → 100×429 with Retry-After, 160 heartbeat calls → 0×429), AI daily spend cap, backup script + restore test (restored copy served 11 workflows, counts matched), OG tags + image, privacy doc, contact panel. The first limiter version throttled the app's own polling and dropped visual capture to 23/29 — the capture caught it, the heartbeat exemption fixed it, and the lesson is baked into check 3 above.
