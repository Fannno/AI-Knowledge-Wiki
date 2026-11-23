# AI Knowledge Wiki
# 研究所計算機概論 AI 雙語知識庫

## 專案簡介

本專案採用 Karpathy LLM Wiki 架構，建立研究所計算機概論 AI 主題知識庫，並以 Wiki 為主要知識來源製作教學型 HTML 簡報。

目標使用者：準備中正、中山、雲林科技大學研究所計算機概論考試的考生。

## 核心架構

```
AI-Knowledge-Wiki/
├── AGENTS.md          核心規範文件（所有 AI 操作的依據）
├── raw/               原始資料層（不可修改）
├── wiki/              知識層（由 AI 完全維護）
└── output/            輸出層（HTML 簡報）
```

## 三大核心操作

**Ingest**：將原始資料匯入並整理至 Wiki 知識頁。
觸發：在 raw/ 新增原始資料後，請 AI 執行 Ingest 操作。

**Query**：依據 Wiki 內容回答問題，並標示知識來源。
觸發：直接向 AI 提問，AI 會先讀取 Wiki 再回答。

**Lint**：定期檢查 Wiki 的結構完整性與知識品質。
觸發：請 AI 執行 Lint 操作，AI 會產出報告後等待確認。

## 使用說明

### 新增原始資料

1. 將資料放入對應的 raw/ 子目錄：
   - `raw/papers/`：學術論文
   - `raw/textbooks/`：教科書章節
   - `raw/exams/[學校代碼]/`：考古題
   - `raw/articles/`：技術文章
   - `raw/official-docs/`：官方文件

2. 告知 AI：「請對 raw/[路徑] 執行 Ingest 操作」

### 提問查詢

直接向 AI 提問。AI 會先讀取 wiki/index.md 和相關頁面，再依 Wiki 內容回答並標示來源。

### 定期維護

每隔一段時間告知 AI：「請執行 Lint 操作」

AI 會產出報告至 wiki/lint-report-YYYY-MM-DD.md，確認後再逐項修正。

## 重要規則（摘要）

詳細規則請見 AGENTS.md。

- raw/ 目錄下的檔案 **不可修改**
- wiki/log.md **只能追加**，不可修改歷史記錄
- 所有知識必須有來源支持，不可添加無依據的內容
- 考古題分為「真實考古題」（authentic/）和「自編練習題」（practice/），不可混淆
- AI 推導的答案必須標記「AI 推導答案」警告
- 全專案禁止使用 Emoji
- 所有說明文字使用繁體中文（台灣用語）

## 知識主題

**傳統機器學習**：KNN、SVM、Decision Tree、Regression、Clustering

**深度學習**：Neural Network、Perceptron、CNN、RNN、LSTM

**自然語言處理**：NLP、Tokenization、Word Embedding

**電腦視覺**：CNN 應用、Image Classification

**強化學習**：RL、MDP、Q-Learning、DQN

**大型語言模型**：Transformer、LLM、Token、Embedding、RAG、RLHF、AI Agent

## 目標學校

- 國立中正大學（CCU）
- 國立中山大學（NSYSU）
- 國立雲林科技大學（YunTech）

## 語言

Wiki 內容：繁體中文為主，英文術語、定義及考題為輔。
HTML 簡報：中英雙語，可切換顯示。