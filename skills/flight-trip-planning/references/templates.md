# Templates

## 1. Intake

Ask only what changes the decision. Offer the default in brackets so the person can just say "yes".

**Quick mode**: what you need to know. Take the answers from the message; ask only the ones it leaves out (usually 0–2) and state the defaults you used.
1. Who is flying, from where to where, which dates? [1 adult, economy]
2. How likely is it that the trip changes or is cancelled? [low → Balanced profile]
3. Bags each way? [carry-on only]
4. Any airports or times that are off-limits? [none]
5. Budget ceiling? [none]

**Standard adds**
6. If cancelled, is airline credit acceptable, and would it be used within a year? → sets refundability class
7. Who books and who travels? Will the traveler read the result? → traveler summary needed
8. Nationality and documents for any border crossed; any pending visa or immigration decision → cancel triggers
9. Acceptable alternative airports and the ground trip from each
10. Profile choice, or the person's own exchange rates

**Thorough adds**: per-date time limits, maximum duration and stops, who audits, output format.

## 2. Requirements doc (Thorough)

```
# <Trip> requirements  vX.Y (date, change summary)
Context: why flexibility / refunds / documents matter

## Hard filters
H1 Departure window (zone)
H2 Return window; latest arrival (zone)
H3 Max stops and max door-to-door duration
H4 Refundability class (A / B / C) and when B is acceptable
H5 Baggage plan
H6 Ticketing: single booking (one PNR), booked on the airline site
H7 Connection feasibility (customs, bag re-check, scarce leg)
H8 Documents (complete before ticketing or before departure; say which)

## Profile and exchange rates
## Airlines and airports in scope (with exclusion reasons)
## Time-zone rule
## Output format
## Boundaries: query only; the person books and pays
```

Each filter must be checkable (a number, a zone, a method). Filters must not contradict each other.

## 3. Comparison cards

### Compact card (Quick)

At most 3 options, one block each:

```
1. <airline and flight> · <total> (<fare> + <bags>) · <fare class, change / cancel in a few words>
   <day dep → arr>, <nonstop | 1 stop in X, connection length>
   Trade-off: <one line in money>
```

Times in one zone when both airports share it; otherwise "local (other)". Then the recommendation in one sentence. No exclusion list unless the person asks.

### Comparison card (Standard and Thorough)

| # | Flights (operator) | Out: dep → arr (both zones) | Back: dep → arr (both zones) | Connection / risk | Class | Basis | Fare | Bags | **Total** | Trade-off |
|---|---|---|---|---|---|---|---|---|---|---|

Basis: one-way or round trip, one booking or separate bookings. Then: the recommendation in one sentence, why the next two lost in money terms, what was excluded and why (short).

### Multi-city and open-jaw card

| # | Segment 1: dep → arr | Segment 2: dep → arr | Segment 3: dep → arr | Ground legs (time, cost) | Booking (one or separate) | Class | **Total** | Trade-off |
|---|---|---|---|---|---|---|---|---|

Add or drop segment columns as needed. The ground legs between open-jaw points are part of the true total.

## 4. Traveler summary (Standard+)

- Title: dates and route in plain words
- One line: time-zone difference, if any
- Top 3 options, each: price incl. bags, out and back times in both zones, connection length, 1–2 tags ("cheapest", "tight return connection")
- 3–5 notes: how and when to cancel, bags, name must match passport, documents to check
- No matrices, exclusion lists or jargon

## 5. Handoff message to another agent

Self-contained. Never point to files the other agent cannot open.
Include: key requirements, time-zone rule, recommendation with numbers, price matrix summary, connection risks, exclusions with reasons, corrections to that agent's earlier work, open items.

## 6. Full result file (Thorough)

Fixed sections, "none" when empty:
1. Conclusion (top 3, reasons)
2. Candidate table (fields as the comparison card plus link and fare class)
3. Price matrix (departure date × return date, per airport) and cabin comparison if relevant
4. Connection assessment per option
5. Exclusion list (constraint violated or reason)
6. Special-date conclusions ("solution" / "no solution" with evidence)
7. Query log (sites, combinations, times, logins skipped, unverified items)

## 7. Booking checklist (for the person)

1. Open the airline site in your own browser and select the exact flights. If the agent could not verify on the airline site, check the price, fare class and bags there now
2. Choose the fare family that matches the decision (and the refundable add-on if chosen); confirm the total
3. Passenger name exactly as in the passport
4. Choose the payment card (refunds return to it)
5. Screenshot the change and cancellation terms before paying
6. Bags can usually be added later in "Manage trip"

## 8. Post-booking check

From the **confirmation page or email** (a checkout page is not proof of booking):
- [ ] Confirmation code present
- [ ] Flights, dates and times match the decision
- [ ] Total matches the expected fare class (a refundable fare's price confirms the class)
- [ ] Full passenger name matches the passport
- [ ] Change and cancellation policy text matches expectations
- [ ] Free-cancellation window end time noted
- [ ] Record appended to the result file
- [ ] Files containing payment details deleted or kept private
- [ ] Reminders: add bags before departure; cancel **before departure** if a cancel trigger fires; consider price tracking for a cancel-and-rebook opportunity
