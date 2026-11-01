[English](README.md) | 简体中文 | [繁體中文](README.zh-TW.md)

> **说明：** 本仓库是一个 Claude skill。关于 Agent Skills 标准，请见 [agentskills.io](https://agentskills.io)。

# flight-trip-planning

当最便宜的航班不是正确答案时，这个 skill 帮 Claude 决定**该订哪张机票**。它建立在机票搜索工具之上：把搜索结果变成别人能核对的决定，在航司官网核实价格，并把灵活性折算成钱。它从不订票、付款或登录。

仓库里同一个 skill 有两种语言：`skills/flight-trip-planning`（英文指令）和 `skills/flight-trip-planning-zh`（中文指令）。安装其中一个即可。

关于 skill 的更多信息：
- [What are skills?](https://support.claude.com/en/articles/12512176-what-are-skills)
- [Using skills in Claude](https://support.claude.com/en/articles/12512180-using-skills-in-claude)
- [How to create custom skills](https://support.claude.com/en/articles/12512198-creating-custom-skills)

# 这个 skill 做什么

搜索引擎回答"哪班最便宜"。这个 skill 回答"我们到底该订哪一班"，专门处理搜索引擎不管的四种情况：

| 情况 | skill 做什么 |
|---|---|
| **计划可能变**（签证或移民审批未定、工作学业日期、家庭安排） | 用盈亏平衡判断给灵活性定价：可退 vs 可改 vs 基础票 |
| **退款必须退现金**（要搬到别的国家、很少再坐这家航司） | 区分"退回原支付方式"和"航司 credit"，并判断 credit 是否真用得上 |
| **跨境行程** | 检查入境证件、在哪过海关、美国预清关、转机时是否要重新托运、每天一班的稀缺航段、时区 |
| **替别人订票** | 给旅客一份大白话精简版，给订票人一份可核对的结果 |

每条推荐都用金额写出取舍（"多付 $27 换白天到达"），可以核对，也可以反驳。

## 什么时候触发

- 在航班或票种（基础、标准、可退）之间做选择，包括"帮我看看 X 到 Y 的机票"这种简单请求
- 判断转机或跨境行程是否可行
- 审核别人或其他 agent 找到的机票方案

## 什么时候不触发

- 酒店或整体行程规划
- 已订机票或被航司取消航班后的退款、credit、赔偿申请
- 实时搜票、价格追踪、该买还是该等的预测（请用 Google Flights、Kayak、Skyscanner 或 Hopper；skill 把它们的结果当作线索）
- 里程积分最优化，以及隐藏城市票、"Hacker Fares"这类出票技巧

# 模式

| 模式 | 适用 | 提问数 | 输出 |
|---|---|---|---|
| **快速** | 境内、日期固定、无特殊约束 | 0–2，只问消息里没说的 | 前 3 名精简卡和一句推荐，篇幅约 250 个英文单词 |
| **标准** | 计划不确定、跨境、替别人订、超过 2 位旅客 | 6–10 | 对比表、取舍说明、旅客精简版 |
| **详细** | 影响大、日期和机场组合多、或由第二个 agent 审核结果 | 完整问询 | 带版本号的需求文档、完整结果文件、审核清单、出票记录 |

# 偏好档位

不用严格的优先级顺序，每个档位是一组兑换率：某项改善值得多花多少钱。共有五个档位：**省钱**、**均衡**（默认）、**舒适**、**计划不确定**、**家庭 / 团体**。例如均衡档认为直飞每人最多值 $100，白天到达最多值 $50。每个兑换率都可以改。

票种的灵活性用盈亏平衡判断来决定，不用兑换率。可退票 $657、可改票 $501 且 credit 会用不掉时，取消概率超过 156 / 657 ≈ 23.7%，可退票更划算。

# 示例提示

- "给我太太订温哥华到北京。Standard $501，取消给 credit；Latitude $657 可退。签证大概有 25% 的可能来不及，而且我们要搬去德国。选哪个？"
- "萨斯卡通经多伦多到芝加哥，同一张 Air Canada 机票，在多伦多转机 75 分钟够吗？"
- "下周五上午 SFO 飞 LAX，只带登机箱，找个合适的就行"
- "我们 2 个大人 2 个孩子，4 件托运行李，多伦多飞坎昆。这三个方案哪个真的最便宜？"
- "另一个 agent 做了这份机票方案，订之前帮我检查一下。"

# 在 Claude Code、Claude.ai 和 API 中使用

## Claude Code

在 Claude Code 里把本仓库注册为插件市场：

```
/plugin marketplace add ArthurDJ/flight-trip-planning
```

然后安装一个语言版本：

```
/plugin install flight-trip-planning@flight-trip-planning
/plugin install flight-trip-planning-zh@flight-trip-planning
```

也可以把 skill 文件夹复制到个人 skills 目录：

```bash
git clone https://github.com/ArthurDJ/flight-trip-planning.git
mkdir -p ~/.claude/skills
cp -r flight-trip-planning/skills/flight-trip-planning ~/.claude/skills/      # 英文版
cp -r flight-trip-planning/skills/flight-trip-planning-zh ~/.claude/skills/   # 或中文版
```

## Claude.ai

把一个 skill 文件夹（`flight-trip-planning` 或 `flight-trip-planning-zh`）压缩成 zip，作为自定义 skill 上传。见 [Using skills in Claude](https://support.claude.com/en/articles/12512180-using-skills-in-claude)。

## Claude API

自定义 skill 可以通过 Claude API 上传和使用，见 [Skills API Quickstart](https://docs.claude.com/en/api/skills-guide#creating-a-skill)。通过 API 使用时，skill 只能用你的应用提供的工具：没有网页浏览工具时，skill 会把每个价格标为未核实。

# 运行要求

| 项目 | 是否必需 | 说明 |
|---|---|---|
| 能加载 skill 的 agent | 是 | Claude Code、Claude.ai 自定义 skill、Claude API，或任何能读 Markdown 指令的 agent |
| 网页浏览 | 强烈建议 | 用来在航司官网核实价格、票种和退改条款。没有时，skill 基于你贴的结果工作，并把每个价格标为未核实 |
| 机票搜索来源 | 建议 | 任何搜索工具或 MCP 服务器，或你贴的结果 |
| 网页搜索 | 建议 | 执行时重新核实有时效的规则（入境规则、乘客权益、时区） |
| 写文件 | 可选 | 详细模式下写需求文档和结果文件 |
| 航司账号或 API 密钥 | 不需要 | agent 从不登录、订票或付款 |

# 局限和安全边界

- **只查询。** agent 从不订票、付款、占座、登录或输入账号密码。遇到登录和验证页面就跳过，订票留给你本人。
- **价格在官网核实之前只是线索。** 没有浏览器时什么都没核实，skill 会明确说明。
- **规则会变。** 行李费、退改政策、入境要求和时区都会变。有时效的事实在 `references/regions.md` 里带"核实日期"和来源，条件允许时 skill 会在执行时重新核实。
- **北美规则最完整**；其他地区只有起步清单。
- 不构成法律、移民或财务建议。

# 从 v1 升级

第 1 版是一个名为 `flight-trip-planning` 的中文 skill。从第 2 版起，这个名字属于**英文版**，中文版改名为 `flight-trip-planning-zh`。

- 如果装过 v1 并想用中文版：先删除 `~/.claude/skills/flight-trip-planning`（旧的 v1 副本），再安装 `flight-trip-planning-zh`。否则过时的 v1 规则会和新 skill 并存，两个都可能被触发。
- 如果想用英文版：用新文件夹替换 `~/.claude/skills/flight-trip-planning`。
- 在 Claude.ai 上：先删除旧的自定义 skill，再上传新的。
- 查看已安装的版本：看 SKILL.md frontmatter 里 `metadata` 下的 `version`。v1 和 v2 都没有这一项。

# 仓库结构

```
.claude-plugin/marketplace.json   Claude Code 插件市场
skills/
  flight-trip-planning/           英文版 skill
    SKILL.md                      各模式流程、范围、规则、输出
    references/
      decision-framework.md       真实总价、档位、盈亏平衡、票价动态
      verification.md             官网核实、中转时间、转机、时区、审核
      regions.md                  带来源的日期事实、北美模块、其他地区
      scenarios.md                单程 vs 往返、多城市、家庭、临时出行、多名旅客
      templates.md                问询、对比卡、旅客精简版、交接、出票清单
    LICENSE.txt
  flight-trip-planning-zh/        中文版 skill，结构相同
evals/                            测试用例和触发测试语句
scripts/check_parity.py           检查中英文文件是否一致
```

# 测试

- `python scripts/check_parity.py` 比较中英文 skill 文件，以及 `README.md`、本文件和 `README.zh-TW.md`：标题结构、每个数字和交叉引用。有任何不一致就报错退出。
- `evals/` 里有 8 个按 `claude plugin eval` 格式编写的测试用例，`evals/trigger-queries.json` 有 20 条用于测试 description 的语句。要对某个语言版本运行这些用例，把 `evals/` 复制到那个 skill 文件夹，在那里运行 `claude plugin eval .`。

# 许可证

MIT。见 [LICENSE](LICENSE)；每个 skill 文件夹里也有一份 `LICENSE.txt`。
