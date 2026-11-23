---
title: "Attention Is All You Need"
title-zh: "注意力機制就是一切你所需要的"
type: source
slug: source-vaswani-2017-attention
created: 2026-11-24
authors:
  - Ashish Vaswani
  - Noam Shazeer
  - Niki Parmar
  - Jakob Uszkoreit
  - Llion Jones
  - Aidan N. Gomez
  - Lukasz Kaiser
  - Illia Polosukhin
source-url: "https://arxiv.org/abs/1706.03762"
source-type: paper
raw-path: "raw/papers/vaswani-2017-attention-is-all-you-need.pdf"
date-published: 2017-06-12
date-ingested: 2026-11-24
ingest-status: full-read
ingest-note: "論文全文透過 ar5iv.labs.arxiv.org/html/1706.03762 完整讀取，涵蓋摘要、架構說明（Section 1-3）、Why Self-Attention（Section 4）、訓練設定（Section 5）、實驗結果（Section 6）及完整參考文獻。PDF 已下載至 raw/papers/vaswani-2017-attention-is-all-you-need.pdf（2.2MB）。"
concepts-updated:
  - "[[transformer-architecture]]"
  - "[[self-attention]]"
  - "[[multi-head-attention]]"
  - "[[positional-encoding]]"
  - "[[encoder-decoder]]"
---

# Attention Is All You Need

## 論文背景與意義

本論文由 Google Brain 與 Google Research 團隊於 2017 年提出，發表於 NIPS 2017。論文標題「Attention Is All You Need」直接點明核心主張：序列轉換任務（sequence transduction）所需的一切，均可透過注意力機制（Attention Mechanism）達成，無需仰賴遞迴神經網路（RNN）或卷積神經網路（CNN）。

本論文影響深遠，Transformer 架構已成為現代自然語言處理（NLP）的基礎，後續 BERT、GPT 系列等大型語言模型均以 Transformer 為骨幹。

## 核心主張

本論文提出 [[transformer-architecture|Transformer]] 架構，核心主張如下：

1. **完全基於注意力機制**：捨棄 RNN 的逐步遞迴計算與 CNN 的卷積操作，改用純粹的 [[self-attention|Self-Attention]] 機制處理序列中的全域依賴關係（global dependencies）。
2. **可高度平行化**：RNN 因逐步計算的特性無法充分平行化；Transformer 的 Self-Attention 可對整個序列同時計算，大幅提升訓練效率。
3. **解決長距離依賴問題**：RNN 在處理長序列時梯度消失問題嚴重；Transformer 透過注意力直接建立任意位置之間的連結，理論上不受序列長度限制。

## 架構說明

### Encoder-Decoder 結構

Transformer 採用編碼器-解碼器（Encoder-Decoder）結構：

- **Encoder**：由 N=6 層相同的 Layer 堆疊組成。每層包含兩個子層：(1) Multi-Head Self-Attention；(2) Position-wise Feed-Forward Network（FFN）。每個子層均使用殘差連接（Residual Connection）與層正規化（Layer Normalization）。
- **Decoder**：同樣由 N=6 層組成。每層包含三個子層：(1) Masked Multi-Head Self-Attention（防止看到未來資訊）；(2) Multi-Head Cross-Attention（與 Encoder 輸出進行交叉注意力）；(3) Position-wise FFN。

模型維度 d_model = 512。

### Scaled Dot-Product Attention

[[self-attention|Scaled Dot-Product Attention]] 公式：

```
Attention(Q, K, V) = softmax(QK^T / sqrt(dk)) * V
```

- Q（Query）、K（Key）、V（Value）分別為查詢、鍵、值矩陣
- dk = 64（每個 head 的鍵向量維度）
- 除以 sqrt(dk)：防止內積結果過大，使 softmax 函數進入梯度極小的飽和區域

### Multi-Head Attention

[[multi-head-attention|Multi-Head Attention]] 使用 h=8 個並行的注意力頭（head）：

```
MultiHead(Q, K, V) = Concat(head_1, ..., head_h) * W^O
head_i = Attention(QW^Q_i, KW^K_i, VW^V_i)
```

每個 head 的維度 dk = dv = d_model / h = 512 / 8 = 64。各 head 可學習不同子空間中的依賴關係，最後將所有 head 的輸出串接後投影。

### Position-wise Feed-Forward Networks

每個 Encoder/Decoder Layer 均包含一個兩層全連接前饋網路：

```
FFN(x) = max(0, xW_1 + b_1) * W_2 + b_2
```

內層維度 d_ff = 2048，使用 ReLU 激活函數。

### Positional Encoding

[[positional-encoding|Positional Encoding]] 使用正弦（sine）與餘弦（cosine）函數：

```
PE(pos, 2i)   = sin(pos / 10000^(2i/d_model))
PE(pos, 2i+1) = cos(pos / 10000^(2i/d_model))
```

位置編碼與詞嵌入（word embedding）相加後輸入 Encoder/Decoder。

## 與 RNN 的比較

| 特性 | Transformer | RNN |
|------|-------------|-----|
| 計算方式 | 平行計算 | 逐步計算 |
| 長距離依賴 | 直接建立連結，O(1) 路徑長度 | 路徑長度隨序列增長 |
| 訓練效率 | 高（可充分利用 GPU） | 低（無法完全平行） |
| 序列內計算複雜度 | O(n²d) | O(nd²) |
| 額外記憶體需求 | O(n²)（Attention 矩陣） | O(1) |

當序列長度 n 遠小於模型維度 d 時，Self-Attention 層計算效率優於 RNN。

## 訓練與實驗結果

- **資料集**：WMT 2014 英德（English-German）與英法（English-French）翻譯任務
- **優化器**：Adam，學習率採 warmup 策略
- **正規化**：Dropout（P=0.1）、Label Smoothing（epsilon=0.1）
- **結果**：
  - 英德翻譯 BLEU 28.4，優於當時所有先前方法（包含 ensemble 模型）
  - 英法翻譯 BLEU 41.8
  - 訓練效率遠優於 RNN 架構（相同效果所需計算量更少）

## 引用的關鍵概念

- [[transformer-architecture]]：本論文提出的核心架構
- [[self-attention]]：Scaled Dot-Product Attention
- [[multi-head-attention]]：多頭注意力機制
- [[positional-encoding]]：正弦位置編碼
- [[encoder-decoder]]：編碼器-解碼器架構
