---
name: flight-trip-planning
description: Decide which flight to book when plans may change, refunds must come back as cash, the trip crosses a border, or you are booking for someone else. Compares true total cost, flexibility, connection feasibility and documents, then recommends. Not a fare search engine.
---

# Flight decision skill

## What this skill is for

Search engines answer "what is the cheapest flight?". This skill answers **"which flight should we actually book?"** when the answer depends on things search engines ignore:

- **Plans that may change**: pending visas or immigration decisions, job or school dates, family events. Flexibility has a price, and this skill puts a number on it.
- **Refunds that must come back as cash**, not airline credit (for example, the traveler is moving countries or rarely flies that airline).
- **Cross-border trips**: entry documents, where customs happens, whether bags must be re-checked at the connection.
- **Booking for someone else** (a partner, parent, colleague): the traveler needs a short answer they can understand, and the booker needs something they can verify.

It turns these into hard filters and priced trade-offs, verifies the shortlist on the airline's own site, and hands over a decision. The person books and pays; the agent never does.

## What this skill does NOT do (use existing tools)

| Need | Use instead |
|---|---|
| Live fare search, cheapest dates, nearby airports | Google Flights (date grid, price graph), Skyscanner, Kayak |
| Price tracking and drop alerts | Google Flights / Google AI Mode "track prices", Kayak alerts |
| Buy-or-wait prediction, price freeze | Hopper, Kayak price forecast (not guaranteed) |
| Award / miles search | Airline sites, Google AI Mode miles, dedicated award tools |
| Deciding refund vs credit vs claim **after** booking | A post-booking refund-decision skill |

Treat these tools as **discovery**. Their results are leads, never final prices.

## Modes

Pick the lightest mode that fits. Escalate only when a trigger appears.

| Mode | When | Questions | Output |
|---|---|---|---|
| **Quick** (default for simple trips) | Domestic, fixed dates, no special constraints | 3–5 | Top 3 comparison card + recommendation |
| **Standard** | Any of: uncertain plans, cross-border, booking for others, more than 2 travelers | 6–10 | Comparison table, trade-off notes, traveler summary |
| **Thorough** | High stakes, many date/airport combinations, or a second agent will audit | Full intake | Versioned requirements doc, full result file, audit, booking record |

Escalation triggers: the traveler might cancel; a border is crossed; a connection depends on a once-a-day flight; immigration or visa status is pending; the person asks for a review of another agent's work.

## Workflow

1. **Intake.** Ask only what changes the decision. Use defaults for the rest and state them. See `references/templates.md` → Intake.
2. **Choose a preference profile** (or let the person write their own). Profiles say how much extra money is worth what. See `references/decision-framework.md` → Profiles.
3. **Set hard filters**: date and time windows (with the time zone named), maximum duration and stops, refundability class, baggage, single ticket, documents.
4. **Discover** candidates with the person's search tool. Cover every date in the window and every time of day; work backwards from the scarcest leg (see `references/verification.md` → Backward planning).
5. **Verify the shortlist on the airline site**: price, fare class, refund and change terms in the airline's words, query time. See `references/verification.md`.
6. **Normalize to true total cost** and **price the flexibility** (refundable vs credit vs basic). See `references/decision-framework.md`.
7. **Assess feasibility**: each connection step, border rules, documents, time zones, scarce-leg risk. Apply the region module that fits (`references/regions.md`).
8. **Decide and explain**: top 3 with the trade-off in money ("pays $27 more to arrive in daylight"), plus why the cheaper or closer options lost.
9. **Hand over**: traveler summary, a self-contained message for any other agent, and a booking checklist. After the person books, run the **post-booking check**.

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

- **Comparison card** (all modes): top 3, true total, refundability class, times in both zones, connection time and risk, one-line trade-off each.
- **Traveler summary** (Standard+): plain language, top 3 only, 3–5 notes (how to cancel, bags, documents).
- **Handoff message** (when another agent is involved): self-contained; never refer to files the other agent cannot open.
- **Full result file and audit** (Thorough).
- **Booking record** after the post-booking check.

Templates are in `references/templates.md`.

## Reference files

- `references/decision-framework.md`: true total cost, preference profiles, flexibility math, price dynamics, payment cards.
- `references/verification.md`: airline-site verification, backward planning, connection assessment, time zones, audit checklist.
- `references/regions.md`: global checks, North America module (US domestic, Canada domestic, US ⇄ Canada), other regions.
- `references/templates.md`: intake questions, requirements doc, comparison card, traveler summary, handoff message, booking and post-booking checklists.
