# Stage 設計筆記

> 給 maintainer 的內部文件，不是讀者面向的內容。
>
> 為什麼是 7 個 stage、每個 stage 結構為什麼這樣切、Hello-X 為什麼必跑、self-check 怎麼設計——這些設計決定的記錄。

---

## 為什麼是 7 個 stage（不是 5 個或 10 個）

### 太少（5 stage）的問題
要把 8 個概念塞 5 個 stage：API 基礎 / prompt / tool use / framework / Claude Code 生態 / memory / RAG / multi-agent。塞下去結果是有的 stage 太擠（譬如 framework + Claude Code 擠一起，3-4 週的內容硬塞 1 stage），讀者跳不過去。

### 太多（10+ stage）的問題
- 時程拉到 6+ 個月，多數人放棄
- stage 間的 dependency 複雜化——讀者看不懂為什麼要先學 X 再學 Y
- maintainer review cost 暴漲

### 7 是「每階段獨立可學完、互相銜接、不重複」的折衷
+ 1 個 Stage 0（prerequisite gateway，可跳）= 8 個檔案，但只有 7 個是真正的 stage。

**判準**：每個 stage 應該對應 1 個**核心問題**（下一節）。若一個 stage 裡塞 2 個核心問題，就該拆；若 2 個 stage 在問同一個問題，就該合。

---

## 每個 stage 的「核心問題」

stage 的價值 = 讀者學完後**能回答這個問題**。

| Stage | 核心問題 | 回答方式 |
|---|---|---|
| **0** 基礎準備 | 「我的開發環境準備好了嗎？」 | 4 個 Hello-X self-test |
| **1** LLM 入門 | 「LLM 是什麼、token 怎麼算、不同 LLM 的差別？」 | 從 API call 到本地 LLM，含 from-scratch 訓練 |
| **2** Prompt 設計 | 「怎麼讓 LLM 照我的意思做事？」 | system / few-shot / CoT / DSPy |
| **3** ⭐ Tool Use & Hello Agent | 「怎麼讓 LLM 呼叫外部工具？」 | function calling + ReAct + 5 個 Hello-X 必跑 |
| **4** Agent 框架 | 「哪個 framework 該學、為什麼？」 | LangGraph / AutoGen / CrewAI / Smolagents 對比 |
| **5** ⭐⭐ Claude Code 生態 | 「Claude Code 生態系怎麼吃？」 | MCP / Skills / Plugins / Marketplace 4 個 sub-stage |
| **6** Memory · RAG | 「怎麼讓 agent 記得事情？怎麼讓它能查自家文件？」 | embedding / vector DB / RAG / contextual retrieval |
| **7** 進階 Multi-Agent | 「multi-agent 跟 production 怎麼一起？」 | orchestration / eval / observability / SDK 進階 |

每個 stage 結尾的 self-check 就是 **「能不能回答這個核心問題」** 的 measurable 版本。

---

## Stage 結構（dominant pattern，非絕對 invariant）

多數 stage 用以下結構（Stage 0 / 5 / 6 / 7 有 documented 例外，見後）：

```
1. ⏱ 時間估算
2. ## 📌 學習目標
3. ## 🚪 進入條件        （Stage 1-4 有；Stage 6 / 7 省略，因為 Stage 5 已給足前置）
4. ## 📚 必修閱讀
5. ## 🛠 Hello-X Projects
6. ## 🎯 精選 Projects
7. ## ✅ 進 Stage N+1 前的自我檢查
```

**已知例外**：
- **Stage 0**：prerequisite gateway，沒有完整結構（見 §「Stage 0 為什麼可以 skip」）
- **Stage 5**：分 4 個 sub-stage（5.1-5.4），每個 sub-stage 各有自己的 學習目標 / 必修閱讀 / Hello-X / 精選 Projects
- **Stage 6 / 7**：直接跳過 進入條件 section（前面 stage 已隱含 prerequisite）

每個 section 的功能：

### 學習目標
- 必須**可量化**（不是「了解 X」，是「能用 PyTorch 寫一個 ReAct agent」）
- 4-6 個 bullet（多會 dilute、少會缺失）
- 每個 bullet 對應 1 個 self-check question

