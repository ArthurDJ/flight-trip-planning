# flight-trip-planning

A Claude skill that turns "help me book a flight" into a structured, auditable process: clarify requirements, write hard constraints and preferences, apply the right rules for **US domestic / Canada domestic / US–Canada cross-border** trips, classify fares by refundability, verify on airline sites, assess connections and time zones, and hand over a decision-ready result.

The skill defines **process and requirements only**. It does not prescribe any search tool, API or scraping method, so it works with whatever flight search capability your agent has.

> 中文说明见下方。The skill body is written in Chinese with English technical terms.

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

安装：把 `skills/flight-trip-planning` 文件夹复制到 `~/.claude/skills/`，或在 Claude.ai 设置里作为自定义 skill 上传。
