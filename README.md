# flight-trip-planning

**A Claude skill for deciding which flight to book when the cheapest one is not the right answer.**

Built for four situations that flight search engines ignore:

| Situation | What the skill does |
|---|---|
| **Plans may change** (pending visa or immigration decision, job or school dates, family events) | Prices flexibility: refundable vs credit vs basic, with a break-even test |
| **Refunds must come back as cash** (moving countries, rarely flying that airline) | Separates "refund to original payment" from "airline credit" and checks whether a credit would really be used |
| **Cross-border trips** | Checks entry documents, where customs happens, preclearance, bag re-check at connections, once-a-day legs, time zones |
| **Booking for someone else** | Produces a plain-language summary for the traveler and a verifiable result for the booker |

It does **not** search fares, predict prices or book tickets. It sits on top of the tools that do, and turns their results into a decision.

> 中文说明见下方。The repo ships an English skill and a Chinese skill with the same content.

## How it works

1. **Ask only what changes the decision**, with defaults (3–5 questions in Quick mode)
2. **Pick a preference profile**: Budget, Balanced, Comfort, Uncertain plans, Family. Each profile states how much extra money is worth a nonstop, a daytime arrival, a closer airport or a safer connection
3. **Set hard filters**: dates and times (with time zones), duration, stops, refundability, bags, single ticket, documents
4. **Discover** with your search tool, then **verify the shortlist on the airline's own site** (price, fare class, refund terms)
5. **Compare true total cost** (fare, bags, seats, ground transport) and **price the flexibility**
6. **Check feasibility**: each connection step, border rules, scarce legs, both time zones
7. **Recommend the top 3** with trade-offs in money ("$27 more to arrive in daylight")
8. **Hand over**: traveler summary, booking checklist, and a **post-booking check** of the confirmation

Three modes: **Quick** (simple domestic trips), **Standard** (any of the four situations above), **Thorough** (requirements doc, full result file, second-agent audit).

## Use it with, not instead of

| Need | Recommended tool |
|---|---|
| Fare search, cheapest dates, nearby airports | Google Flights, Skyscanner, Kayak |
| Price tracking and alerts | Google Flights / Google AI Mode, Kayak |
| Buy-or-wait forecasts, price freeze | Hopper, Kayak |
| Award and miles search | Airline sites, Google AI Mode, award tools |
| Refund vs credit vs claim **after** booking | A post-booking refund-decision skill |

Aggregator results are treated as leads. Separate-ticket "hacker fares", virtual interlining and hidden-city tickets are excluded by default.

## Who it is for

- People booking for a partner, parent or colleague
- Travelers whose plans depend on a pending decision
- US / Canada travelers, including cross-border trips (a full North America module is included; other regions have starter checklists)
- Multi-agent setups where one agent searches and another audits

**Not for**: automatic booking or payment, points optimization, fare hacks, live price monitoring.

## Requirements

| Item | Required? | Notes |
|---|---|---|
| An agent that can load skills | Yes | Claude Code, Claude desktop / Claude.ai custom skills, or any agent that reads Markdown instructions |
| Web browsing | Yes | To verify price, fare class and refund terms on the airline site |
| A fare search source | Recommended | Any search tool or MCP server |
| Web search | Recommended | To check current rules (entry, passenger rights, time zones) at run time |
| File write | Optional | For the requirements doc and result file (Thorough mode) |
| Airline accounts or API keys | Not needed | The agent never logs in, books or pays |

## Install

```bash
git clone https://github.com/ArthurDJ/flight-trip-planning.git
mkdir -p ~/.claude/skills
# English
cp -r flight-trip-planning/skills/flight-trip-planning ~/.claude/skills/
# or Chinese
cp -r flight-trip-planning/skills/flight-trip-planning-zh ~/.claude/skills/
```

Install one language version. On Claude.ai, zip the folder and upload it as a custom skill.

## Repository layout

```
skills/
  flight-trip-planning/        English
    SKILL.md                   workflow, modes, rules, outputs
    references/
      decision-framework.md    true total cost, profiles, flexibility math, price dynamics, cards
      verification.md          airline-site checks, backward planning, connections, time zones, audit
      regions.md               global checks, North America module, other regions
      templates.md             intake, requirements doc, comparison card, traveler summary, handoff, booking checks
  flight-trip-planning-zh/     Chinese, same structure
```

## Caveats

Baggage fees, refund policies, entry requirements and time-zone rules change. The skill tells the agent to verify against official sources at run time and to label anything unverified. This is not legal, immigration or financial advice.

## License

MIT

---

## 中文说明

**当"最便宜"不是正确答案时，用来决定该订哪张机票的 Claude skill。**

专门处理搜索引擎不管的四种情况：

| 情况 | skill 做什么 |
|---|---|
| **计划可能变**（签证或移民审批未定、工作学业日期、家庭安排） | 给灵活性定价：可退 vs credit vs 基础票，附盈亏平衡判断 |
| **退款必须退现金**（要搬家、很少再坐这家航司） | 区分"退回原支付方式"和"航司 credit"，并判断 credit 是否真用得上 |
| **跨境行程** | 检查入境证件、在哪过海关、预清关、转机时是否要重新托运、每天一班的稀缺航段、两地时差 |
| **替别人订票** | 给旅客一份大白话精简版，给订票人一份可核对的结果 |

它**不搜票、不预测价格、不代订**，而是建立在这些工具之上，把搜索结果变成决策。

- 三种模式：快速（简单境内行程）/ 标准（上述任一情况）/ 详细（需求文档、完整结果、第二个 agent 审核）
- 五个偏好档位：省钱 / 均衡 / 舒适 / 计划不确定 / 家庭，每档写明"多付多少钱换什么值得"
- 推荐前 3 名，每条用金额写取舍；出票后核对确认页
- 执行边界：只查询，不下单、不付款、不登录

安装：把 `skills/flight-trip-planning-zh` 复制到 `~/.claude/skills/`（中文版），或复制 `skills/flight-trip-planning`（英文版），二选一。
