[English](README.md) | [简体中文](README.zh-CN.md) | 繁體中文

> **說明：** 本儲存庫收錄一個 Claude skill。關於 Agent Skills 標準，請參閱 [agentskills.io](https://agentskills.io)。

# flight-trip-planning

當最便宜的航班不是正確答案時，這個 skill 協助 Claude 決定**該訂哪一張機票**。它建立在機票搜尋工具之上：把搜尋結果變成別人能核對的決定，到航空公司官網核實價格，並把彈性換算成金額。它從不訂票、付款或登入。

儲存庫裡同一個 skill 有兩種語言：`skills/flight-trip-planning`（英文指示）和 `skills/flight-trip-planning-zh`（中文指示）。安裝其中一個即可。

關於 skill 的更多資訊：
- [What are skills?](https://support.claude.com/en/articles/12512176-what-are-skills)
- [Using skills in Claude](https://support.claude.com/en/articles/12512180-using-skills-in-claude)
- [How to create custom skills](https://support.claude.com/en/articles/12512198-creating-custom-skills)

# 這個 skill 做什麼

搜尋引擎回答「哪一班最便宜」。這個 skill 回答「我們到底該訂哪一班」，專門處理搜尋引擎不管的四種情況：

| 情況 | skill 做什麼 |
|---|---|
| **計畫可能改變**（簽證或移民審查未定、工作或學業日期、家庭安排） | 用損益兩平判斷為彈性定價：可退票 vs 可改票 vs 基本票 |
| **退款必須退現金**（要搬到其他國家、很少再搭這家航空公司） | 區分「退回原付款方式」和「航空公司抵用金（credit）」，並判斷抵用金是否真的用得到 |
| **跨境行程** | 檢查入境證件、在哪裡通關、美國預先通關（preclearance）、轉機時是否要重新託運、每天只有一班的稀少航段、時區 |
| **替別人訂票** | 給旅客一份白話精簡版，給訂票人一份可核對的結果 |

每項建議都用金額寫出取捨（「多付 $27 換白天抵達」），可以核對，也可以反駁。

## 什麼時候觸發

- 在航班或票種（基本、標準、可退）之間做選擇，包括「幫我看看 X 到 Y 的機票」這類簡單請求
- 判斷轉機或跨境行程是否可行
- 審查別人或其他 agent 找到的機票方案

## 什麼時候不觸發

- 住宿或完整行程規劃
- 已訂機票或航空公司取消航班後的退款、抵用金、賠償申請
- 即時查詢機票、價格追蹤、該買還是該等的預測（請用 Google Flights、Kayak、Skyscanner 或 Hopper；skill 把它們的結果當作線索）
- 里程點數最佳化，以及隱藏城市票、「Hacker Fares」這類開票技巧

# 模式

| 模式 | 適用情況 | 提問數量 | 輸出 |
|---|---|---|---|
| **快速** | 國內航線、日期固定、沒有特殊限制 | 0–2，只問訊息裡沒提到的 | 前 3 名精簡卡和一句建議，篇幅約 250 個英文字 |
| **標準** | 計畫不確定、跨境、替別人訂、超過 2 位旅客 | 6–10 | 比較表、取捨說明、旅客精簡版 |
| **詳細** | 影響重大、日期和機場組合多，或由第二個 agent 審查結果 | 完整提問 | 附版本號的需求文件、完整結果檔案、審查清單、開票紀錄 |

# 偏好設定

不採用嚴格的優先順序，每種偏好設定是一組換算比率：某項改善值得多花多少錢。共有五種：**省錢**、**均衡**（預設）、**舒適**、**計畫不確定**、**家庭 / 團體**。例如均衡設定認為直飛每人最多值 $100，白天抵達最多值 $50。每個比率都可以調整。

票種的彈性用損益兩平判斷來決定，不用換算比率。可退票 $657、可改票 $501 且抵用金會用不掉時，取消機率超過 156 / 657 ≈ 23.7%，可退票就比較划算。

# 提示範例

- 「幫我太太訂溫哥華到北京。Standard $501，取消給抵用金；Latitude $657 可退款。簽證大約有 25% 的機會來不及，而且我們要搬去德國。該選哪個？」
- 「薩斯卡通經多倫多到芝加哥，同一張 Air Canada 機票，在多倫多轉機 75 分鐘夠嗎？」
- 「下週五上午 SFO 飛 LAX，只帶隨身行李，找個合理的就好」
- 「我們 2 個大人 2 個小孩，4 件託運行李，多倫多飛坎昆。這三個方案哪個真的最便宜？」
- 「另一個 agent 做了這份機票方案，訂票前幫我檢查一下。」

# 在 Claude Code、Claude.ai 和 API 中使用

## Claude Code

在 Claude Code 中把本儲存庫註冊為外掛程式市集：

```
/plugin marketplace add ArthurDJ/flight-trip-planning
```

接著安裝其中一個語言版本：

```
/plugin install flight-trip-planning@flight-trip-planning
/plugin install flight-trip-planning-zh@flight-trip-planning
```

也可以把 skill 資料夾複製到個人的 skills 目錄：

```bash
git clone https://github.com/ArthurDJ/flight-trip-planning.git
mkdir -p ~/.claude/skills
cp -r flight-trip-planning/skills/flight-trip-planning ~/.claude/skills/      # 英文版
cp -r flight-trip-planning/skills/flight-trip-planning-zh ~/.claude/skills/   # 或中文版
```

## Claude.ai

把一個 skill 資料夾（`flight-trip-planning` 或 `flight-trip-planning-zh`）壓縮成 zip，以自訂 skill 上傳。請參閱 [Using skills in Claude](https://support.claude.com/en/articles/12512180-using-skills-in-claude)。

## Claude API

自訂 skill 可以透過 Claude API 上傳和使用，請參閱 [Skills API Quickstart](https://docs.claude.com/en/api/skills-guide#creating-a-skill)。透過 API 使用時，skill 只能使用你的應用程式提供的工具：沒有網頁瀏覽工具時，skill 會把每個價格標示為未核實。

# 系統需求

| 項目 | 是否必要 | 說明 |
|---|---|---|
| 能載入 skill 的 agent | 是 | Claude Code、Claude.ai 自訂 skill、Claude API，或任何能讀取 Markdown 指示的 agent |
| 網頁瀏覽 | 強烈建議 | 用來到航空公司官網核實價格、票種和退改規定。沒有的話，skill 會以你貼上的結果為準，並把每個價格標示為未核實 |
| 機票搜尋來源 | 建議 | 任何搜尋工具或 MCP 伺服器，或你貼上的結果 |
| 網頁搜尋 | 建議 | 執行時重新確認有時效性的規定（入境規定、旅客權益、時區） |
| 寫入檔案 | 選用 | 詳細模式下寫入需求文件和結果檔案 |
| 航空公司帳號或 API 金鑰 | 不需要 | agent 從不登入、訂票或付款 |

# 限制與安全界線

- **只查詢。** agent 從不訂票、付款、保留座位、登入或輸入帳號密碼。遇到登入和驗證頁面就跳過，訂票交給你本人。
- **價格在官網核實之前都只是線索。** 沒有瀏覽器時什麼都沒有核實，skill 會明確說明。
- **規定會改變。** 行李費、退改政策、入境規定和時區都會變。有時效性的事實在 `references/regions.md` 裡附有「核實日期」和來源，條件允許時 skill 會在執行時重新確認。
- **北美的規定最完整**；其他地區只有入門檢查清單。
- 不構成法律、移民或財務建議。

# 從 v1 升級

第 1 版是名為 `flight-trip-planning` 的中文 skill。從第 2 版起，這個名稱屬於**英文版**，中文版改名為 `flight-trip-planning-zh`。

- 如果裝過 v1 又想用中文版：先刪除 `~/.claude/skills/flight-trip-planning`（舊的 v1 副本），再安裝 `flight-trip-planning-zh`。否則過時的 v1 規則會和新 skill 同時存在，兩者都可能被觸發。
- 如果想用英文版：用新資料夾取代 `~/.claude/skills/flight-trip-planning`。
- 在 Claude.ai 上：先刪除舊的自訂 skill，再上傳新的。
- 查看已安裝的版本：看 SKILL.md frontmatter 中 `metadata` 底下的 `version`。v1 和 v2 都沒有這個欄位。

# 儲存庫結構

```
.claude-plugin/marketplace.json   Claude Code 外掛程式市集
skills/
  flight-trip-planning/           英文版 skill
    SKILL.md                      各模式流程、範圍、規則、輸出
    references/
      decision-framework.md       實際總價、偏好設定、損益兩平、票價變動
      verification.md             官網核實、轉機時間、轉機評估、時區、審查
      regions.md                  附來源的日期事實、北美模組、其他地區
      scenarios.md                單程 vs 來回、多城市、家庭、臨時出發、多名旅客
      templates.md                提問、比較卡、旅客精簡版、交接、訂票清單
    LICENSE.txt
  flight-trip-planning-zh/        中文版 skill，結構相同
evals/                            測試案例和觸發測試查詢
scripts/check_parity.py           檢查中英文檔案是否一致
scripts/validate_skills.py        檢查 SKILL.md frontmatter、檔案參照、marketplace.json 和 evals
.github/                          CI 工作流程、issue 表單、pull request 範本
```

# 測試

- `python scripts/check_parity.py` 比較中英文 skill 檔案，以及 `README.md`、`README.zh-CN.md` 和本檔案：標題結構、每個數字和交叉參照。只要有任何不一致，就回報錯誤並結束。
- `evals/` 裡有 8 個依 `claude plugin eval` 格式撰寫的測試案例，`evals/trigger-queries.json` 有 20 條用來測試 description 的查詢。要對某個語言版本執行這些案例，把 `evals/` 複製到該 skill 資料夾，並在那裡執行 `claude plugin eval .`。
- `python scripts/validate_skills.py` 依 [Agent Skills 規格](https://agentskills.io/specification) 檢查兩個 SKILL.md（name 與資料夾同名、description 長度、`license`、`metadata` 只含字串），檢查 skill 裡每個 `references/` 路徑和相對連結都存在、`.claude-plugin/marketplace.json` 以存在的路徑列出每個 skill 資料夾、兩個 SKILL.md、`marketplace.json` 和 `CHANGELOG.md` 的版本號一致，以及每個測試案例都有 `prompt.md`、`case.yaml` 和 graders。只要有任何問題，就回報錯誤並結束。

# 參與貢獻

每個 pull request 和每次推送到 `main` 時，GitHub Actions 都會執行這兩個指令碼（`.github/workflows/ci.yml`）。它們只需要 Python 和標準函式庫，推送前請先在本機執行：

```bash
python scripts/check_parity.py
python scripts/validate_skills.py
```

中英文 skill 和三個 README 要在同一個 pull request 裡一起修改。修改規則需要官方來源，並更新「带日期的事实」表（中文版）和「Dated facts」表（英文版）中的核實日期；完整清單請參閱 pull request 範本。規則過時，或 skill 該觸發時沒有觸發、不該觸發時卻觸發了，請使用 issue 表單回報。

# 授權條款

MIT。請參閱 [LICENSE](LICENSE)；每個 skill 資料夾裡也有一份 `LICENSE.txt`。
