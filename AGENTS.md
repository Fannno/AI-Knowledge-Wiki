# AGENTS.md
# AI Knowledge Wiki - 研究所計算機概論 AI 雙語知識庫
# 核心規範文件

版本：1.0
建立日期：2026-11-23
最後更新：2026-11-23

本文件是所有 AI Agent 操作的規範依據。每次執行 Ingest、Query、Lint 前，必須先讀取本文件確認規範版本。

---

## 專案概述

### 目標

建立一個以 Karpathy LLM Wiki 架構為核心的研究所計算機概論 AI 雙語知識庫，並以 Wiki 為主要知識來源製作教學型 HTML 簡報。

### 核心理念

本專案不是 RAG（Retrieval-Augmented Generation）系統。核心差異在於：
- 傳統 RAG：每次查詢都重新檢索原始文件並重新整理答案
- 本專案：由 LLM 持續維護一個可累積、可連結、可更新的 Markdown Wiki，查詢時直接閱讀已整理的 Wiki 頁面

### 語言規範

- 所有 Wiki 頁面、說明文字、介面元素：一律使用繁體中文（台灣用語）
- 英文僅用於：專業術語、英文定義、原始考題、來源標題
- 禁止使用簡體中文
- 禁止在任何文件中使用 Emoji（包含 Markdown、Wiki、HTML 介面元素、按鈕、標題）

---

## 資料夾分工

### raw/ - 原始資料層（不可修改）

- 存放所有原始資料：學術論文、教科書章節、考古題、官方文件
- AI Agent 只能讀取，不可修改或覆寫任何 raw/ 內的檔案
- 子目錄：
  - `raw/papers/`：學術論文
  - `raw/textbooks/`：教科書章節、講義
  - `raw/exams/ccu/`：中正大學考古題
  - `raw/exams/nsysu/`：中山大學考古題
  - `raw/exams/yuntech/`：雲林科技大學考古題
  - `raw/articles/`：技術文章
  - `raw/official-docs/`：官方文件
  - `raw/_registry.md`：已匯入來源登錄表（防重複匯入）

### wiki/ - 知識層（由 LLM 完全維護）

- 存放所有由 LLM 建立與維護的 Markdown 知識頁
- 子目錄：
  - `wiki/sources/`：來源摘要頁（一份原始資料對應一頁）
  - `wiki/concepts/`：概念頁（依主題分類）
  - `wiki/comparisons/`：比較頁
  - `wiki/synthesis/`：綜合整理頁
  - `wiki/exam-bank/authentic/`：真實考古題（依學校分類）
  - `wiki/exam-bank/practice/`：自編練習題
- 特殊檔案：
  - `wiki/index.md`：主目錄，每次操作必讀
  - `wiki/log.md`：追加式操作紀錄，不可修改歷史記錄

### output/ - 輸出層

- 存放由 Wiki 生成的輸出檔案
- `output/slides/`：HTML 簡報
- `output/build-log.md`：簡報建置紀錄

---

## 頁面類型規範

### 1. 概念頁（type: concept）

存放位置：`wiki/concepts/[分類]/[slug].md`

YAML Frontmatter 格式：

```yaml
---
title: "英文概念名稱"
title-zh: "中文概念名稱"
type: concept
slug: english-slug-with-hyphens
created: YYYY-MM-DD
updated: YYYY-MM-DD
status: seed | developing | mature | evergreen
tags:
  - 分類標籤
sources:
  - "[[source-slug]]"
related:
  - "[[related-concept-slug]]"
confidence: high | medium | low
contested: false
lang: zh-en
exam-appearance:
  ccu: []
  nsysu: []
  yuntech: []
---
```

狀態定義：
- `seed`：剛建立，內容尚不完整
- `developing`：有基本內容，仍在擴充
- `mature`：內容完整，有多個來源支持
- `evergreen`：已達高品質，仍持續更新

