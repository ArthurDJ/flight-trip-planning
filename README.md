# flight-trip-planning

A Claude skill that turns "help me book a flight" into a structured, auditable process: clarify requirements, write hard constraints and preferences, apply the right rules for **US domestic / Canada domestic / US–Canada cross-border** trips, classify fares by refundability, verify on airline sites, assess connections and time zones, and hand over a decision-ready result.

The skill defines **process and requirements only**. It does not prescribe any search tool, API or scraping method, so it works with whatever flight search capability your agent has.

> 中文说明见下方。The skill body is written in Chinese with English technical terms.

## Goal

Most flight skills answer "what is the cheapest flight?". This one answers **"which flight should we book, given everything that can go wrong?"**: plans that may be cancelled, refunds that must come back as cash, cross-border entry rules, tight connections, and time zones that silently shift. The output is something a person can act on and another agent can audit.

## Who it is for

- **People booking for someone else** (a partner, parent, colleague) who need a result the traveler can understand at a glance
- **Travelers with uncertain plans**: pending visas or immigration applications, job changes, moves between countries, where refundability matters more than price
- **US / Canada travelers**, especially cross-border trips between the two countries
- **Multi-agent setups** where one agent searches and another reviews (the skill includes an audit checklist and a merge rule)

**Not for**: automatic booking or payment, award / points optimization, hidden-city or self-transfer hacks, real-time fare tracking.

## How it differs from related projects

| Project type | Examples | What they do | What this skill adds |
|---|---|---|---|
| Search tools / MCP servers | Google Flights wrappers, ITA Matrix CLIs | Find flights and prices fast | Requirements doc, refund classification, official-site verification, connection and time-zone assessment |
| Price-hack skills | Hidden-city / hub-arbitrage finders | Cheapest possible routing, often separate tickets | The opposite stance: single PNR, protected connections, cancellable fares |
| All-in-one travel agents | Search + compare + book + track | Convenience and automation | Explicit hard constraints, an exclusion list, a query log, and a strict "never book or pay" boundary |
| Booking skills | Agent-driven reservation and payment | Complete the purchase | Human-only checkout; refunds and changes treated as first-class requirements |

These are complementary: use a search tool for **discovery**, and this skill for **deciding, verifying and auditing**.

## Requirements and configuration

| Item | Required? | Notes |
|---|---|---|
| An LLM agent that can load skills | Yes | Claude Code, Claude desktop / Claude.ai with custom skills, or any agent that accepts Markdown instructions |
| A way to read airline websites | Yes | Web browsing (e.g. a browser extension or headless browser) to confirm price, fare class and refund terms on the airline's own site |
| A flight discovery source | Recommended | Any search tool or MCP server; results are treated as leads only |
| File write access | Recommended | To save the versioned requirements doc and the result file |
| Web search | Recommended | To check current rules (entry requirements, passenger-rights rules, time-zone changes) at run time |
| A second agent or reviewer | Optional | For the audit step; the skill works single-agent too |
| Airline accounts / API keys | Not required | The agent must not log in, enter credentials, book or pay |

Inputs the agent will ask for: travelers and cabin, origin and acceptable alternative airports, departure and return windows (with time limits), maximum trip duration and stops, refund requirements and cancel triggers, baggage plan, passport nationality and entry documents, and preference order.

## What it covers

| Stage | Output |
|---|---|
| Clarify | Must-ask checklist: dates, airports, max duration, refund needs, bags, documents, preference order |
| Requirements doc | Versioned template with hard constraints (H) and ranked preferences (P) |
| Pre-checks | Passport, visa / eTA / ESTA / TRV, residency status, pending immigration applications, cancel triggers |
| Scope | Airline × airport × date grid based on real routes, with exclusion reasons |
| Verification | Third-party results for discovery only; price, fare class and refund terms confirmed on the airline site |
| Assessment | Connection feasibility (customs, preclearance, bag recheck), risk grades, time-zone rules |
| Output | Full result file (fixed sections) plus a short traveler-facing summary |
| Audit | Checklist for reviewing another agent's results |
| Handoff | Booking checklist for the human; the agent never books or pays |

