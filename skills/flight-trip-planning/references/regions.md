# Region modules

Rules change. Check them at run time against official sources and record the date. Facts with a date are in the table below; when a fact's "verified on" date is old or the trip is close, recheck it before relying on it.

## Dated facts

| Fact | Value | Source | Verified on |
|---|---|---|---|
| British Columbia time | Permanent UTC-7 ("Pacific time") from 2026-11-01; the last clock change was 2026-03-08. Exceptions: northeastern BC stays on UTC-7; southeastern BC (East Kootenay, Golden) still switches between UTC-7 and UTC-6. In winter, Vancouver is 1 hour ahead of Seattle | BC government release 2026AG0013-000209 (news.gov.bc.ca) | 2026-11-01 |
| US preclearance airports in Canada | Vancouver, Calgary, Edmonton, Winnipeg, Toronto Pearson, Toronto Billy Bishop, Ottawa, Montréal, Halifax | Public Safety Canada, "Preclearance in Canada and the United States" (publicsafety.gc.ca) | 2026-11-01 |
| After preclearance | Travelers skip CBP and TSA on arrival in the US and go straight to the connection or exit | CBP, "Learn About Preclearance" (cbp.gov) | 2026-11-01 |
| Arriving in the US from abroad without preclearance | Clear CBP at the first US airport, connecting travelers included; claim checked bags, clear customs, then re-check them. A CBP pilot (International Remote Baggage Screening, from 2025-04-08) lets some routes skip the re-check | Port of Seattle, "International connections" (portseattle.org); CBP release of 2025-04-08 (cbp.gov) | 2026-11-01 |
| US refund on airline cancellation or significant change | A full refund, including for non-refundable tickets, if the traveler does not accept the change or rebooking; a credit or voucher only if the traveler chooses it instead. "Significant change": departure at least 3 h earlier or arrival at least 3 h later (domestic), 6 h (international); also a different airport, more connections, or a lower class of service | 14 CFR 260.2 and 260.6 | 2026-11-01 |
| US 24-hour rule | The airline must hold the reservation without payment, or allow cancellation without penalty, for at least 24 hours, if booked one week or more before departure | 14 CFR 259.5(b)(4) | 2026-11-01 |
| EU Entry/Exit System (EES) | Fully operational since 2026-04-10 (phased start 2025-10-12): non-EU travelers are registered at the first Schengen border | European Commission, Home Affairs, "Smart borders" | 2026-11-01 |
| EU ETIAS | Not in operation; no start date announced | European Commission, Home Affairs, "Smart borders" | 2026-11-01 |

DOT enforcement of parts of the refund rule has been adjusted since it took effect: check the current position at run time before promising a refund.

## Global checks (any trip that crosses a border)

- **Passport validity**: many countries require validity beyond the stay (often 6 months).
- **Entry permission** for the traveler's nationality: visa, eTA / ESTA-type authorization, or visa-free. The visa category must match the trip purpose.
- **Transit**: some countries require a transit visa even airside; changing airports or terminals overnight may require entry. A connection through a US airport means clearing CBP there (see Dated facts), so check US entry documents for the traveler's nationality at run time.
- **Return home**: documents needed to re-enter the country of residence must be valid on the return date (residence card, permits, re-entry visa).
- **Pending applications**: ask whether leaving the country affects an immigration or visa application in progress, or a status maintained by it. If an approval could force a cancellation, write it as a cancel trigger and push the profile to "Uncertain plans".
- **Where customs happens**: at the first point of entry, unless the departure airport offers preclearance. Bags may have to be reclaimed and re-checked at that point.
- **Passenger-rights regimes** differ by country and apply per flight (e.g. US DOT rules, Canada's APPR, EU261).
- **Currency**: fares and bag fees are priced in the origin's currency; the return may differ.
- **Time zones** do not switch on the same dates everywhere, and some regions have stopped switching (see Dated facts).

## North America module

### US domestic
| Topic | Check |
|---|---|
| ID | REAL ID-compliant ID or passport for adults; non-US travelers carry a passport |
| Customs | None; connections stay airside |
| Refund rights | 24-hour rule (see Dated facts); a refund if the airline cancels or significantly changes the flight and the traveler declines the alternative, even on a basic fare. This is a route to cash in the "refund must be cash" situation |
| Fares | Basic economy often no changes, may exclude carry-on or seats; Main usually changeable with credit on cancel; refundable fares sold separately |
| Bags | Usually extra; often cheaper prepaid online |
| Time zones | 4 mainland zones plus Alaska and Hawaii; most of Arizona does not observe DST |
| Connections | 45–90 min typical domestic (guidance, not a rule); winter disruption at northern hubs |

### Canada domestic
| Topic | Check |
|---|---|
| ID | Government-issued ID for adults; foreign travelers carry a passport |
| Customs | None |
| Refund rights | APPR: refund to original payment when the airline cancels and cannot rebook within the required time; compensation depends on cause and airline size |
| Fares | Some carriers have **no refundable economy fare**; only premium fare families refund to the original payment |
| Bags | Low fares may exclude carry-on; compare total cost with ultra-low-cost carriers |
| Time zones | 6 zones incl. Newfoundland (half hour); most of Saskatchewan no DST; British Columbia on permanent UTC-7 (see Dated facts) |
| Connections | Fog and snow at smaller airports; few daily frequencies, a missed connection may mean next day |

### US ⇄ Canada
| Topic | Check |
|---|---|
| Documents | Passport (or eligible alternatives by nationality); US visa / ESTA and Canadian TRV / eTA by nationality; visa category matches purpose |
| Return to Canada | Non-citizen residents: PR card / PRTD, or work / study permit plus a valid TRV if required; airlines check before boarding |
| Pending immigration | Effect of travel on a pending application or maintained status; timing of landing vs travel; write cancel triggers |
| Into the US | From a preclearance airport (see Dated facts): clear US customs and immigration before departure; no CBP or TSA on arrival. Connecting at a Canadian preclearance hub onto a US-bound flight (e.g. Saskatoon → Toronto → Chicago): preclearance happens at the hub and is a connection step. From other Canadian airports: clear CBP at the first US airport, then security again |
| Into Canada | Clear CBSA at the first Canadian airport; on international-to-domestic connections some airports require reclaiming and re-checking bags. From outside North America via a US hub: the traveler clears CBP at that hub (US entry documents needed), claims and re-checks bags there unless a CBP pilot applies, then clears CBSA in Canada |
| Refund rights | US 24-hour rule applies to flights to or from the US; APPR applies to flights to, from and within Canada; third-party sites may not honor the 24-hour rule |
| Credits | If the traveler is moving countries, check whether the airline flies from the new home |
| Time zones | Provinces and states switch on different rules, and British Columbia no longer switches; recheck the offset for the travel dates |

## Other regions (starter checks)
- **EU / UK**: EU261 / UK261 passenger rights; Schengen 90/180 rule; EES registration at the first Schengen entry adds border time, so a connection there needs more buffer; ETIAS not yet required (see Dated facts); UK ETA where in force, verify at run time
- **Asia-Pacific**: transit visa rules at hubs; long layovers may need entry; low-cost carrier bag and seat fees
- **Latin America**: tourist cards and onward-ticket checks at boarding; currency of fares

Add a full module when a trip needs it, using the North America tables as the template.