概念頁固定段落結構（依序）：
1. 基本資訊（英文名稱、中文名稱、領域、考古題出現紀錄）
2. 中文概念說明（2-4 段，重要術語第一次出現時附英文）
3. 英文技術定義（正確自然的學術英文，2-3 段）
4. 重要詞彙（表格：英文 | 中文 | 簡要說明）
5. 核心原理（公式、步驟、流程）
6. 實例說明
7. 易混淆概念（與相似概念的比較）
8. 考試出題方向（僅依考古題中實際出現的題型填寫，無依據不填）
9. 相關來源（Wiki Links 列表）

### 2. 來源摘要頁（type: source）

存放位置：`wiki/sources/[slug].md`

YAML Frontmatter 格式：

```yaml
---
title: "原始資料英文標題"
title-zh: "原始資料中文標題"
type: source
slug: source-[作者]-[年份]-[關鍵字]
created: YYYY-MM-DD
authors:
  - 作者姓名
source-url: "原始連結"
source-type: paper | textbook | article | exam | official-doc
raw-path: "raw/[子目錄]/[檔名]"
date-published: YYYY-MM-DD
date-ingested: YYYY-MM-DD
ingest-status: full-read | partial-read | summary-only
ingest-note: ""
concepts-updated:
  - "[[concept-slug]]"
---
```

ingest-status 規則：
- `full-read`：已閱讀全部原始內容
- `partial-read`：只閱讀部分（須在 ingest-note 說明原因）
- `summary-only`：只有摘要（須在 ingest-note 說明原因，僅在原始資料確實無法取得時使用）

### 3. 比較頁（type: comparison）

存放位置：`wiki/comparisons/[slug].md`

YAML Frontmatter 格式：

```yaml
---
title: "A vs B"
title-zh: "A 與 B 的比較"
type: comparison
slug: a-vs-b
subjects:
  - "[[concept-a]]"
  - "[[concept-b]]"
sources:
  - "[[source-slug]]"
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```

### 4. 考題頁 - 真實考古題（type: exam-question, source-type: authentic）

存放位置：`wiki/exam-bank/authentic/[學校代碼]/[題目代碼].md`

命名規則：`[學校代碼大寫]-[年份]-[科目縮寫]-Q[題號].md`
範例：`CCU-2023-CS-Q05.md`

YAML Frontmatter 格式：

```yaml
---
title: "CCU-2023-CS-Q05"
type: exam-question
source-type: authentic
school: 國立中正大學
school-code: ccu | nsysu | yuntech
year: 2023
subject: 計算機概論 | 資料結構
raw-path: "raw/exams/ccu/ccu-2023-cs.pdf"
has-official-answer: true | false
answer-source: official | ai-derived
answer-note: ""
topic:
  - "[[concept-slug]]"
verified: false
---
```

若 answer-source 為 ai-derived，頁面最頂部必須加入：

```markdown
> [!WARNING] AI 推導答案
> 本題目無官方解答。以下解析由 AI 根據下列教材推導：
> - [[source-slug-1]]
> - [[source-slug-2]]
> 本解析未經官方驗證，使用時請自行核對教材內容。
```

### 5. 考題頁 - 自編練習題（type: exam-question, source-type: practice）

存放位置：`wiki/exam-bank/practice/[題目代碼].md`

命名規則：`PRACTICE-[主題]-[編號三位數].md`

YAML Frontmatter 格式：

```yaml
---
title: "PRACTICE-transformer-001"
type: exam-question
source-type: practice
warning: "本題為自編練習題，非真實考古題，請勿作為考試出題依據"
based-on-concepts:
  - "[[concept-slug]]"
based-on-sources:
  - "[[source-slug]]"
created: YYYY-MM-DD
---
```

頁面最頂部必須加入：

```markdown
> [!NOTE] 自編練習題
> 本題為自編練習題，非真實考古題。題目依據以下 Wiki 頁面設計：
> - [[concept-slug]]
> 請勿作為考試出題依據。
```

---

## Wikilinks 規則

1. 所有專有名詞初次出現時用 `[[slug]]` 或 `[[slug|顯示名稱]]` 標記
2. 同一頁面內，相同連結只在第一次出現時標記，不重複
3. 連結使用英文 slug（全小寫、以連字號分隔）
4. 中英對照格式：`[[transformer-architecture|Transformer 架構]]`
5. 連結指向不存在的頁面時，在 log.md 記錄為「待建頁面」
6. YAML frontmatter 內的 wikilinks 需加引號：`"[[slug]]"`