### Scenario modules
- **US domestic**: REAL ID, DOT 24-hour rule and refund rights, Basic Economy limits, multiple time zones
- **Canada domestic**: APPR, carriers without refundable economy fares, half-hour and fixed time zones, winter disruption
- **US ⇄ Canada**: visa category vs. trip purpose, documents to return home, pending immigration risk, US preclearance vs. clearing CBP at the first US airport, CBSA bag recheck, credit usability after moving countries, unsynchronized time-zone rules
- **Other international** (brief): passport validity, transit visas, regional passenger-rights rules

### Fare refundability classes
- **A**: changeable + cancel refunds to original form of payment
- **B**: changeable + cancel returns airline credit only
- **C**: not changeable, or cancel forfeits value

Includes the details that usually get missed: "refundable" generally requires cancelling before departure; free changes still require paying the fare difference; premium cabins are not automatically refundable; "last N seats" refers to a fare bucket, not the aircraft.

## Install

**Claude Code / Claude desktop (local skills)**

```bash
git clone https://github.com/<owner>/flight-trip-planning.git
mkdir -p ~/.claude/skills
cp -r flight-trip-planning/skills/flight-trip-planning ~/.claude/skills/
```

**Claude.ai**: upload the `skills/flight-trip-planning` folder (zipped) as a custom skill in Settings.

**Other agents**: `SKILL.md` is plain Markdown with YAML front matter; load it as a system or task instruction.

## Usage examples
- "帮我规划圣诞节从温哥华飞洛杉矶的机票，要能退现金"
- "Review this flight search result against the requirements doc"
- "Write a requirements doc for a family trip Toronto → Orlando in March"

## Caveats
Rules on baggage fees, refunds, entry requirements and time zones change. The skill tells the agent to verify against official sources at run time and to label anything unverified. It is not legal or immigration advice.

## License
MIT

---

## 中文说明

一个把"帮我订机票"变成**可审核流程**的 Claude skill：澄清需求 → 需求文档（硬约束 H / 偏好 P）→ 证件与身份前置检查 → 查询范围 → 官网核实 → 转机、时差、行李评估 → 正式结果 + 旅客精简版 → 交叉审核 → 出票交接。

- 只做流程设计和需求补充，**不涉及具体查询方法**
- 内置**美国境内 / 加拿大境内 / 美加跨境**三类场景的限制条件清单
- 可退改分为 A（退原支付方式）/ B（只退 credit）/ C（不可改或作废）
- 执行边界：只查询，不下单、不付款、不输入账号或证件信息

**目标**：不只回答"哪班最便宜"，而是回答"考虑到可能取消、必须退现金、跨境入境、转机和时差之后，应该订哪一班"，结果既能直接给旅客看，也能被另一个 agent 审核。

**适用人群**：替家人或同事订票的人；计划不确定（签证、移民申请、搬家、换工作）的旅客；美加境内及跨境出行者；"一个 agent 查询、另一个 agent 审核"的多 agent 工作流。
**不适用**：自动下单付款、里程兑换优化、隐藏城市或自行转机等票价技巧、实时价格监控。

**需要的配置**：能加载 skill 的 agent（必需）；能访问航司官网的浏览能力（必需，用于核实价格、票种、退改条款）；机票搜索工具或 MCP（推荐，只用于发现航班）；文件写入（推荐）；网页搜索（推荐，用于执行时核实最新规则）；第二个审核 agent（可选）。不需要航司账号或 API key，agent 不得登录、下单或付款。

安装：把 `skills/flight-trip-planning` 文件夹复制到 `~/.claude/skills/`，或在 Claude.ai 设置里作为自定义 skill 上传。