### 進入條件
- Stage 跳級者的 self-test：「你已經會這些就能直接從這個 stage 開始」
- Stage 0 沒這個 section（Stage 0 本身就是 entry condition）

### 必修閱讀
- 3-5 個 link（多會讀不完、少會 under-cover）
- 該 stage 開始前 / 中 / 後都行，但「不讀就跟不上」是判準
- 偏好官方 doc / 經典論文，不放長部落格

### Hello-X Projects
- 通常 3-5 個（Stage 1 / 3 因為要 cover 多個概念，會到 5-6 個）
- 每個都有具體成功標準（跑出某個輸出、看到某個錯誤等）
- **必須是「不動手就學不會」的東西**——光讀光看不算
- Hello-X 跟 self-check 是 **conceptual coverage 對應**（不是 1:1 編號對應）——跑過 Hello-X 後，self-check 整體應該能過；單一條 self-check 可能對應到多個 Hello-X
- Stage 5 因為 sub-section（5.1-5.4）結構，Hello-X 分散在各 sub-section

### 精選 Projects
- 跑完 Hello-X 後的延伸學習
- 每個 entry 照 [style guide](../resources/style-guide.md) §1 schema
- 數量：通常 7-15 個（Stage 5 例外，20 個分散在 4 個 sub-section）

### 自我檢查
- **measurable**——能 verify 的不是「了解 X」
- 通常 4-6 個 checkbox（依 stage 範圍調整；不固定數）
- binary judgment（會 / 不會），全部能勾才算通關

---

## Hello-X 設計原則

### 為什麼必跑、不能只是讀

Stage 3 的 5 個 Hello-X 是整個 catalog 最重要的設計決定。理由：

agent 寫過 vs 沒寫過 ≠ 多讀一篇 paper vs 少讀一篇。寫過的人後面學 LangGraph 知道 framework 在抽象什麼；沒寫過直接學 framework 會被 magic 困住。

所以 Stage 3 結尾的「進 Stage 4 前的自我檢查」第一條就是：**「用不到 100 行 Python、不靠任何 framework，把 ReAct 迴圈寫出來」**——這是 binary 的 gate，跳不過就回去再跑一次。

### 具體成功標準（不是「了解 X」）
反例：「了解 ReAct pattern」→ 不可量化
正例：「給 5 個工具的 agent 完成『找台北人口除以紐約人口』的多步推理」→ 可量化

### 數量
- 3-5 個是 sweet spot
- 多會 dilute（讀者覺得負擔大、跳過）
- 少會 under-cover（譬如 Stage 1 只有 3 個 Hello-X，但要涵蓋 API call / token / pricing / cross-provider / error handling / local LLM——所以該 stage 後來補到 6 個）
- Stage 5 因為 4 個 sub-section，每個 sub-section 再有 2-3 個 Hello-X

---

## Entry 選入 / 排除原則（補強 [style-guide](../resources/style-guide.md)）

style-guide 講格式、用詞、license。這份補跨 stage 的考量：

### 跟 stage 核心問題的相關度
entry 的「教什麼」應該是該 stage 核心問題的一個答案的具體實作。
- Stage 1 核心問題：LLM 是什麼。→ Anthropic Cookbook（教怎麼用）✓、rasbt/LLMs-from-scratch（教內部）✓
- Stage 1 核心問題不該 cover：tool use（那是 Stage 3）、memory（那是 Stage 6）

### Entry 不重複
- 同一 repo 在不同 stage 出現要有不同 framing（譬如 `obra/superpowers` 在 Stage 5 是 SKILL.md collection，在 for-developer 是 TDD skill）
- framing 重複的 entry 要刪一個

### 廣度 vs 深度
- 同類型工具列 2-3 個就夠（譬如 vector DB 列 Chroma + Qdrant + pgvector + Weaviate，但不需要列 5 個更小眾的）
- 同 audience 工具列 3-5 個（譬如 coding agent 列 Cursor + Aider + Cline + Continue + Goose）

---

## Self-check 怎麼設計

