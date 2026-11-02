English | [简体中文](README.zh-CN.md) | [繁體中文](README.zh-TW.md)

> **Note:** This repository contains a skill for Claude. For information about the Agent Skills standard, see [agentskills.io](https://agentskills.io).

# flight-trip-planning

A skill that helps Claude decide **which flight to book** when the cheapest one is not the right answer. It sits on top of flight search tools: it turns their results into a decision that someone else can check, verifies prices on the airline's own site, and puts a money value on flexibility. It never books, pays or logs in.

The repository ships the same skill in two languages: `skills/flight-trip-planning` (English instructions) and `skills/flight-trip-planning-zh` (Chinese instructions). Install one of them.

For more information about skills, check out:
- [What are skills?](https://support.claude.com/en/articles/12512176-what-are-skills)
- [Using skills in Claude](https://support.claude.com/en/articles/12512180-using-skills-in-claude)
- [How to create custom skills](https://support.claude.com/en/articles/12512198-creating-custom-skills)

# What the skill does

Search engines answer "what is the cheapest flight?". This skill answers "which flight should we actually book?", and it is built for four situations that search engines ignore:

| Situation | What the skill does |
|---|---|
| **Plans may change** (pending visa or immigration decision, job or school dates, family events) | Prices flexibility with a break-even test: refundable vs changeable vs basic |
| **Refunds must come back as cash** (moving countries, rarely flying that airline) | Separates "refund to the original payment" from "airline credit" and checks whether a credit would really be used |
| **Cross-border trips** | Checks entry documents, where customs happens, US preclearance, bag re-check at connections, once-a-day legs and time zones |
| **Booking for someone else** | Writes a plain-language summary for the traveler and a verifiable result for the booker |

Every recommendation states its trade-off in money ("$27 more to arrive in daylight"), so it can be checked and argued with.

## When it triggers

- Choosing between flights or fare types (basic, main, refundable), including simple requests such as "find me a reasonable flight from X to Y"
- Checking whether a connection or a cross-border itinerary works
- Reviewing flight options that another person or agent found

## When it does not

- Hotels or full trip itineraries
- Refund, credit or compensation claims on a flight that is already booked or was cancelled by the airline
- Live fare search, price tracking and buy-or-wait forecasts (use Google Flights, Kayak, Skyscanner or Hopper; the skill uses their results as leads)
- Points optimization and fare tricks such as hidden-city or "hacker fare" tickets

# Modes

| Mode | Used for | Questions | Output |
|---|---|---|---|
| **Quick** | Domestic trips with fixed dates and no special constraints | 0–2, only what the message leaves out | A compact card with the top 3 and one recommendation, about 250 words |
| **Standard** | Uncertain plans, cross-border trips, booking for others, more than 2 travelers | 6–10 | Comparison table, trade-offs, a summary for the traveler |
| **Thorough** | High stakes, many date and airport combinations, or a second agent auditing the result | Full intake | Versioned requirements doc, full result file, audit checklist, booking record |

# Preference profiles

Instead of a strict priority order, each profile is a set of exchange rates: how much extra money an improvement is worth. There are five profiles: **Budget**, **Balanced** (default), **Comfort**, **Uncertain plans** and **Family / group**. For example, Balanced values a nonstop at up to $100 per person and a daytime arrival at up to $50. Every rate can be changed.

Fare flexibility is decided by a break-even test, not a rate. With a refundable fare at $657 and a changeable fare at $501 whose credit would go unused, the refundable fare wins when the chance of cancelling is above 156 / 657 ≈ 23.7%.

# Example prompts

- "Booking Vancouver to Beijing for my wife. Standard is $501 with credit on cancel, Latitude is $657 refundable. About a 25% chance her visa is late, and we're moving to Germany. Which fare?"
- "Is a 75-minute connection in Toronto OK going Saskatoon to Chicago on one Air Canada ticket?"
- "need a flight SFO to LAX next friday morning, carry-on only, just something reasonable"
- "We're 2 adults and 2 kids with 4 checked bags, Toronto to Cancún. Which of these three options is really cheapest?"
- "Another agent prepared this flight plan. Please check it before I book."

# Try in Claude Code, Claude.ai, and the API

## Claude Code

Register this repository as a plugin marketplace in Claude Code:

```
/plugin marketplace add ArthurDJ/flight-trip-planning
```

Then install one language version:

```
/plugin install flight-trip-planning@flight-trip-planning
/plugin install flight-trip-planning-zh@flight-trip-planning
```

Or copy a skill folder into your personal skills directory:

```bash
git clone https://github.com/ArthurDJ/flight-trip-planning.git
mkdir -p ~/.claude/skills
cp -r flight-trip-planning/skills/flight-trip-planning ~/.claude/skills/      # English
cp -r flight-trip-planning/skills/flight-trip-planning-zh ~/.claude/skills/   # or Chinese
```

## Claude.ai

Zip one skill folder (`flight-trip-planning` or `flight-trip-planning-zh`) and upload it as a custom skill. See [Using skills in Claude](https://support.claude.com/en/articles/12512180-using-skills-in-claude).

## Claude API

Custom skills can be uploaded and used through the Claude API. See the [Skills API Quickstart](https://docs.claude.com/en/api/skills-guide#creating-a-skill). The API only gives the skill what your application provides: without a web-browsing tool, the skill labels every price as unverified.

# Requirements

| Item | Required? | Notes |
|---|---|---|
| An agent that can load skills | Yes | Claude Code, Claude.ai custom skills, the Claude API, or any agent that reads Markdown instructions |
| Web browsing | Strongly recommended | To verify price, fare class and refund terms on the airline site. Without it, the skill works from results you paste and labels every price unverified |
| A fare search source | Recommended | Any search tool or MCP server, or results you paste |
| Web search | Recommended | To recheck time-sensitive rules (entry rules, passenger rights, time zones) at run time |
| File write | Optional | For the requirements doc and result file in Thorough mode |
| Airline accounts or API keys | Not needed | The agent never logs in, books or pays |

# Limitations and safety

- **Query only.** The agent never books, pays, holds seats, logs in or enters credentials. It skips login and verification pages and leaves the booking to you.
- **Prices are leads until verified** on the airline's own site. Without a browser, nothing is verified, and the skill says so.
- **Rules change.** Baggage fees, refund policies, entry requirements and time zones change. Time-sensitive facts carry a "verified on" date and a source in `references/regions.md`, and the skill rechecks them at run time when it can.
- **North America is covered in depth**; other regions have starter checklists.
- Not legal, immigration or financial advice.

# Upgrading from v1

Version 1 was a Chinese skill named `flight-trip-planning`. Since version 2, that name belongs to the **English** skill, and the Chinese skill is `flight-trip-planning-zh`.

- If you installed v1 and want Chinese: delete `~/.claude/skills/flight-trip-planning` (the old v1 copy), then install `flight-trip-planning-zh`. Otherwise you keep the outdated v1 rules next to the new skill, and both can trigger.
- If you want English: replace `~/.claude/skills/flight-trip-planning` with the new folder.
- On Claude.ai: delete the old custom skill before uploading the new one.
- To see which version is installed, look for `version` under `metadata` in the SKILL.md frontmatter. v1 and v2 have no version there.

# Repository layout

```
.claude-plugin/marketplace.json   Claude Code plugin marketplace
skills/
  flight-trip-planning/           English skill
    SKILL.md                      workflow by mode, scope, rules, outputs
    references/
      decision-framework.md       true total cost, profiles, break-even test, price dynamics
      verification.md             airline-site checks, connection time, connections, time zones, audit
      regions.md                  dated facts with sources, North America module, other regions
      scenarios.md                one-way vs round trip, multi-city, families, last-minute, several travelers
      templates.md                intake, cards, traveler summary, handoff, booking checklists
    LICENSE.txt
  flight-trip-planning-zh/        Chinese skill, same structure
evals/                            test cases and trigger queries
scripts/check_parity.py           checks that the English and Chinese files match
scripts/validate_skills.py        checks SKILL.md frontmatter, file references, marketplace.json and evals
.github/                          CI workflow, issue forms, pull request template
```

# Testing

- `python scripts/check_parity.py` compares the English and Chinese skill files, and this README with `README.zh-CN.md` and `README.zh-TW.md`: heading structure, every number, and cross-references. It exits with an error on any mismatch.
- `evals/` holds 8 test cases in the `claude plugin eval` layout and `evals/trigger-queries.json` with 20 queries for testing the description. To run the cases against one language version, copy `evals/` into that skill folder and run `claude plugin eval .` from there.
- `python scripts/validate_skills.py` checks both SKILL.md files against the [Agent Skills specification](https://agentskills.io/specification) (the name matches the folder, description length, `license`, string-only `metadata`), that every `references/` path and relative link in a skill exists, that `.claude-plugin/marketplace.json` lists every skill folder by a path that exists, that the version is the same in both SKILL.md files, `marketplace.json` and `CHANGELOG.md`, and that every eval case has `prompt.md`, `case.yaml` and graders. It exits with an error on any problem.

# Contributing

GitHub Actions runs both scripts on every pull request and on every push to `main` (`.github/workflows/ci.yml`). They need only Python and its standard library, so run them before pushing:

```bash
python scripts/check_parity.py
python scripts/validate_skills.py
```

Change the English and Chinese skills, and the three READMEs, in the same pull request. A rule change needs an official source and a new "verified on" date in the "Dated facts" table; the pull request template has the full checklist. To report a rule that is out of date or a skill that loads when it should not (or does not load when it should), use the issue forms.

# License

MIT. See [LICENSE](LICENSE); each skill folder also contains a copy as `LICENSE.txt`.
