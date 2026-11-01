# Changelog

## 3.0.0 — 2026-11-01

Behavior changes (the same input can now get a different recommendation):
- **Break-even test fixed.** v2 chose the refundable fare when `P(cancel) × credit_at_risk > premium`, which treats the premium as spent even when the trip is cancelled. Cancelling a refundable fare refunds the premium too. The threshold is now `P* = (R − C) / ((R − C) + f·C)`: the worked example moves from 31% (156 / 501) to 23.7% (156 / 657). Adds the derivation, a cancellation-fee variant, a change-instead-of-cancel variant and an audit checklist line.
- **Quick mode is quick.** A step-by-mode workflow table: Quick infers from the message, asks at most 2 questions, verifies only the top two options on the airline site and returns a compact card of about 250 words.
- **When tools are missing.** Without a search tool, browser or web search, the skill works from pasted results, labels every price unverified and never invents flight numbers, times, prices or fees.
- **Concrete exchange rates for all five profiles**, with per-person and per-booking units (v2 had numbers only for Balanced).

Rules (each checked against an official source; see the "Dated facts" table in `references/regions.md`):
- British Columbia on permanent UTC-7 from 2026-11-01; US preclearance airports in Canada; CBP first port of entry and bag re-check; US refund on airline cancellation or significant change (14 CFR 260); 24-hour rule (14 CFR 259.5); EU EES in full operation, ETIAS not yet running.
- Fixed: "connecting through the US to Canada checks bags through". Travelers from outside North America clear CBP at the US hub.
- New connection step: departure-side preclearance at a Canadian hub. Minimum connection time and "legal connection" defined.

Scope:
- New `references/scenarios.md`: one-way vs round trip, open-jaw and multi-city, families and bags, last-minute, several travelers on one booking, award options, price tracking.
- New description: also triggers on simple flight requests and on reviews of another agent's result; names near misses (hotels, itineraries, post-booking claims).
- New cards: compact card (Quick), basis column, multi-city card.

Repository:
- `scripts/check_parity.py` checks the English and Chinese skills, and the three READMEs, for matching heading structure, numbers and cross-references. Chinese cross-references now use section names.
- `evals/`: 8 test cases and 20 trigger queries.
- Documentation in the style of the anthropics/skills repository: `README.md`, `README.zh-CN.md`, `README.zh-TW.md`; `license` and `metadata.version` in the SKILL.md frontmatter; `LICENSE.txt` in each skill folder; `.claude-plugin/marketplace.json` for installing from Claude Code.

## 2.0.0 — 2026-10-24
- Repositioned as a **decision layer** on top of existing flight tools; search, price tracking, forecasts and award search are delegated
- Focus on four situations: uncertain plans, cash-only refunds, cross-border trips, booking for others
- Three modes (Quick / Standard / Thorough) instead of one heavy process
- Preference profiles with money exchange rates replace strict priority order
- New: true total cost, refundable-vs-credit break-even test, price dynamics, payment-card notes
- New: backward planning from the scarcest leg, post-booking check, self-contained handoff message, browser-tab etiquette
- Region rules moved to `references/regions.md`: global checks, North America module, starter checks for other regions
- English skill (`flight-trip-planning`) and Chinese skill (`flight-trip-planning-zh`)
- **Rename (recorded in 3.0.0):** v1's Chinese skill was named `flight-trip-planning`. In 2.0.0 that name went to the English skill and the Chinese skill became `flight-trip-planning-zh`. Users who installed v1 and copy the Chinese folder end up with both skills; see "Upgrading from v1" in the README.

## 1.0.0 — 2026-10-24
- Initial Chinese process skill: requirements doc, US/Canada domestic and cross-border conditions, refundability classes, verification, audit, output format
