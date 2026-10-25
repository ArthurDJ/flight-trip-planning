# Decision framework

## 1. True total cost

Compare options on what the trip actually costs, not the headline fare.

| Component | Include when | Notes |
|---|---|---|
| Fare incl. taxes | Always | From the airline site, same itinerary, same fare class |
| Checked bags | The traveler will check bags on any leg | Per direction; price at the time bags are added; often cheaper online than at the airport |
| Carry-on | The fare excludes it (basic / ultra-low-cost fares) | |
| Seat selection | The traveler needs seats together or a specific seat | |
| Ground transport | Airports differ in distance from the real destination | Parking, rideshare, tolls, or a pickup driver's time |
| Time cost | The person names a value of time, or options differ by a night or a day | Only if the person agrees to a number |

Show the total and the parts. If a part is excluded, say so on the same line.

## 2. Preference profiles

A profile replaces a strict priority order (strict orders produce absurd results such as paying $359 to save a 10-minute drive). Each profile is a set of **exchange rates**: how much extra money is acceptable for an improvement.

| Profile | Price | Flexibility | Connection safety | Schedule comfort | Airport convenience |
|---|---|---|---|---|---|
| **Budget** | Dominant | Basic fares allowed if plans are firm | Accept legal minimum | Red-eyes and early starts OK | Low |
| **Balanced** (default) | High | Changeable fare | ≥ comfortable buffer | Avoid red-eyes | Medium |
| **Comfort** | Medium | Changeable | Generous buffer | Daytime, fewer stops | High |
| **Uncertain plans** | Medium | **Refundable to original payment, or a credit the traveler will surely use** | Generous buffer | Medium | Low |
| **Family / group** | Medium | Changeable | Generous buffer, no tight self-transfers | Avoid very early starts | High |

Default exchange rates for **Balanced** (the person may change any of them):
- Nonstop vs one stop: worth up to $100 per person
- Daytime arrival vs late night: worth up to $50
- Preferred airport vs another within the acceptable list: worth up to $40
- Connection buffer above the comfortable minimum: worth up to $30 per extra 30 minutes when the next flight is the last of the day

Write the trade-off in money for every recommendation: "Option B costs $27 more and arrives at 13:17 instead of 19:38."

## 3. Flexibility: refundable vs credit vs basic

Fare classes (names vary by airline; check each airline's fare families):

| Class | Change | Cancel |
|---|---|---|
| **A: Refundable** | Allowed, pay fare difference | Refund to original form of payment |
| **B: Changeable** | Allowed, pay fare difference | Airline credit (expiry and who may use it vary) |
| **C: Basic** | Not allowed or high fee | Forfeit or partial credit |

### Break-even test (A vs B)

```
premium          = price(A) − price(B)
credit_at_risk   = price(B) × (share of the credit you expect NOT to use before it expires)
choose A if      P(cancel) × credit_at_risk > premium
```

Example: premium $156, B price $501. If an unused credit would be fully wasted, A wins when P(cancel) > 31%. If the credit will almost surely be used, A rarely wins.

Questions that set `credit_at_risk`:
- Does this airline fly from where the traveler will live in the next 12 months?
- How long is the credit valid, and can it be used to book for someone else?
- Would the traveler **change** the date instead of cancelling? Changing a B fare usually avoids credit entirely (pay only the fare difference).

### Details people miss
- Refunds usually require cancelling **before departure**. A no-show can forfeit the refund.
- "Free change" means no change fee, not no fare difference. The difference is computed on the day you change.
- Premium economy, business and first are **not automatically refundable**; many airlines sell non-refundable versions.
- Partial use: whether an unused return leg is refunded depends on the fare rules.
- Many countries have a short free-cancellation window right after booking (e.g. the US 24-hour rule for bookings made directly with the airline). Use it as a final check.

## 4. Price dynamics

- "Last N seats" usually refers to a **fare bucket**, not the aircraft. When it sells out the price moves to the next bucket.
- Holiday fares tend to rise closer to departure; the size and timing are not knowable. Do not promise a direction.
- A refundable or changeable booking works as a hedge: book now to lock the price; if the fare drops, cancel (A) or change (B, if the airline refunds or credits the difference) and rebook. **Cancel and confirm the refund first, then rebook**, to avoid two bookings in one name.
- Delegate buy-or-wait forecasts and price tracking to tools built for it; mention their accuracy is not guaranteed.

## 5. Payment card

- Cash-back or points are real value only if the trip happens; refunds usually claw them back.
- Card trip-cancellation insurance generally covers **non-refundable** fares for limited reasons (illness, death, carrier insolvency), not "plans changed".
- Delay and baggage insurance on some cards can be worth more than cash back on winter itineraries with connections.
- Do not route a booking through a card's travel portal for extra points if that breaks the "book on the airline site" rule.
