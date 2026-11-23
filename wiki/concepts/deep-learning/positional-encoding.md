---
title: "Positional Encoding"
title-zh: "位置編碼"
type: concept
slug: positional-encoding
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
  - "[[multi-head-attention]]"
confidence: high
contested: false
lang: zh-en
exam-appearance:
  ccu: []
  nsysu: []
  yuntech: []
---

# 位置編碼（Positional Encoding）

## 基本資訊

| 項目 | 內容 |
|------|------|
| 英文名稱 | Positional Encoding |
| 中文名稱 | 位置編碼 |
| 領域 | 深度學習、自然語言處理 |
| 核心論文 | [[source-vaswani-2017-attention]] |
| 考古題出現紀錄 | 尚無考古題紀錄 |

## 中文概念說明

位置編碼（Positional Encoding）是 [[transformer-architecture|Transformer]] 架構中用於補充序列位置資訊的機制。由於 Transformer 的 [[self-attention|Self-Attention]] 機制在計算時對序列中所有位置同時計算，本身不具備捕捉位置順序的能力——相較於 RNN 透過逐步計算天然保有順序資訊，Transformer 若不額外加入位置資訊，對「貓追狗」與「狗追貓」會產生完全相同的表示。位置編碼的作用便是將位置資訊注入模型，使每個位置在嵌入空間中有所區別（來源：[[source-vaswani-2017-attention]]）。

Vaswani 2017 論文中採用**正弦位置編碼**（Sinusoidal Positional Encoding），使用不同頻率的正弦（sine）與餘弦（cosine）函數為每個位置生成一個固定的 d_model 維度向量。這個向量與對應位置的詞嵌入（Word Embedding）直接相加後，作為 Encoder 與 Decoder 的輸入。選擇正弦函數的理由是：(1) 函數值有界（[-1, 1]），與詞嵌入的尺度相容；(2) 任意固定位移 k 的位置編碼可以表示為當前位置編碼的線性函數，使模型更容易學習到相對位置關係（來源：[[source-vaswani-2017-attention]]）。

除了固定的正弦位置編碼，另一種常見做法是**可學習位置嵌入**（Learned Positional Embedding），即像詞嵌入一樣將位置視為可學習的參數，在訓練過程中調整。論文作者實驗比較兩種方法，發現在翻譯任務上效果幾乎相同；選擇正弦位置編碼的優點是可以外推到訓練時未見過的更長序列（來源：[[source-vaswani-2017-attention]]）。

## 英文技術定義

Positional encoding is a mechanism for injecting information about the absolute position of tokens into the input representations of a Transformer model. Because the self-attention mechanism is permutation-equivariant—it treats all positions symmetrically—the model would otherwise be insensitive to the order of elements in a sequence. Positional encodings are added to the input embeddings at the bottom of both the encoder and decoder stacks.

Vaswani et al. propose a sinusoidal positional encoding where each dimension of the encoding corresponds to a sinusoid with a geometrically spaced frequency:

```
PE(pos, 2i)   = sin(pos / 10000^(2i/d_model))
PE(pos, 2i+1) = cos(pos / 10000^(2i/d_model))
```

Here pos denotes the position in the sequence and i denotes the dimension index. The wavelengths range from 2π (at i=0) to 10000·2π (at i=d_model/2). A key theoretical property of this formulation is that the positional encoding at position pos+k can be represented as a linear function of PE(pos), which may help the model learn to attend to relative positions.

An alternative is learned positional embeddings, where a unique embedding vector is associated with each absolute position and trained alongside the model parameters. Empirically, both approaches yield similar performance on translation tasks, but sinusoidal encodings have the advantage of allowing the model to generalize to sequences longer than those seen during training.

## 重要詞彙

| 英文術語 | 中文術語 | 簡要說明 |
|----------|----------|----------|
| Positional Encoding | 位置編碼 | 注入位置資訊的固定或可學習向量 |
| Sinusoidal Encoding | 正弦位置編碼 | 以 sin/cos 函數生成的固定位置向量 |
| Learned Positional Embedding | 可學習位置嵌入 | 透過訓練學習的位置嵌入，與詞嵌入概念相似 |
| pos | 位置索引 | 序列中的絕對位置（0, 1, 2, ...） |
| i | 維度索引 | 位置編碼向量中的維度位置 |
| d_model | 模型維度 | 位置編碼向量的維度，與詞嵌入維度相同（512） |
| Wavelength | 波長 | 各維度正弦函數的週期，隨維度幾何級數增長 |
| Permutation Equivariant | 排列等變性 | Self-Attention 對輸入順序不敏感的特性 |
| Word Embedding | 詞嵌入 | 將詞彙映射至連續向量空間的表示 |
| Relative Position | 相對位置 | 兩個 token 在序列中的相對距離 |

