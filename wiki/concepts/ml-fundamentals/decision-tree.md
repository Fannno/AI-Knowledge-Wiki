---
title: "Decision Tree"
title-zh: "決策樹"
type: concept
slug: decision-tree
created: 2026-11-24
updated: 2026-11-24
status: developing
tags:
  - ml-fundamentals
  - classification
  - regression
  - tree-methods
sources: []
related:
  - "[[k-nearest-neighbors]]"
  - "[[svm]]"
confidence: high
contested: false
lang: zh-en
exam-appearance:
  ccu: []
  nsysu: []
  yuntech: []
---

# 決策樹（Decision Tree）

## 基本資訊

| 項目 | 內容 |
|------|------|
| 英文名稱 | Decision Tree |
| 中文名稱 | 決策樹 |
| 領域 | 傳統機器學習、分類、回歸 |
| 類型 | 非參數式、積極學習（Eager Learning） |
| 考古題出現紀錄 | 尚無考古題紀錄 |

## 中文概念說明

決策樹（Decision Tree）是一種樹狀結構的監督式學習模型，能夠同時用於分類（Classification）與回歸（Regression）任務。模型的結構由三種節點組成：根節點（Root Node，位於頂部）、內部節點（Internal Node，代表特徵判斷條件）與葉節點（Leaf Node，代表最終預測值或類別）。決策時，樣本從根節點開始，依照每個內部節點的判斷條件（如「年齡 > 30？」）逐步向下，直到到達葉節點得出預測結果（通識）。

決策樹的核心在於**如何選擇最佳分裂特徵（Splitting Feature）與分裂點**。最常見的準則是**資訊增益**（Information Gain），基於**熵**（Entropy）來衡量資料集的不純度（Impurity）：熵越高，資料越混亂；選擇能最大程度降低熵的特徵進行分裂，這是 ID3 演算法的做法。C4.5 演算法改進為使用**增益率**（Gain Ratio）以解決資訊增益偏好多值特徵的問題；CART 演算法則使用**吉尼不純度**（Gini Impurity）（通識）。

決策樹的主要缺點是容易**過擬合**（Overfitting）——未加限制的決策樹會一直分裂直到每個葉節點只包含一種類別的樣本，導致決策邊界過於複雜、在測試資料上表現差。解決方法是**剪枝**（Pruning）：預剪枝（Pre-pruning）在訓練時設定深度上限、最小樣本數等條件提早停止；後剪枝（Post-pruning）先讓樹完整生長，再由下往上移除不必要的分支（通識）。

## 英文技術定義

A Decision Tree is a non-parametric supervised learning model that partitions the feature space into a hierarchical series of axis-aligned rectangles, assigning a class label (for classification) or a constant value (for regression) to each leaf partition. The tree structure consists of a root node, internal decision nodes, and leaf nodes. At each internal node, a split condition on a single feature is applied to route examples to child nodes.

Tree induction algorithms select the best feature and split point at each node by maximizing an impurity reduction criterion. The ID3 algorithm uses Information Gain based on Shannon entropy; C4.5 uses Gain Ratio to normalize for the number of distinct feature values; CART (Classification and Regression Trees) uses Gini Impurity for classification and mean squared error reduction for regression.

A fully-grown decision tree tends to overfit, as it creates a separate leaf region for every training example in the extreme case. Regularization is achieved through pruning: pre-pruning applies stopping criteria (maximum depth, minimum samples per leaf) during training, while post-pruning removes branches after the tree is fully grown using held-out validation data or statistical significance tests.

## 重要詞彙

