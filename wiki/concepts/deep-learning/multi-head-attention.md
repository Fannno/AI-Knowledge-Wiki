---
title: "Multi-Head Attention"
title-zh: "多頭注意力機制"
type: concept
slug: multi-head-attention
created: 2026-11-24
updated: 2026-11-24
status: developing
tags:
  - deep-learning
  - attention
  - nlp
  - transformer
sources:
  - "[[source-vaswani-2017-attention]]"
related:
  - "[[transformer-architecture]]"
  - "[[self-attention]]"
  - "[[positional-encoding]]"
confidence: high
contested: false
lang: zh-en
exam-appearance:
  ccu: []
  nsysu: []
  yuntech: []
---

# 多頭注意力機制（Multi-Head Attention）

## 基本資訊

| 項目 | 內容 |
|------|------|
| 英文名稱 | Multi-Head Attention |
| 中文名稱 | 多頭注意力機制 |
| 領域 | 深度學習、自然語言處理 |
| 核心論文 | [[source-vaswani-2017-attention]] |
| 考古題出現紀錄 | 尚無考古題紀錄 |

## 中文概念說明

多頭注意力機制（Multi-Head Attention）是 [[transformer-architecture|Transformer]] 架構中的核心模組，由 Vaswani 等人於 2017 年提出。其設計理念是：單一的注意力函數（Single-Head Attention）一次只能在一個表示子空間（subspace）中計算依賴關係；而透過並行執行多個注意力頭（head），模型可以同時在多個不同的子空間中學習不同類型的依賴關係，例如一個 head 可能學習短距離的語法依賴，另一個 head 學習長距離的語義依賴（來源：[[source-vaswani-2017-attention]]）。

Multi-Head Attention 的操作流程如下：首先，對輸入的 Query（Q）、Key（K）、Value（V）矩陣分別使用 h 組不同的可學習線性投影矩陣（W^Q_i, W^K_i, W^V_i），將其投影至較低維度的子空間；接著，對每組投影後的矩陣獨立執行 [[self-attention|Scaled Dot-Product Attention]]；最後，將 h 個 head 的輸出沿維度方向串接（Concatenate），再通過一個輸出投影矩陣 W^O，得到最終輸出（來源：[[source-vaswani-2017-attention]]）。

在 Vaswani 2017 的原始設定中，模型使用 h=8 個並行的注意力頭，模型總維度 d_model=512，因此每個 head 的 Q/K/V 維度均為 dk = dv = d_model / h = 512 / 8 = 64。由於維度隨 head 數等比例縮減，Multi-Head Attention 的總計算成本與單一全維度注意力（Single-Head Attention with d_model）相近，但表達能力顯著提升（來源：[[source-vaswani-2017-attention]]）。

## 英文技術定義

Multi-Head Attention is a generalization of the standard attention mechanism that runs h attention functions in parallel over the same input. Rather than applying a single attention function to the full-dimensional queries, keys, and values, the model projects them h times into distinct lower-dimensional subspaces using learned linear transformations, computes attention independently in each subspace, and then concatenates the resulting representations before applying a final linear projection.

The formal definition is:

```
MultiHead(Q, K, V) = Concat(head_1, ..., head_h) * W^O

where head_i = Attention(Q * W^Q_i, K * W^K_i, V * W^V_i)
```

With h=8 heads and d_model=512, the per-head dimensionality is dk = dv = 64. The total computational cost is therefore comparable to that of a single-head attention with full dimensionality, since the reduction in dimension per head compensates for the increased number of projections. The different heads are able to jointly attend to information from different representation subspaces and positions, capturing a richer set of relational structures within the input sequence.

## 重要詞彙

| 英文術語 | 中文術語 | 簡要說明 |
|----------|----------|----------|
| Head | 注意力頭 | 一組獨立的 Attention 計算單元，各有其投影矩陣 |
| Projection Matrix | 投影矩陣 | 將高維輸入投影至低維子空間的可學習權重矩陣 |
| Subspace | 子空間 | 每個 head 學習的特定表示空間 |
| Concat | 串接 | 將所有 head 的輸出沿維度方向合併 |
| W^O | 輸出投影矩陣 | 串接後用於整合各 head 資訊的可學習矩陣 |
| h | 頭數 | 並行的 Attention head 數量，本論文設定為 8 |
| dk | 鍵向量維度 | 每個 head 的 Key/Query 向量維度（= d_model / h = 64） |
| dv | 值向量維度 | 每個 head 的 Value 向量維度（= d_model / h = 64） |
| d_model | 模型維度 | 整體模型的輸出維度，本論文設定為 512 |
| Single-Head Attention | 單頭注意力 | 只有一個 head 的基本注意力計算，Multi-Head 的基礎 |

