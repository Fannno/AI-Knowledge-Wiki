---
title: "Transformer Architecture"
title-zh: "Transformer 架構"
type: concept
slug: transformer-architecture
created: 2026-11-24
updated: 2026-11-24
status: developing
tags:
  - deep-learning
  - attention
  - nlp
  - sequence-modeling
sources:
  - "[[source-vaswani-2017-attention]]"
related:
  - "[[self-attention]]"
  - "[[multi-head-attention]]"
  - "[[positional-encoding]]"
  - "[[encoder-decoder]]"
confidence: high
contested: false
lang: zh-en
exam-appearance:
  ccu: []
  nsysu: []
  yuntech: []
---

# Transformer 架構

## 基本資訊

| 項目 | 內容 |
|------|------|
| 英文名稱 | Transformer Architecture |
| 中文名稱 | Transformer 架構 |
| 領域 | 深度學習、自然語言處理 |
| 提出年份 | 2017 |
| 核心論文 | [[source-vaswani-2017-attention]] |
| 考古題出現紀錄 | 尚無考古題紀錄 |

## 中文概念說明

Transformer 是 Vaswani 等人於 2017 年提出的序列轉換（sequence transduction）模型架構，其核心特徵在於**完全不依賴遞迴（Recurrence）或卷積（Convolution）**，而是完全建立在注意力機制（Attention Mechanism）之上。這一設計突破使 Transformer 能夠對輸入序列進行高度平行化的計算，克服了傳統遞迴神經網路（RNN）因逐步運算導致難以充分利用現代 GPU 的問題（來源：[[source-vaswani-2017-attention]]）。

Transformer 採用編碼器-解碼器（[[encoder-decoder|Encoder-Decoder]]）結構。**編碼器**（Encoder）由 N=6 層相同的 Layer 堆疊組成，每層包含兩個子層：(1) [[multi-head-attention|Multi-Head Self-Attention]] 機制，(2) Position-wise Feed-Forward Network（FFN）。**解碼器**（Decoder）同樣由 6 層組成，每層額外包含一個對 Encoder 輸出進行 Cross-Attention 的子層。每個子層均配有殘差連接（Residual Connection）與層正規化（Layer Normalization），形式為 LayerNorm(x + Sublayer(x))（來源：[[source-vaswani-2017-attention]]）。

Transformer 的重要性不僅在於其在機器翻譯任務上創下的 BLEU 紀錄，更在於它奠定了現代大型語言模型（LLM）的基礎架構。BERT、GPT 系列、T5 等模型均以 Transformer 的 Encoder 或 Decoder 為骨幹，使 Transformer 成為當代 AI 最具影響力的架構之一（來源：[[source-vaswani-2017-attention]]）。

## 英文技術定義

The Transformer is a sequence transduction architecture introduced by Vaswani et al. (2017) that relies entirely on attention mechanisms, dispensing with recurrence and convolutions entirely. It maps an input sequence of symbol representations to a continuous sequence of representations, which a decoder then uses to generate an output sequence one symbol at a time.

The model follows an encoder-decoder paradigm. The encoder is composed of a stack of N=6 identical layers, each containing two sub-layers: a multi-head self-attention mechanism and a position-wise fully connected feed-forward network. The decoder similarly consists of N=6 layers, augmented with a third sub-layer performing multi-head attention over the encoder output. Residual connections and layer normalization are applied around each sub-layer. The model dimension is d_model=512 throughout.

A key limitation of the Transformer is its quadratic memory and computational complexity with respect to sequence length in the self-attention sub-layers—O(n²d)—since every position attends to every other position in the sequence. For sequences shorter than the model dimension, however, self-attention is faster than recurrent layers and captures long-range dependencies in a single step.

## 重要詞彙

| 英文術語 | 中文術語 | 簡要說明 |
|----------|----------|----------|
| Transformer | Transformer | 本論文提出的核心模型架構，完全基於注意力機制 |
| Encoder | 編碼器 | 將輸入序列映射為連續表示的堆疊 Layer 結構 |
| Decoder | 解碼器 | 根據 Encoder 輸出逐步生成輸出序列的結構 |
| Self-Attention | 自注意力 | 序列內位置間互相計算注意力的機制 |
| Multi-Head Attention | 多頭注意力 | 並行執行多個 Attention 以捕捉多種子空間依賴關係 |
| Positional Encoding | 位置編碼 | 注入序列位置資訊以補充注意力機制的不足 |
| Residual Connection | 殘差連接 | 子層輸出加上輸入後再進行正規化，緩解梯度消失 |
| Layer Normalization | 層正規化 | 在每個子層輸出前進行的正規化操作 |
| Feed-Forward Network | 前饋網路 | 每個 Layer 中的兩層全連接網路，d_ff=2048 |
| d_model | 模型維度 | 所有子層的輸出維度，本論文設定為 512 |
| Scaled Dot-Product Attention | 縮放點積注意力 | Transformer 的基本注意力計算單元 |
| Masked Attention | 遮罩注意力 | Decoder 中防止看到未來位置的注意力變體 |