Slug 命名規範：
- 全小寫英文
- 空格以連字號（-）取代
- 不使用底線（_）
- 不超過五個單詞
- 範例：`transformer-architecture`、`k-nearest-neighbors`、`deep-q-network`

---

## 矛盾處理規則

當新來源與現有 Wiki 內容有矛盾時：

1. 保留原有內容，不可覆蓋
2. 在矛盾位置新增矛盾標記區塊：

```markdown
> [!WARNING] 矛盾標記（待確認）
> 本頁描述（來源：[[source-A]]）：[舊說法]
> 新來源（[[source-B]]）描述：[新說法]
>
> AI 分析：[分析兩說法的差異，可能的原因，以及建議]
> 建議：[建議採用哪種說法或如何整合，並說明理由]
> 狀態：待確認
> 開啟日期：YYYY-MM-DD
```

3. AI 只能提出建議，不能擅自認定矛盾已解決
4. 矛盾解決後，更新狀態並記錄於 log.md

---

## Ingest 操作協議

觸發條件：使用者在 raw/ 目錄下新增了一份原始資料

執行步驟（依序完成，不可跳過）：

Step 1  讀取本 AGENTS.md 文件（確認規範版本）
Step 2  讀取 raw/_registry.md
          若該資料已登錄 -> 停止，回報重複，詢問是否要重新處理
          若未登錄 -> 繼續執行
Step 3  讀取 wiki/log.md 最後 20 筆記錄（掌握近期操作脈絡）
Step 4  讀取 wiki/index.md（掌握現有知識地圖）
Step 5  閱讀目標 raw/ 文件
          必須閱讀原始內容，不可只依賴摘要或摘要
          記錄閱讀範圍：
            full-read：已讀全部
            partial-read：只讀部分（須說明頁數範圍及原因）
          若原始資料確實無法讀取，記錄原因後停止 Ingest，尋找替代來源
Step 6  建立 wiki/sources/[slug].md（來源摘要頁）
Step 7  對每個相關概念判斷：
          概念頁已存在 -> 增量更新
            在相關 section 追加新資訊，注明來源
            若有矛盾 -> 標示矛盾（見矛盾處理規則），不覆蓋
          概念頁不存在 -> 建立新頁面
Step 8  更新或建立比較頁、綜合頁（若有相關）
Step 9  更新 wiki/index.md（新增或更新對應條目）
Step 10 更新 raw/_registry.md（登錄此來源）
Step 11 追加 wiki/log.md 記錄（格式見執行紀錄規範）

強制規則：
- 一份原始來源可影響多個 Wiki 頁面（非一對一）
- 不可覆蓋既有頁面，只能增量更新
- 所有新增內容需附 [[source-slug]] 引用標記
- 不可只看論文摘要就宣稱完成 Ingest
- Ingest 完成後須回報：影響了哪些頁面、是否發現矛盾、是否需要建立新頁面

---

## Query 操作協議

觸發條件：使用者提問

執行步驟（依序完成）：

Step 1  讀取 wiki/index.md（確認哪些頁面與問題相關）
Step 2  讀取相關概念頁（完整內容，非摘要）
Step 3  讀取相關比較頁、綜合頁（若有）
Step 4  必要時追溯來源摘要頁（如需詳細出處）
Step 5  組織回答
          中文為主，重要術語附英文
          每個論點標示來源：（來源：[[source-slug]]）
          若有矛盾觀點，明確呈現不同立場，不隱藏
Step 6  若發現知識缺口 -> 明確指出，提出查證需求
Step 7  若答案有長期保存價值 -> 詢問使用者是否整理為新 Wiki 頁面

規則：
- 所有回答必須有 Wiki 內容支持
- 不可添加 Wiki 中沒有的知識
- 若 Wiki 中確實沒有相關知識，誠實說明並建議補充來源

---

## Lint 操作協議

觸發條件：使用者執行定期維護，或 Ingest 後自動建議