## 核心原理

### Multi-Head Attention 完整公式

（來源：[[source-vaswani-2017-attention]]）

```
MultiHead(Q, K, V) = Concat(head_1, head_2, ..., head_h) * W^O

其中：
  head_i = Attention(Q * W^Q_i, K * W^K_i, V * W^V_i)
  Attention(Q, K, V) = softmax(QK^T / sqrt(dk)) * V
```

**超參數設定（本論文）**：
- h = 8（頭數）
- d_model = 512
- dk = dv = d_model / h = 64
- 投影矩陣維度：W^Q_i ∈ R^(d_model × dk)，W^K_i ∈ R^(d_model × dk)，W^V_i ∈ R^(d_model × dv)
- 輸出投影矩陣：W^O ∈ R^(h*dv × d_model)

### 計算流程圖

```
輸入 Q, K, V（維度：n × d_model）
         |
         |-- Linear(W^Q_1) --> Q_1（n × dk）--|
         |-- Linear(W^K_1) --> K_1（n × dk）  |--> Attention --> head_1（n × dv）
         |-- Linear(W^V_1) --> V_1（n × dv）--|
         |
         |-- Linear(W^Q_2) --> Q_2（n × dk）--|
         |-- Linear(W^K_2) --> K_2（n × dk）  |--> Attention --> head_2（n × dv）
         |-- Linear(W^V_2) --> V_2（n × dv）--|
         |
         |   ... (共 h=8 組) ...
         |
         Concat(head_1, ..., head_8)（n × h*dv = n × 512）
                              |
                       Linear(W^O)
                              |
                      輸出（n × d_model）
```

### 與 Single-Head Attention 的計算量比較

Single-Head Attention（全維度 dk=512）：
- Q, K, V 投影：O(d_model²)
- Attention 計算：O(n² * d_model)

Multi-Head Attention（h=8, dk=64）：
- 每個 head 的投影：O(d_model * dk) × h = O(d_model²)（總量相同）
- 每個 head 的 Attention：O(n² * dk) × h = O(n² * d_model)（總量相同）
- 額外的輸出投影：O(d_model²)

**結論**：Multi-Head Attention 的計算成本與 Single-Head Attention 相近，但表達能力更強（來源：[[source-vaswani-2017-attention]]）。

## 實例說明

以英文句子「The bank can guarantee deposits will eventually cover future tuition costs because it is so effective.」為例（通識）：

在 Multi-Head Attention 中，不同的 head 可能分別學習到：
- **Head 1**：「it」與「bank」的指代關係（長距離語義依賴）
- **Head 2**：「deposits」與「cover」的動賓關係（短距離語法依賴）
- **Head 3**：「because」引導的因果關係（篇章結構依賴）

每個 head 在自己的子空間中捕捉不同類型的語言模式，最終串接後形成豐富的序列表示。

## 易混淆概念

### Multi-Head Attention vs Single-Head Attention

Single-Head Attention 在完整的 d_model 維度空間中執行一次 Attention，只能學習一種注意力模式；Multi-Head Attention 並行執行 h 組 Attention，每組在獨立的 dk 維子空間中學習，可同時捕捉 h 種不同的依賴關係（來源：[[source-vaswani-2017-attention]]）。

### Multi-Head Attention vs Self-Attention

[[self-attention|Self-Attention]] 是注意力的基本計算單元（Attention 函數）；Multi-Head Attention 是以 Self-Attention 為基礎的**組合機制**，透過多組 Self-Attention 的並行執行來增強表達能力。可以說，Multi-Head Attention 是 Self-Attention 的擴展版本（來源：[[source-vaswani-2017-attention]]）。

## 考試出題方向

尚無考古題紀錄。

## 相關來源

- [[source-vaswani-2017-attention]]：Multi-Head Attention 公式與超參數設定的原始出處