## 核心原理

### 整體架構流程

```
輸入序列
   |
   v
Input Embedding + Positional Encoding
   |
   v
Encoder (x6 layers)
   |- Multi-Head Self-Attention
   |- Add & Norm
   |- Feed-Forward Network
   |- Add & Norm
   |
   v
Encoder Output
   |
   v
Decoder (x6 layers)
   |- Masked Multi-Head Self-Attention
   |- Add & Norm
   |- Multi-Head Cross-Attention (Q from Decoder, K/V from Encoder)
   |- Add & Norm
   |- Feed-Forward Network
   |- Add & Norm
   |
   v
Linear + Softmax
   |
   v
輸出序列（機率分佈）
```

### Scaled Dot-Product Attention 公式

（來源：[[source-vaswani-2017-attention]]）

```
Attention(Q, K, V) = softmax(QK^T / sqrt(dk)) * V
```

- Q (Query)：查詢矩陣
- K (Key)：鍵矩陣
- V (Value)：值矩陣
- dk：Key 向量的維度（本論文中 dk = 64）
- 除以 sqrt(dk) 的原因：當 dk 較大時，Q 與 K 的點積值可能很大，使 softmax 進入梯度近乎為零的飽和區域；除以 sqrt(dk) 可將數值縮放至適當範圍

### Multi-Head Attention 公式

（來源：[[source-vaswani-2017-attention]]）

```
MultiHead(Q, K, V) = Concat(head_1, ..., head_h) * W^O

其中 head_i = Attention(QW^Q_i, KW^K_i, VW^V_i)
```

- h = 8（本論文設定的頭數）
- dk = dv = d_model / h = 512 / 8 = 64
- W^Q_i, W^K_i, W^V_i 為各 head 的投影矩陣
- W^O 為最終輸出投影矩陣

### Position-wise Feed-Forward Network

（來源：[[source-vaswani-2017-attention]]）

```
FFN(x) = max(0, xW_1 + b_1) * W_2 + b_2
```

- 內層維度 d_ff = 2048
- 使用 ReLU 激活函數
- 對序列每個位置獨立且相同地應用（position-wise）

### Positional Encoding

（來源：[[source-vaswani-2017-attention]]）

```
PE(pos, 2i)   = sin(pos / 10000^(2i/d_model))
PE(pos, 2i+1) = cos(pos / 10000^(2i/d_model))
```

- pos：位置索引（0, 1, 2, ...）
- i：維度索引
- d_model = 512
- 位置編碼與詞嵌入向量直接相加後輸入模型

## 實例說明

以英德機器翻譯（WMT 2014）為例（來源：[[source-vaswani-2017-attention]]）：

**輸入**（英文）："The cat sat on the mat."

**Encoder 處理流程**：
1. 每個詞經 Embedding 轉為 512 維向量，加上對應的 Positional Encoding
2. 通過 6 層 Encoder，每層的 Self-Attention 讓每個詞能「看到」其他所有詞
3. 例如處理「sat」時，Self-Attention 分數會對「cat」（主詞）給予較高的注意力權重

**Decoder 生成流程**：
1. 從特殊起始符號（\<BOS\>）開始逐步生成
2. 每個生成步驟，Decoder 透過 Cross-Attention 參考 Encoder 的全序列表示
3. 依序生成「Die」→「Katze」→「saß」→...直到生成終止符

**實驗結果**：BLEU 28.4（英德）、41.8（英法），優於當時所有先前最佳方法（來源：[[source-vaswani-2017-attention]]）。

## 易混淆概念

### Transformer vs RNN（循環神經網路）

| 比較項目 | Transformer | RNN / LSTM |
|----------|-------------|------------|
| 計算方式 | 平行計算整個序列 | 逐步計算（t 依賴 t-1） |
| 長距離依賴 | O(1) 路徑長度，直接建立連結 | O(n) 路徑，梯度消失風險 |
| 訓練速度 | 快（GPU 高度平行化） | 慢（無法完全平行） |
| 序列長度擴展性 | 記憶體需求 O(n²) | 記憶體需求 O(n) |
| 位置資訊 | 需要 Positional Encoding | 天然具有順序性 |

（來源：[[source-vaswani-2017-attention]]）

### Transformer vs BERT

BERT（Bidirectional Encoder Representations from Transformers）是僅使用 Transformer **Encoder** 部分的預訓練模型，採用雙向（bidirectional）Self-Attention，適合理解類任務（分類、問答）。Transformer 原始架構為完整的 Encoder-Decoder 結構，適合序列轉換任務（翻譯、摘要）。

### Transformer vs GPT

GPT（Generative Pre-trained Transformer）是僅使用 Transformer **Decoder** 部分（帶 Masked Attention）的生成式預訓練模型，採用單向（左至右）注意力。GPT 適合文本生成任務，而原始 Transformer 是完整的 Encoder-Decoder 架構，兩者的應用場景不同。

## 考試出題方向

尚無考古題紀錄。

## 相關來源

- [[source-vaswani-2017-attention]]：本架構的原始論文，所有技術細節均以此為基準
