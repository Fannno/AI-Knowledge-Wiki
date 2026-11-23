---
title: "K-Nearest Neighbors"
title-zh: "K 近鄰演算法"
type: concept
slug: k-nearest-neighbors
created: 2026-11-24
updated: 2026-11-24
status: developing
tags:
  - ml-fundamentals
  - classification
  - regression
  - non-parametric
sources: []
related:
  - "[[svm]]"
  - "[[decision-tree]]"
confidence: high
contested: false
lang: zh-en
exam-appearance:
  ccu: []
  nsysu: []
  yuntech: []
---

# K 近鄰演算法（K-Nearest Neighbors）

## 基本資訊

| 項目 | 內容 |
|------|------|
| 英文名稱 | K-Nearest Neighbors (KNN) |
| 中文名稱 | K 近鄰演算法 |
| 領域 | 傳統機器學習、分類、回歸 |
| 類型 | 非參數式（Non-parametric）、懶惰學習（Lazy Learning） |
| 考古題出現紀錄 | 尚無考古題紀錄 |

## 中文概念說明

K 近鄰演算法（K-Nearest Neighbors, KNN）是一種最基礎的監督式學習（Supervised Learning）演算法，屬於非參數式（Non-parametric）方法，意指其不對資料分佈做任何假設。KNN 的核心思想極為直觀：要預測一個新樣本的類別（分類）或數值（回歸），只需找到訓練集中與其距離最近的 K 個樣本，然後依多數投票（分類）或平均值（回歸）做出預測（通識）。

KNN 屬於懶惰學習（Lazy Learning）演算法——訓練階段幾乎不進行任何計算，只是將訓練資料儲存起來；所有計算均在預測（推論）時才進行。這與大多數機器學習模型在訓練階段建立模型參數的做法截然不同。KNN 的優點是易於理解與實作，且不需要訓練時間；缺點是推論時必須與所有訓練樣本計算距離，時間複雜度為 O(nd)，其中 n 為訓練集大小、d 為特徵維度（通識）。

超參數 K 的選擇對模型效果影響顯著：K 過小時，模型容易受到雜訊影響，產生過擬合（Overfitting）；K 過大時，模型過於平滑，可能產生欠擬合（Underfitting）。此外，KNN 在高維度資料上效果通常較差，這是「維度詛咒」（Curse of Dimensionality）的具體表現——在高維空間中，所有點之間的距離趨於相等，使得「最近鄰」的概念失去意義（通識）。

## 英文技術定義

K-Nearest Neighbors (KNN) is a non-parametric, instance-based supervised learning algorithm that makes predictions by finding the K training examples closest to a query point in the feature space. For classification, the predicted label is determined by majority vote among the K neighbors; for regression, the prediction is the mean (or weighted mean) of their target values.

KNN is a lazy learner: it defers all computation to inference time, storing the entire training dataset without building an explicit model during the training phase. At prediction time, the algorithm computes the distance between the query point and all n training points, identifies the K nearest, and aggregates their labels. This results in O(nd) inference complexity per query, where n is the number of training samples and d is the number of features.

The choice of K controls the bias-variance tradeoff: small K values yield low-bias, high-variance models susceptible to overfitting, while large K values increase bias and reduce variance. KNN is also sensitive to the scale of features—distances should be computed on normalized data—and degrades in performance under the curse of dimensionality, since in high-dimensional spaces, distances between all pairs of points tend to converge, undermining the notion of meaningful proximity.

## 重要詞彙

| 英文術語 | 中文術語 | 簡要說明 |
|----------|----------|----------|
| K | K 值 | 最近鄰的數量，核心超參數 |
| Distance Metric | 距離度量 | 衡量兩樣本之間距離的函數 |
| Euclidean Distance | 歐幾里得距離 | 最常用的距離度量，直線距離 |
| Manhattan Distance | 曼哈頓距離 | 各維度差值絕對值之和 |
| Minkowski Distance | 明氏距離 | 歐幾里得與曼哈頓的廣義形式 |
| Majority Vote | 多數投票 | 分類任務中取 K 個鄰居中最多票的類別 |
| Non-parametric | 非參數式 | 不對資料分佈做假設的方法 |
| Lazy Learning | 懶惰學習 | 不在訓練期間建立模型，推論時才計算 |
| Curse of Dimensionality | 維度詛咒 | 高維空間中距離度量失效的現象 |
| Overfitting | 過擬合 | K 過小時對訓練資料過度擬合 |
| Feature Scaling | 特徵縮放 | KNN 使用前的必要前處理，使各特徵尺度一致 |

