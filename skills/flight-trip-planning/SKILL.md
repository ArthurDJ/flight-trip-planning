---
name: flight-trip-planning
description: Use this skill whenever someone needs to decide which flight or fare to book, compare flight options or fare types (basic vs main vs refundable), check whether a connection or a cross-border itinerary works, or review flight options that another person or agent found. This includes simple requests like "find me a reasonable flight from X to Y". It matters most when plans may change, a refund must come back as cash, the trip crosses a border (US/Canada preclearance, customs, bag re-check), or someone is booking for another person. It verifies third-party prices on the airline's site, prices flexibility in money, and never books or pays. Not for hotels or full trip itineraries, and not for refund or compensation claims on a flight already booked or cancelled.
---

# Flight decision skill

## What this skill is for

Search engines answer "what is the cheapest flight?". This skill answers **"which flight should we actually book?"**, for simple trips (Quick mode) and especially when the answer depends on things search engines ignore:

- **Plans that may change**: pending visas or immigration decisions, job or school dates, family events. Flexibility has a price, and this skill puts a number on it.
- **Refunds that must come back as cash**, not airline credit (for example, the traveler is moving countries or rarely flies that airline).
- **Cross-border trips**: entry documents, where customs happens, whether bags must be re-checked at the connection.
- **Booking for someone else** (a partner, parent, colleague): the traveler needs a short answer they can understand, and the booker needs something they can verify.

It turns these into hard filters and priced trade-offs, verifies the shortlist on the airline's own site, and hands over a decision. The person books and pays; the agent never does.

## Scope

**Handled here**, with details in `references/scenarios.md`: one-way vs round trip, open-jaw and multi-city, families with bags, last-minute trips, several travelers on one booking, auditing another agent's result.

**Not handled here.** Say so in one line and point to the right tool:

| Need | Use instead |
|---|---|
| Live fare search, cheapest dates, price tracking, buy-or-wait forecasts | Google Flights, Kayak, Skyscanner, Hopper (forecasts are not guaranteed) |
| Award and points optimization | Airline sites and award tools; an award option the person already has can still be compared (`references/scenarios.md` → Award and points options) |
| Hidden-city, "hacker fare" or self-transfer tricks | Excluded by default |
| Hotels, full itineraries | Out of scope |
| Refund, credit or compensation claims **after** booking or after an airline cancellation | A post-booking refund-decision skill; mention the airline-cancellation refund right if it applies (`references/regions.md` → Dated facts) |

Treat search tools as **discovery**. Their results are leads, never final prices.

## Modes

Pick the lightest mode that fits. Escalate only when a trigger appears.

| Mode | When | Questions | Output |
|---|---|---|---|
| **Quick** (default for simple trips) | Domestic, fixed dates, no special constraints | 0–2: only what the message leaves out | Compact card (top 3) + recommendation |
| **Standard** | Any of: uncertain plans, cross-border, booking for others, more than 2 travelers | 6–10 | Comparison table, trade-off notes, traveler summary |
| **Thorough** | High stakes, many date/airport combinations, or a second agent will audit | Full intake | Versioned requirements doc, full result file, audit, booking record |

Escalation triggers: the traveler might cancel; a border is crossed; a connection depends on a once-a-day flight; immigration or visa status is pending; the person asks for a review of another agent's work.

## Workflow by mode

| Step | Quick | Standard | Thorough |
|---|---|---|---|
| 1. Intake (`references/templates.md` → Intake) | Infer from the message; ask only what is missing and changes the decision; state defaults | Standard questions | Full intake + requirements doc |
| 2. Profile (`references/decision-framework.md` → Preference profiles) | Balanced unless the person says otherwise | Choose with the person | Choose, write the exchange rates down |
| 3. Hard filters | Dates, times, bags | + refundability, documents, single ticket | + per-date limits, max duration and stops |
| 4. Discover | One search pass for the given dates | Every date and time of day; backward planning (`references/verification.md` → Backward planning) | + every acceptable airport; price matrix |
| 5. Verify on the airline site (`references/verification.md` → Verify on the airline site) | Recommended option and runner-up | Every shortlisted option | Every shortlisted option + query log |
| 6. True total and flexibility (`references/decision-framework.md`) | Fare + bags; break-even only if plans may change | Full true total; break-even test | Same, with inputs written out |
| 7. Feasibility (`references/verification.md` → Connection assessment, `references/regions.md`) | Connection steps, only if there is a connection | + border rules, documents, time zones | Same, both directions weighed |
| 8. Decide and explain | Top 3, trade-off in money | Same + why cheaper or closer options lost | Same |
| 9. Hand over (`references/templates.md`) | Compact card | + traveler summary, booking checklist | + full result file, audit, post-booking check |

Quick mode stays short. Budget: about 250 words in total, made of the compact card, a one-sentence recommendation, at most 2 questions (only ones whose answer would change the pick) and at most 2 short notes. No requirements doc, no region module unless a rule applies, and no booking checklist: offer it in one line instead.

## When tools are missing

- **No flight search tool**: ask the person to paste results, links or screenshots, and work from those.
- **No browser**: prices cannot be checked on the airline site. Label every price "provided by you, not verified on the airline site", say what could change (fare class, bags, taxes), and give the booking checklist (`references/templates.md` → Booking checklist) so the person checks before paying.
- **No web search**: use the rules in `references/regions.md` with their "verified on" dates and say that they must be rechecked.
- In every case, do not fill gaps from memory: no invented flight numbers, times, prices or fees. A typical schedule can be described only as "typical, unverified".

## Core rules

1. **Conclusion first.** Every deliverable opens with the recommendation.
2. **Never book, pay, hold seats, log in or enter credentials.** Skip login and verification pages; leave them to the person.
3. **Never take over a browser tab the person is using.** Open a new tab and close it when done.
4. **Third-party results are leads.** Final price, fare class and refund terms come from the airline's own site for the same itinerary.
5. **Same unit or no comparison.** Do not put one-way and round-trip, refundable and non-refundable, or with-bags and without-bags prices in one column without a "basis" column.
6. **State confidence** (high / medium / low) and the reason for every inference; write "unverified" instead of guessing.
7. **Do not relax a hard filter silently.** Report "no result" and why; the person decides.
8. **Rules change.** Baggage fees, refund policies, entry rules and time zones are checked at run time and the query time is recorded.
9. **Two time zones, always.** Any departure or arrival that crosses zones is written "local time (other place's time)".

## Outputs

- **Compact card** (Quick): top 3, true total, refundability class, times, connection, one-line trade-off each.
- **Comparison card** (Standard+): adds both time zones, connection risk and the basis of each price.
- **Traveler summary** (Standard+): plain language, top 3 only, 3–5 notes (how to cancel, bags, documents).
- **Handoff message** (when another agent is involved): self-contained; never refer to files the other agent cannot open.
- **Full result file and audit** (Thorough).
- **Booking record** after the post-booking check.

Templates are in `references/templates.md`.

## Reference files

- `references/decision-framework.md`: true total cost, preference profiles with exchange rates, the break-even test, price dynamics, payment cards.
- `references/verification.md`: airline-site verification, minimum connection time, backward planning, connection assessment, time zones, audit checklist.
- `references/regions.md`: dated facts with sources, global checks, North America module (US domestic, Canada domestic, US ⇄ Canada), other regions.
- `references/scenarios.md`: one-way vs round trip, open-jaw and multi-city, families and bags, last-minute, several travelers, award options, price tracking.
- `references/templates.md`: intake questions, requirements doc, compact and comparison cards, multi-city card, traveler summary, handoff message, booking and post-booking checklists.
