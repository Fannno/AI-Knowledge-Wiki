# wiki/exam-bank/_template.md
# 考題頁面範本

本文件為考題頁面範本，建立新考題頁面時，請複製本範本並填入實際內容。

---

## 真實考古題範本

存放位置：`wiki/exam-bank/authentic/[學校代碼]/[題目代碼].md`
命名規則：`[學校代碼大寫]-[年份]-[科目縮寫]-Q[題號].md`

```markdown
---
title: "CCU-2023-CS-Q05"
type: exam-question
source-type: authentic
school: 國立中正大學
school-code: ccu
year: 2023
subject: 計算機概論
raw-path: "raw/exams/ccu/ccu-2023-cs.pdf"
has-official-answer: false
answer-source: ai-derived
answer-note: "官方未提供解答，本解析由 AI 依 [[source-bishop-prml-ch1]] 推導，非官方答案"
topic:
  - "[[k-nearest-neighbors]]"
verified: false
---

> [!WARNING] AI 推導答案（僅在 answer-source 為 ai-derived 時加入此區塊）
> 本題目無官方解答。以下解析由 AI 根據下列教材推導：
> - [[source-slug-1]]
> 本解析未經官方驗證，使用時請自行核對教材內容。

# 題目代碼：學校-年份-科目-題號

## 原題（英文）

（完整引用原題文字，不可修改）

## 中文翻譯

（自然流暢的繁體中文翻譯）

## 關鍵字

- 英文關鍵字 1
- 英文關鍵字 2

## 考點分析

（分析這題在考什麼概念、難度判斷）

## 參考答案

（詳細解答，附公式、推導過程）

## 解題示範

（逐步解題邏輯）

## 相關 Wiki 頁面

- [[concept-slug-1|概念名稱]]
- [[concept-slug-2|概念名稱]]
```

---

## 自編練習題範本

存放位置：`wiki/exam-bank/practice/[題目代碼].md`
命名規則：`PRACTICE-[主題]-[編號三位數].md`

```markdown
---
title: "PRACTICE-transformer-001"
type: exam-question
source-type: practice
warning: "本題為自編練習題，非真實考古題，請勿作為考試出題依據"
based-on-concepts:
  - "[[transformer-architecture]]"
based-on-sources:
  - "[[source-vaswani-2017-attention]]"
created: YYYY-MM-DD
---

> [!NOTE] 自編練習題
> 本題為自編練習題，非真實考古題。題目依據以下 Wiki 頁面設計：
> - [[transformer-architecture|Transformer 架構]]
> 請勿作為考試出題依據。

# 練習題：主題說明

## 題目（英文）

（題目文字）

## 中文翻譯

## 關鍵字

## 考點分析

## 參考答案

（附來源）

## 解題示範

## 相關 Wiki 頁面
```