## 核心原理

### 演算法步驟

（通識）

1. **特徵縮放前處理**：將所有特徵縮放至相同尺度（如 Min-Max 或 Z-score 正規化），避免高量值特徵主導距離計算。

2. **選定超參數 K**：K 通常為奇數（避免分類任務中出現平局），常見方法是透過交叉驗證（Cross-Validation）選擇最佳 K 值。

3. **計算距離**：對每個測試樣本，計算其與訓練集中所有樣本的距離。

4. **找出 K 個最近鄰**：依距離排序，取前 K 個訓練樣本。

5. **做出預測**：
   - **分類**：取 K 個鄰居中出現次數最多的類別（多數投票）
   - **回歸**：取 K 個鄰居目標值的平均（或加權平均）

### 常用距離度量公式

（通識）

**歐幾里得距離（Euclidean Distance，p=2 Minkowski）**：
```
d(x, y) = sqrt(sum((x_i - y_i)^2))
```

**曼哈頓距離（Manhattan Distance，p=1 Minkowski）**：
```
d(x, y) = sum(|x_i - y_i|)
```

**明氏距離（Minkowski Distance，廣義形式）**：
```
d(x, y) = (sum(|x_i - y_i|^p))^(1/p)
```

### 超參數 K 的影響

| K 值 | 決策邊界 | 偏差 | 方差 | 傾向 |
|------|---------|------|------|------|
| 小（如 K=1） | 複雜、不規則 | 低 | 高 | 過擬合 |
| 大（如 K=n） | 平滑、接近多數類 | 高 | 低 | 欠擬合 |
| 適中（經交叉驗證） | 適當平衡 | 適中 | 適中 | 最佳泛化 |

### 時間與空間複雜度

| 操作 | 複雜度 |
|------|--------|
| 訓練 | O(1)（只儲存資料） |
| 推論（每個查詢） | O(nd)（暴力搜索） |
| 儲存空間 | O(nd)（需儲存所有訓練資料） |

n = 訓練樣本數，d = 特徵維度（通識）

## 實例說明

以二元分類問題為例（通識）：訓練集有 10 個樣本，分為「貓」與「狗」兩類，每個樣本有體重與腿長兩個特徵。

給定一個新樣本（體重=4kg，腿長=20cm），使用 K=3：

1. 計算與所有 10 個訓練樣本的歐幾里得距離
2. 找出距離最近的 3 個樣本：假設為「貓、貓、狗」
3. 多數投票：貓 2 票 > 狗 1 票
4. 預測結果：「貓」

## 易混淆概念

### KNN vs SVM

| 比較項目 | KNN | [[svm|SVM]] |
|----------|-----|-----|
| 學習類型 | 懶惰學習（無訓練） | 積極學習（訓練建立模型） |
| 推論速度 | 慢 O(nd) | 快（只需少數支持向量） |
| 決策邊界 | 局部、非線性 | 全域、最大邊界超平面 |
| 記憶體需求 | 高（儲存全部訓練資料） | 低（只儲存支持向量） |
| 適用資料規模 | 小資料集 | 中大型資料集 |

### KNN vs Decision Tree

| 比較項目 | KNN | [[decision-tree|Decision Tree]] |
|----------|-----|--------------|
| 可解釋性 | 低（黑盒查詢） | 高（規則可視化） |
| 訓練速度 | O(1) | O(nd log n) |
| 推論速度 | O(nd) | O(log n) |
| 對缺失值的處理 | 困難 | 可處理 |
| 高維度效果 | 差（維度詛咒） | 相對較好 |

## 考試出題方向

尚無考古題紀錄。

## 相關來源

（尚無 raw 來源，以上內容為通識，待 Ingest 教科書後補充具體來源）
