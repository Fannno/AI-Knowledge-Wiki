# raw/_registry.md
# 已匯入來源登錄表

本檔案記錄所有已匯入（Ingest）的原始資料。
Ingest 前必須檢查此表，避免重複處理。
本檔案由 AI Agent 維護，格式不可修改。

---

## 登錄格式說明

每筆登錄包含：
- **slug**：Wiki 來源摘要頁的 slug
- **標題**：原始資料標題
- **類型**：paper | textbook | article | exam | official-doc
- **原始路徑**：在 raw/ 中的相對路徑
- **匯入日期**：YYYY-MM-DD
- **Wiki 來源頁**：對應的 wiki/sources/[slug].md 路徑

---

## 已登錄來源

### 1. source-vaswani-2017-attention

- **slug**：source-vaswani-2017-attention
- **標題**：Attention Is All You Need
- **類型**：paper
- **原始路徑**：raw/papers/vaswani-2017-attention-is-all-you-need.pdf
- **匯入日期**：2026-11-24
- **Wiki 來源頁**：wiki/sources/source-vaswani-2017-attention.md
- **閱讀狀態**：full-read
- **影響頁面**：transformer-architecture, self-attention, multi-head-attention, positional-encoding

---

最後更新：2026-11-24
