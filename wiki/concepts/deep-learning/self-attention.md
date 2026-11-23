---
title: "Self-Attention"
title-zh: "自注意力機制"
type: concept
slug: self-attention
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
  - "[[multi-head-attention]]"
  - "[[positional-encoding]]"
confidence: high
contested: false
lang: zh-en
exam-appearance:
  ccu: []
  nsysu: []
  yuntech: []
---

# 自注意力機制（Self-Attention）

## 基本資訊

| 項目 | 內容 |
|------|------|
| 英文名稱 | Self-Attention / Intra-Attention |
| 中文名稱 | 自注意力機制 |
| 領域 | 深度學習、自然語言處理 |
| 核心論文 | [[source-vaswani-2017-attention]] |
| 考古題出現紀錄 | 尚無考古題紀錄 |

## 中文概念說明

自注意力機制（Self-Attention），又稱內部注意力（Intra-Attention），是一種計算序列中各位置之間關聯性的機制。與傳統注意力機制（Attention Mechanism）的差異在於：傳統注意力作用於**兩個不同**的序列之間（如機器翻譯中 Encoder 與 Decoder 之間的 Cross-Attention），而 Self-Attention 則作用於**同一**序列的不同位置之間，使每個位置能夠整合整個序列的資訊（來源：[[source-vaswani-2017-attention]]）。

Self-Attention 的核心操作是 Scaled Dot-Product Attention。對於輸入序列的每個位置，模型分別計算一個查詢向量（Query, Q）、一個鍵向量（Key, K）和一個值向量（Value, V）。Q 與所有位置的 K 進行點積運算以計算注意力分數（Attention Score），再透過 Softmax 函數正規化得到注意力權重，最後以此權重對所有位置的 V 進行加權求和，得到該位置的輸出（來源：[[source-vaswani-2017-attention]]）。

Self-Attention 的關鍵優勢在於，任意兩個位置之間的關聯路徑長度（path length）為常數 O(1)，相較於 RNN 的 O(n) 路徑長度，Self-Attention 能夠更有效地捕捉長距離依賴（long-range dependency）。然而其計算複雜度隨序列長度呈平方增長（O(n²d)），對於非常長的序列是一項挑戰（來源：[[source-vaswani-2017-attention]]）。

## 英文技術定義

Self-attention, also known as intra-attention, is an attention mechanism that relates different positions within a single sequence to compute a representation of that sequence. Unlike cross-attention, which operates between two distinct sequences, self-attention allows each position in a sequence to attend to all other positions within the same sequence, thereby directly capturing global dependencies regardless of their distance.

The core computation is Scaled Dot-Product Attention, formulated as:

```
Attention(Q, K, V) = softmax(QK^T / sqrt(dk)) * V
```

The queries, keys, and values are all derived from the same input sequence via learned linear projections. The scaling factor sqrt(dk) prevents the dot products from growing large in magnitude as the dimensionality dk increases, which would otherwise push the softmax function into regions with extremely small gradients and impede learning.

Self-attention achieves a maximum path length of O(1) between any two positions, in contrast to recurrent architectures where this length is O(n). This property makes self-attention particularly effective at modeling long-range dependencies, though at the cost of O(n²) memory complexity for storing the full attention matrix over a sequence of length n.

## 重要詞彙

| 英文術語 | 中文術語 | 簡要說明 |
|----------|----------|----------|
| Query (Q) | 查詢向量 | 當前位置發出的「查詢」，用於與其他位置比對 |
| Key (K) | 鍵向量 | 各位置的「索引」，與 Query 進行點積計算相似度 |
| Value (V) | 值向量 | 各位置的「內容」，根據注意力權重加權求和 |
| Attention Score | 注意力分數 | QK^T 的結果，代表兩位置之間的相關程度 |
| Scaled Dot-Product | 縮放點積 | 除以 sqrt(dk) 後的點積，防止數值過大 |
| Softmax | Softmax 函數 | 將注意力分數轉換為加總為 1 的機率分佈 |
| Attention Weight | 注意力權重 | Softmax 後的正規化分數，決定各位置貢獻比例 |
| dk | 鍵向量維度 | Key 向量的維度，本論文設定為 64 |
| Long-range Dependency | 長距離依賴 | 序列中距離較遠的兩個位置之間的關聯 |
| Intra-Attention | 內部注意力 | Self-Attention 的別名，強調序列內部操作 |

## 核心原理

### Scaled Dot-Product Attention 完整計算步驟

（來源：[[source-vaswani-2017-attention]]）

給定輸入矩陣 X（序列長度為 n，每個位置維度為 d_model）：

1. **線性投影**：
   ```
   Q = X * W^Q    （形狀：n × dk）
   K = X * W^K    （形狀：n × dk）
   V = X * W^V    （形狀：n × dv）
   ```

2. **點積計算**：
   ```
   scores = Q * K^T    （形狀：n × n，每個元素代表一對位置的相關性）
   ```

3. **縮放**：
   ```
   scaled_scores = scores / sqrt(dk)
   ```
   原因：當 dk 較大時，點積結果的方差會較大，導致 softmax 輸出幾乎為 one-hot，梯度近乎為零；除以 sqrt(dk) 可穩定梯度（來源：[[source-vaswani-2017-attention]]）。

4. **（可選）遮罩（Masking）**：
   Decoder 的 Self-Attention 中，對未來位置填入負無限大（-∞），確保生成時不會「看到」未來的詞。

5. **Softmax 正規化**：
   ```
   weights = softmax(scaled_scores)    （每列加總為 1）
   ```

6. **加權求和**：
   ```
   output = weights * V    （形狀：n × dv）
   ```

### 完整公式

```
Attention(Q, K, V) = softmax(QK^T / sqrt(dk)) * V
```

## 實例說明

以句子「The animal didn't cross the street because it was too tired.」為例：

當模型處理「it」這個詞時，Self-Attention 機制會計算「it」的 Query 與句子中所有詞的 Key 之間的相似度。結果可能是「animal」獲得最高的注意力權重，表示模型理解「it」指的是「animal」而非「street」。這種指代消解（coreference resolution）能力正是 Self-Attention 長距離建模能力的體現（通識）。

## 易混淆概念

### Self-Attention vs Cross-Attention（交叉注意力）

| 比較項目 | Self-Attention | Cross-Attention |
|----------|---------------|-----------------|
| Q 來源 | 同一序列 | Decoder 端 |
| K, V 來源 | 同一序列 | Encoder 輸出 |
| 應用位置 | Encoder 與 Decoder 的內部 | Decoder 中用於整合 Encoder 資訊 |
| 功能 | 捕捉序列內部依賴 | 對齊輸入與輸出序列 |

### Self-Attention vs Multi-Head Attention（多頭注意力）

Self-Attention 是**單一**的注意力計算單元；[[multi-head-attention|Multi-Head Attention]] 是**並行執行多個** Self-Attention 的機制，各 head 擁有獨立的 W^Q, W^K, W^V 投影矩陣，可學習不同子空間中的依賴關係，最後將所有 head 的輸出串接。Multi-Head Attention 是以 Self-Attention 為基礎的進階機制（來源：[[source-vaswani-2017-attention]]）。

## 考試出題方向

尚無考古題紀錄。

## 相關來源

- [[source-vaswani-2017-attention]]：Scaled Dot-Product Attention 公式與原理的原始出處