### Measurable 是核心
反例：
- 「了解 LangGraph」 ❌
- 「能解釋 LangGraph 為什麼用 graph」 ❌（subjective）
- 「能寫一個 LangGraph workflow 含 conditional edge + checkpoint」 ✓（binary）

### 跟 Hello-X 對應（conceptual coverage，不是 1:1 編號）
跑完該 stage 全部 Hello-X 之後，整份 self-check 應該能過。但**不要求 Hello-N 對應 self-check N 號這種編號 mapping**——一條 self-check 可能 cover 多個 Hello-X，反之亦然。範例：Stage 3 的 self-check 第 1 條「定義一個 tool schema」對應 Hello-1，但 self-check 第 2 條「不靠 framework 寫 ReAct」其實是 Hello-3 的能力。

### 例外：abstract concept check
有些核心問題很難 measurable（譬如「為什麼 agent 需要退出條件？」）——這時用「**能不能口頭解釋給朋友聽**」做替代。但這種 check 不該超過 self-check 總數的 30%。

---

## Stages 之間的銜接

### 為什麼 4 → 5 → 6 → 7 是這順序
- 4 framework 後 → 5 Claude Code 生態（為什麼 Claude Code 是核心？因為它把 5.1-5.4 的概念集成在一個工具裡）
- 5 → 6 memory（agent 有 framework 之後才會問「怎麼記住」）
- 6 → 7 multi-agent（單 agent + memory 都會了，才考慮多 agent）

不是純線性——Stage 4 有「memory peek」指 Stage 6（「LangGraph 有 checkpoint，那是 memory 的東西，到 Stage 6 會講」），讓讀者知道延伸但不卡關。

### 跨 stage walkthrough 怎麼用
[`walkthroughs/build-first-agent-in-7-steps.md`](../walkthroughs/build-first-agent-in-7-steps.md) 用同一個 Paper Summary Bot 串完 Stage 1 到 7。這份是 stage 之間銜接的 ground truth：每個 stage 結束時 agent 應該長什麼樣，下一 stage 怎麼增加新層。

如果某個 stage 改了結構（譬如 Stage 6 換了 vector DB），walkthrough 也要同步改——是 maintain cost，但確保 stage 之間真的能串得起來。

---

## ⭐⭐ 標記為什麼放 Stage 5

兩個原因：

### 1. 這 stage 是 Claude Code 使用者的核心
Repo 名字是 `awesome-agentic-ai-zh`，受眾偏 Claude Code 使用者。Stage 5 是這個生態的完整教學——不會這 stage 就不算懂 Claude Code。

### 2. 內容量比其他 stage 偏大
- 多數 stage：1-2 週、7-15 個 entry
- Stage 5：3-4 週、4 個 sub-section、20 個 entry
- Stage 7 也大（22 個 entry），但結構是 flat 的——Stage 5 的 sub-section 結構是它特別需要 ⭐⭐ 提醒的原因

所以額外加 ⭐⭐ 提醒讀者「這個 stage 比較大、結構比較複雜，別跳」。Stage 3 加 ⭐ 是因為「Hello Agent 是整個 catalog 最重要的轉折點」（不寫 ReAct 寫不出 agent）。

---

## Stage 0 為什麼可以 skip

Stage 0 不是 stage——它是 prerequisite gateway。
- Python / git / CLI / JSON 已經會的人 → 直接 Stage 1
- 不會的人 → Stage 0 不是要從零教 Python，是給「我該不該學這 4 樣才能開始」的 self-test，順便給快速 reference 連結

所以 Stage 0 沒有完整的學習目標 / Hello-X / self-check structure——只有「skip 條件」+ 「資源連結」。它存在是為了**讓真的初學者不會在後面 stage 卡住**，但不假設讀者要從這裡完整走完。

---

## 不在這份的內容

- **個別 stage 的 entry 詳細**：見 `stages/0X-...md` 本身
- **branch 設計理由**：見 [`../branches/DESIGN.md`](../branches/DESIGN.md)
- **entry schema / 用詞規範**：見 [`../resources/style-guide.md`](../resources/style-guide.md)
- **跨 stage 範例**：見 [`../walkthroughs/build-first-agent-in-7-steps.md`](../walkthroughs/build-first-agent-in-7-steps.md)