| 英文術語 | 中文術語 | 簡要說明 |
|----------|----------|----------|
| Root Node | 根節點 | 決策樹頂部的第一個分裂點 |
| Internal Node | 內部節點 | 代表一個特徵判斷條件的節點 |
| Leaf Node | 葉節點 | 樹的末端，代表最終預測結果 |
| Entropy | 熵 | 衡量資料集不純度的指標，越高越混亂 |
| Information Gain | 資訊增益 | 分裂前後熵的減少量，ID3 的分裂準則 |
| Gain Ratio | 增益率 | 資訊增益除以特徵自身熵，C4.5 的改進 |
| Gini Impurity | 吉尼不純度 | 衡量不純度的另一種指標，CART 採用 |
| Pruning | 剪枝 | 減少決策樹複雜度、防止過擬合的技術 |
| Pre-pruning | 預剪枝 | 訓練時設限提早停止生長 |
| Post-pruning | 後剪枝 | 訓練後移除不必要分支 |
| Splitting Criterion | 分裂準則 | 選擇最佳分裂特徵的評估標準 |
| ID3 / C4.5 / CART | 決策樹演算法 | 三種主流決策樹歸納演算法 |

## 核心原理

### 熵（Entropy）公式

（通識）

```
H(S) = -sum_c p(c) * log2(p(c))
```

- S：訓練資料集
- c：各類別
- p(c)：類別 c 在 S 中的比例
- H(S) = 0：所有樣本屬於同一類別（純淨）
- H(S) = 1（二元分類）：兩類樣本各佔 50%（最混亂）

### 資訊增益（Information Gain）

（通識）

```
IG(S, A) = H(S) - sum_v (|S_v| / |S|) * H(S_v)
```

- A：候選分裂特徵
- v：特徵 A 的各個取值
- S_v：特徵 A = v 的子集
- 選擇使 IG 最大的特徵 A 進行分裂

### 吉尼不純度（Gini Impurity）

（通識）

```
Gini(S) = 1 - sum_c p(c)^2
```

- Gini = 0：所有樣本屬於同一類別
- Gini 越大，資料越不純
- CART 選擇使加權 Gini 不純度最小的分裂特徵

### 三種主流演算法比較

| 特性 | ID3 | C4.5 | CART |
|------|-----|------|------|
| 分裂準則 | 資訊增益 | 增益率 | 吉尼不純度 / MSE |
| 分裂類型 | 多分支（categorical） | 多分支 | 二分支（binary） |
| 支援連續特徵 | 否 | 是 | 是 |
| 支援缺失值 | 否 | 是 | 有限 |
| 支援回歸 | 否 | 否 | 是 |

## 實例說明

以「是否核准貸款申請」為例（通識）：

```
                 收入 > 50K?
                /            \
           是                  否
           |                   |
      信用評分 > 700?       拒絕貸款
       /         \
    是             否
    |               |
  核准貸款       有無擔保品?
                 /      \
              有           無
              |             |
           核准貸款      拒絕貸款
```

每個內部節點代表一個特徵判斷，沿分支走到葉節點得出決策結果，決策過程透明可解釋。

## 易混淆概念

### Decision Tree vs Random Forest（隨機森林）

隨機森林（Random Forest）是由多棵決策樹組成的集成學習（Ensemble Learning）方法。每棵樹以不同的**自助抽樣**（Bootstrap Sampling）訓練資料和隨機選取的特徵子集建立，最後以多數投票（分類）或平均（回歸）整合結果。Random Forest 大幅降低了單棵決策樹的過擬合問題，但犧牲了部分可解釋性（通識）。

### Decision Tree vs KNN

| 比較項目 | Decision Tree | [[k-nearest-neighbors|KNN]] |
|----------|--------------|-----|
| 可解釋性 | 高（規則樹狀圖） | 低 |
| 訓練速度 | O(nd log n) | O(1) |
| 推論速度 | O(深度 d_tree) | O(nd) |
| 決策邊界 | 軸對齊矩形 | 非線性（距離） |
| 高維度效果 | 相對較好 | 差（維度詛咒） |

## 考試出題方向

尚無考古題紀錄。

## 相關來源

（尚無 raw 來源，以上內容為通識，待 Ingest 教科書後補充具體來源）
