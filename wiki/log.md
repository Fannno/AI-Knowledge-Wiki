# wiki/log.md
# 操作紀錄

本檔案為追加式操作紀錄。
只能追加新記錄，不可修改或刪除歷史記錄。
每次 Ingest、Query、Lint 完成後均須追加一筆記錄。

格式：

```
## [YYYY-MM-DD HH:MM] [動作] | [標題]

- 動作：ingest | query | lint | error | fix
- 來源：（Ingest 時填寫）
- 閱讀狀態：full-read | partial-read | summary-only（Ingest 時填寫）
- 新建頁面：（Ingest 時列出）
- 更新頁面：（Ingest 時列出）
- 矛盾標記：有 | 無
- 備註：

---
```

---

## [2026-11-23 19:58] 初始化 | 專案骨架建立

- 動作：初始化
- 建立檔案：
  - AGENTS.md
  - raw/_registry.md
  - wiki/index.md
  - wiki/log.md
  - wiki/exam-bank/_template.md
  - output/build-log.md
  - README.md
- 建立資料夾：
  - raw/papers、raw/textbooks、raw/exams/ccu、raw/exams/nsysu、raw/exams/yuntech
  - raw/articles、raw/official-docs
  - wiki/sources、wiki/concepts/（含各子目錄）、wiki/comparisons
  - wiki/synthesis、wiki/exam-bank/authentic/（含各子目錄）、wiki/exam-bank/practice
  - output/slides
- 備註：Phase 1 骨架建立完成，尚未匯入任何來源資料

---

## [2026-11-24 14:38] ingest | Attention Is All You Need（Vaswani et al. 2017）

- 動作：ingest
- 來源：raw/papers/vaswani-2017-attention-is-all-you-need.pdf
- 閱讀狀態：full-read（論文全文透過 ar5iv.labs.arxiv.org/html/1706.03762 完整讀取，涵蓋 Section 1-6 及完整參考文獻）
- 新建頁面：
  - wiki/sources/source-vaswani-2017-attention.md
  - wiki/concepts/deep-learning/transformer-architecture.md
  - wiki/concepts/deep-learning/self-attention.md
  - wiki/concepts/deep-learning/multi-head-attention.md
  - wiki/concepts/deep-learning/positional-encoding.md
  - wiki/concepts/ml-fundamentals/k-nearest-neighbors.md（通識，任務二）
  - wiki/concepts/ml-fundamentals/svm.md（通識，任務二）
  - wiki/concepts/ml-fundamentals/decision-tree.md（通識，任務二）
  - wiki/concepts/ml-fundamentals/regression.md（通識，任務二）
  - wiki/concepts/ml-fundamentals/clustering.md（通識，任務二）
- 更新頁面：
  - wiki/index.md（新增來源表、概念頁列表、待建頁面清單，更新統計數字）
  - raw/_registry.md（登錄 source-vaswani-2017-attention）
- 矛盾標記：無
- 備註：
  - 任務一（Vaswani 2017 Ingest）：建立 source 頁與 4 個 deep-learning 概念頁，所有技術細節均引用 [[source-vaswani-2017-attention]]
  - 任務二（傳統機器學習基礎）：建立 5 個 ml-fundamentals 概念頁，知識來源標記「通識」，待未來 Ingest 教科書後補充具體來源引用
  - 偵測到待建頁面：[[encoder-decoder]]，已登錄於 wiki/index.md 待建頁面清單
  - 任務三至五：更新 index.md、_registry.md、log.md 完成

---