## 核心原理

### 正弦位置編碼公式

（來源：[[source-vaswani-2017-attention]]）

```
PE(pos, 2i)   = sin(pos / 10000^(2i/d_model))
PE(pos, 2i+1) = cos(pos / 10000^(2i/d_model))
```

**符號說明**：
- pos：token 在序列中的位置（0-indexed）
- i：維度索引，範圍 0 至 d_model/2 - 1
- d_model = 512：每個位置編碼向量的維度
- 偶數維度（2i）使用 sine，奇數維度（2i+1）使用 cosine

**特性**：
- 波長範圍：從 2π（i=0，最短波長，捕捉相鄰位置差異）到 10000 × 2π（i=255，最長波長，捕捉遠距離位置關係）
- 相對位置性：PE(pos+k) 可由 PE(pos) 線性表示，有助於模型學習相對位置關係

### 位置編碼的加入方式

```
輸入表示 = Embedding(token) + PE(pos)
```

詞嵌入向量與位置編碼向量**直接相加**（非串接），兩者維度均為 d_model=512。加總後的向量作為 Encoder 或 Decoder 第一層的輸入。

為防止位置編碼的加入影響詞嵌入的尺度，模型在加入位置編碼前會將詞嵌入向量乘以 sqrt(d_model) 進行縮放（來源：[[source-vaswani-2017-attention]]）。

### 正弦編碼 vs 可學習嵌入比較

| 比較項目 | 正弦位置編碼 | 可學習位置嵌入 |
|----------|------------|--------------|
| 是否需要訓練 | 否（固定公式） | 是（隨模型訓練） |
| 長序列外推能力 | 可外推到更長序列 | 受限於訓練時的最大長度 |
| 相對位置表達 | 理論上可學習 | 訓練資料充足時效果佳 |
| 實際翻譯效果 | 相近 | 相近 |
| 後續研究採用 | 仍有使用（T5 等） | 更多模型採用（BERT、GPT） |

（來源：[[source-vaswani-2017-attention]]）

## 實例說明

以序列「cat sat mat」（pos=0, 1, 2），d_model=4（簡化示意）為例（通識）：

```
PE(0, 0) = sin(0 / 10000^0)   = sin(0) = 0
PE(0, 1) = cos(0 / 10000^0)   = cos(0) = 1
PE(0, 2) = sin(0 / 10000^0.5) = sin(0) = 0
PE(0, 3) = cos(0 / 10000^0.5) = cos(0) = 1
=> 位置 0 的編碼：[0, 1, 0, 1]

PE(1, 0) = sin(1 / 1)         = sin(1) ≈ 0.841
PE(1, 1) = cos(1 / 1)         = cos(1) ≈ 0.540
...
=> 位置 1 的編碼：[0.841, 0.540, ...]
```

每個位置獲得唯一的編碼向量，與詞嵌入相加後，模型即可區分相同詞彙出現在不同位置時的不同含義。

## 易混淆概念

### Positional Encoding vs Word Embedding

詞嵌入（Word Embedding）編碼的是**語義資訊**（詞彙的含義），不同詞彙有不同的嵌入向量；位置編碼（Positional Encoding）編碼的是**位置資訊**（token 在序列中的位置），與詞彙內容無關。兩者相加後共同構成 Transformer 的輸入表示。

### Positional Encoding vs Positional Embedding

在習慣用語上，「Positional Encoding」通常特指 Vaswani 2017 論文中基於公式的**固定**位置向量；「Positional Embedding」通常指**可學習**的位置嵌入（如 BERT、GPT 中使用的方式）。兩者都是位置資訊注入的方法，但實現方式不同。

## 考試出題方向

尚無考古題紀錄。

## 相關來源

- [[source-vaswani-2017-attention]]：正弦位置編碼公式的原始出處，以及與可學習位置嵌入的比較實驗
