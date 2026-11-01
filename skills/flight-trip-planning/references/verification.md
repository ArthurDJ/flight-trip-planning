# Verification and feasibility

## 1. Verify on the airline site

1. Use search tools and aggregators to **find** itineraries only.
2. For each shortlisted itinerary, open the **operating or ticketing airline's site**, search the same flights and dates, and record:
   - fare family / fare class shown (e.g. Saver, Main, Refundable, Flex)
   - total price incl. taxes, currency, and what it excludes (bags, seats)
   - change and cancellation terms, quoted briefly in the airline's words
   - query time
3. Drop any itinerary that cannot be found on the airline site, or whose price there differs and cannot be reconciled.
4. Mark the price type: **"from" price** (cheapest in the cabin, shown before selection) vs **selected total** (after choosing both legs). Only selected totals are final.
5. Suspiciously uniform results (several dates with identical prices) get one spot check on the airline's page before you rely on them.
6. If a login, verification code or identity check appears, enter nothing. Skip it or let the person handle it, and note it in the query log.

Single ticket: one booking, one PNR (booking reference), partner-operated legs allowed. Two separate bookings, even on the same airline, are **not** a single ticket. Aggregator "virtual interlining" and "hacker fare" combinations are usually separate tickets: no protection if a delay breaks the connection, and bags must be re-checked.

## 2. Backward planning

Coverage gaps usually come from searching only the obvious departure time.

1. Find the **scarcest leg** of each direction (e.g. a once-daily flight into a small airport).
2. Work backwards: which feeder flights reach the scarce leg with a comfortable buffer? That fixes the latest acceptable origin departure.
3. Search **every date** in the window and **every time of day** (early, midday, evening). Record which combinations were searched.
4. Note what happens if the scarce leg is missed: a few hours late, next day, or a hard-constraint violation.

## 3. Connection assessment

List the steps at each connection and estimate time:

1. Deplane (regional aircraft: wait for gate-checked carry-ons at the door)
2. Walk or take the airport train
3. Immigration / customs, if entering a country here
4. Reclaim and re-check bags, if required here
5. Security again, if required
6. Walk to the next gate; boarding closes ~15–20 min before departure

Risk grades (guidance):
- **Low**: normal time for the required steps + 30 min or more spare
- **Medium-low**: 15–30 min spare, or simple steps with a short window
- **Medium**: customs or bag re-check with < 30 min spare, or holiday / winter peak
- **High**: spare below normal time, or depends on an on-time arrival

Assess outbound and return separately: the consequence of a miss differs, so the weight differs. First-time entry on a new visa can trigger secondary inspection; leave buffer.

Advice for the traveler: carry-on only when possible, sit near the front, keep documents at hand, use the airline app for gate changes, contact the airline right away if the first leg is delayed.

## 4. Time zones

1. Check each place's UTC offset **for the travel dates** (daylight-saving switch dates; regions with fixed or changed rules).
2. Write every cross-zone time as "local (other place)".
3. State which zone a hard constraint uses ("arrive by 20:00 destination time").
4. Trip duration comes from the airline's elapsed time, not clock subtraction.
5. Give pickup times in the pickup person's local time.

## 5. Audit checklist (reviewing another agent's result)

- [ ] Uses the latest requirements version (no preferences quoted from an old version)
- [ ] Coverage: every acceptable airport, every date in the window, every time of day
- [ ] Every price verified on the airline site, with basis stated (taxes, bags, one-way/round-trip, refundable or not)
- [ ] Refundability classes correct; premium cabins not assumed refundable
- [ ] Arithmetic (differences, totals) correct
- [ ] Break-even uses P* = (R − C) / ((R − C) + f·C), with the inputs shown (not P·f·C > R − C)
- [ ] Time zones checked for the travel dates and shown in both zones
- [ ] Connections assessed for customs, bag re-check and scarce-leg risk
- [ ] Exclusion list, special-date conclusions and query log present
- [ ] No unsupported promises ("no fare difference", "you will make it")
- [ ] Uniform-looking data spot-checked

Reply point by point: valid / partly valid / not valid, with evidence and the fix. When merging two results, take the structure from the more complete one and the data from the one with wider verified coverage, and cite sources.