Phase 1  結構檢查
  - 孤立頁面（無任何 incoming wikilinks）
  - 失效連結（[[]] 指向不存在的頁面）
  - 缺少 YAML frontmatter 的頁面
  - YAML 格式錯誤（欄位缺失、型別錯誤）
  - 命名不符規範的檔案

Phase 2  知識品質檢查
  - 矛盾標記（已標示但未解決，列出清單）
  - 過時內容（有更新來源但頁面未更新）
  - 缺少概念頁的重要術語（wikilinks 指向不存在）
  - 中英文定義不一致（同一概念在不同頁面有不同譯名）
  - status 為 seed 超過 30 天但內容仍不完整

Phase 3  考題合規檢查
  - authentic/ 目錄下缺少學校、年度資訊
  - authentic/ 目錄下有 AI 推導答案但未正確標記
  - practice/ 目錄下有可能被誤認為真實考古題的內容

Phase 4  產出 Lint 報告
  - 路徑：wiki/lint-report-YYYY-MM-DD.md
  - 格式：依問題類型分類，每項說明頁面路徑及問題描述
  - 僅產出報告，不自動修改

Phase 5  等待使用者確認
  - 使用者確認後，逐項修正
  - 每項修正記錄於 log.md

---

## 執行紀錄規範

log.md 為追加式記錄，只能追加，不可修改歷史記錄。

每筆記錄格式：

```
## [YYYY-MM-DD HH:MM] [動作] | [標題]

- 動作：ingest | query | lint | error | fix
- 來源：（Ingest 時填寫 raw/ 路徑）
- 閱讀狀態：full-read | partial-read | summary-only（Ingest 時填寫）
- 新建頁面：（Ingest 時列出）
- 更新頁面：（Ingest 時列出）
- 矛盾標記：有 | 無（Ingest 時填寫）
- 備註：（任何需要記錄的資訊，包含錯誤、非預期情況、決策原因）

---
```

特殊情況記錄：
- 錯誤：動作填 error，備註說明錯誤原因及處理方式
- 修正：動作填 fix，說明修正了哪個問題及原因
- 重複來源：記錄已偵測到重複，不繼續 Ingest

規則：
- 不可捏造記錄
- 不可修改或刪除歷史記錄
- 錯誤和失敗的操作也必須記錄

---

## 引用規則

1. 所有知識主張必須能追溯到 raw/ 中的原始資料
2. 引用格式：在論點後標示 `（來源：[[source-slug]]）`
3. 若引用的是某原始資料的特定章節，格式為：`（來源：[[source-slug]]，Chapter X）`
4. 不可添加沒有原始來源支持的知識
5. 若某知識是廣泛認知的基本事實，可標記 `（通識）` 但仍建議尋找來源

---

## 考試出題方向填寫規則

概念頁中的「考試出題方向」欄位：
- 只能依據 wiki/exam-bank/authentic/ 中已收錄的真實考古題填寫
- 若該概念尚無考古題紀錄，此欄位留空或標記「尚無考古題紀錄」
- 不可依據 AI 猜測填寫
- 不可標示無依據的「必考」或給予星級評分

---

## 中英雙語規範

概念頁中的英文定義規則：
- 必須是正確、自然的學術英文
- 應參考來源文件中的英文表述
- 不可直接翻譯中文解釋
- 不可使用不自然的中式英文

重要詞彙表規則：
- 只收錄在原始來源中出現的術語
- 中文翻譯以台灣學術慣用譯名為準
- 若有多種常見譯名，全部列出並說明使用脈絡

---

## 禁止事項

1. 修改或覆寫 raw/ 目錄下的任何檔案
2. 修改 wiki/log.md 的歷史記錄
3. 在頁面中使用 Emoji（包含標題、按鈕、介面元素）
4. 在 authentic/ 目錄下建立沒有明確學校、年度來源的考題
5. 只看摘要就宣稱完成 Ingest
6. 添加沒有原始來源支持的知識
7. 在沒有考古題依據的情況下標示「必考」或給予星級
8. 使用簡體中文
9. AI 擅自認定矛盾已解決（必須等使用者確認）
10. 捏造任何執行紀錄
