---
title: "Clustering"
title-zh: "分群演算法"
type: concept
slug: clustering
created: 2026-11-24
updated: 2026-11-24
status: developing
tags:
  - ml-fundamentals
  - unsupervised-learning
  - clustering
sources: []
related:
  - "[[k-nearest-neighbors]]"
  - "[[decision-tree]]"
confidence: high
contested: false
lang: zh-en
exam-appearance:
  ccu: []
  nsysu: []
  yuntech: []
---

# 分群演算法（Clustering）

## 基本資訊

| 項目 | 內容 |
|------|------|
| 英文名稱 | Clustering |
| 中文名稱 | 分群演算法 |
| 領域 | 傳統機器學習、非監督式學習 |
| 類型 | 非監督式（Unsupervised）、探索性分析 |
| 考古題出現紀錄 | 尚無考古題紀錄 |

## 中文概念說明

分群（Clustering）是**非監督式學習**（Unsupervised Learning）的核心任務之一，目標是在沒有事先標記的情況下，將資料集中的樣本依照某種相似性度量自動劃分為若干組（群，Cluster）——同一群內的樣本彼此相似，不同群的樣本彼此差異大。與監督式分類任務不同，分群在訓練時沒有標籤（Label）可供參考（通識）。

最廣為人知的分群演算法是 **K-Means**，其設計簡單、效率高，適用於大規模資料。K-Means 需要事先指定群數 K，透過迭代方式交替更新群中心（Centroid）與樣本群歸屬，直到收斂。演算法對初始化敏感，不同的初始群中心可能導致不同的最終結果，實務上常採用 K-Means++ 初始化策略改善（通識）。

選擇合適的 K 值是 K-Means 的一大挑戰。常見的方法是**手肘法**（Elbow Method）：繪製不同 K 值對應的群內距離平方和（Within-Cluster Sum of Squares, WCSS）曲線，尋找曲線「轉折點」（如手肘形狀）作為最佳 K。此外，**輪廓係數**（Silhouette Score）提供更嚴格的量化評估，同時衡量群內緊密度與群間分離度（通識）。

## 英文技術定義

Clustering is an unsupervised learning task that partitions a dataset into groups—clusters—such that intra-cluster similarity is maximized and inter-cluster similarity is minimized, without relying on labeled training examples. It is used for exploratory data analysis, pattern discovery, and as a preprocessing step in many machine learning pipelines.

K-Means is the most widely used clustering algorithm. Given the number of clusters K, it iteratively alternates between two steps: assigning each data point to the nearest centroid (measured by Euclidean distance) and recomputing each centroid as the mean of all points assigned to it. The algorithm minimizes the within-cluster sum of squared distances (WCSS) objective:

```
WCSS = sum_k sum_{x in C_k} ||x - mu_k||^2
```

K-Means is sensitive to initialization; K-Means++ mitigates this by selecting initial centroids with probability proportional to their distance from already-chosen centroids. The algorithm is guaranteed to converge but may converge to a local minimum.

Hierarchical clustering offers an alternative that does not require K to be specified in advance. Agglomerative hierarchical clustering begins with each point as its own cluster and iteratively merges the two most similar clusters until a single cluster remains, producing a dendrogram that can be cut at any desired level. The silhouette score provides a principled metric for evaluating cluster quality by measuring both intra-cluster cohesion and inter-cluster separation.

## 重要詞彙

| 英文術語 | 中文術語 | 簡要說明 |
|----------|----------|----------|
| Cluster | 群 | 由演算法劃分出的相似樣本組 |
| Centroid | 群中心 | K-Means 中每個群的平均位置向量 |
| K-Means | K 均值演算法 | 最常用的分群演算法，需事先指定 K |
| K-Means++ | K 均值++ | K-Means 的改進初始化策略 |
| WCSS | 群內距離平方和 | Within-Cluster Sum of Squares，K-Means 最佳化目標 |
| Elbow Method | 手肘法 | 透過 WCSS 曲線轉折點選擇最佳 K 值的方法 |
| Silhouette Score | 輪廓係數 | 衡量分群品質的量化指標，範圍 [-1, 1] |
| Hierarchical Clustering | 階層式分群 | 不需預設 K，產生樹狀結構的分群方法 |
| Dendrogram | 樹狀圖 | 階層式分群結果的樹狀視覺化 |
| Agglomerative | 凝聚式 | 自底向上合併的階層式分群策略 |
| Divisive | 分裂式 | 自頂向下拆分的階層式分群策略（較少用） |
| Intra-cluster Distance | 群內距離 | 同一群內樣本之間的平均距離 |
| Inter-cluster Distance | 群間距離 | 不同群樣本之間的平均距離 |

## 核心原理

### K-Means 演算法步驟

（通識）

1. **初始化**：隨機選取 K 個訓練樣本作為初始群中心 μ_1, ..., μ_K（K-Means++ 採用加權隨機選取）。

2. **分配步驟（Assignment Step）**：
   ```
   對每個樣本 x_i：
   c_i = argmin_k ||x_i - mu_k||^2
   ```
   將每個樣本分配給距離最近的群中心。

3. **更新步驟（Update Step）**：
   ```
   對每個群 k：
   mu_k = (1/|C_k|) * sum_{x_i in C_k} x_i
   ```
   將群中心更新為該群所有樣本的均值。

4. **重複步驟 2-3**，直到群歸屬不再改變（收斂）或達到最大迭代次數。

**目標函數**：
```
最小化 WCSS = sum_k sum_{x in C_k} ||x - mu_k||^2
```

### 手肘法（Elbow Method）選擇 K 值

（通識）

```
步驟：
1. 對 K = 1, 2, 3, ..., max_K 分別執行 K-Means
2. 記錄每個 K 對應的 WCSS
3. 繪製 WCSS vs K 的折線圖
4. 找到曲線下降速率顯著減慢的「轉折點」（手肘）作為最佳 K
```

**侷限性**：有時曲線平滑無明顯轉折點，此時搭配輪廓係數評估更為可靠。

### 輪廓係數（Silhouette Score）

（通識）

對於每個樣本 x_i：
```
a(i) = 樣本 i 與同群其他樣本的平均距離（群內緊密度）
b(i) = 樣本 i 與最近鄰群所有樣本的平均距離（群間分離度）

s(i) = (b(i) - a(i)) / max(a(i), b(i))
```

**整體輪廓係數** = 所有樣本 s(i) 的平均值

| 分數範圍 | 解讀 |
|---------|------|
| 接近 +1 | 樣本與所在群高度相似，分群效果良好 |
| 接近 0 | 樣本位於群邊界，分群模糊 |
| 接近 -1 | 樣本可能被錯誤分群 |

### 階層式分群（Hierarchical Clustering）

（通識）

**凝聚式（Agglomerative）流程**：

```
1. 初始：每個樣本各自為一個群（n 個群）
2. 重複：合併距離最近的兩個群
3. 直到：所有樣本合為一個群
4. 產出：樹狀圖（Dendrogram）
5. 決策：在所需層級切割 Dendrogram 得到 K 個群
```

**群間距離定義方式（Linkage Criteria）**：

| 連結方式 | 定義 | 特性 |
|---------|------|------|
| Single Linkage | 兩群中最近兩點的距離 | 易形成鏈狀群 |
| Complete Linkage | 兩群中最遠兩點的距離 | 趨向球形群 |
| Average Linkage | 兩群所有點對的平均距離 | 折中，較常用 |
| Ward Linkage | 合併後 WCSS 增量最小 | 趨向等大小群 |

## 實例說明

**K-Means 客戶分群範例**（通識）：

電商平台欲將客戶依購買行為分群，特徵為「每月消費金額」與「購買頻率」：

1. 設定 K=3（高價值、中等、低活躍）
2. 初始化 3 個隨機群中心
3. 迭代分配與更新：
   - 收斂後群 1：高消費高頻率（VIP 客戶）
   - 群 2：中等消費中頻率（一般客戶）
   - 群 3：低消費低頻率（流失風險客戶）
4. 對不同群客戶採取差異化行銷策略

**選擇 K 值**：用手肘法測試 K=1 到 8，發現 K=3 時 WCSS 下降速率顯著放緩，確認 K=3 為最佳。

## 易混淆概念

### Clustering（分群） vs Classification（分類）

| 比較項目 | Clustering | Classification |
|----------|-----------|---------------|
| 學習類型 | 非監督式（無標籤） | 監督式（有標籤） |
| 群/類別定義 | 由演算法自動發現 | 由訓練標籤預先定義 |
| 評估方式 | 輪廓係數、WCSS 等 | Accuracy、F1-Score 等 |
| 典型演算法 | K-Means、Hierarchical | KNN、SVM、Decision Tree |
| 應用 | 探索性分析、市場分群 | 垃圾郵件過濾、圖像辨識 |

### K-Means vs K-Nearest Neighbors (KNN)

雖然兩者名稱都含 K，但本質截然不同：
- **K-Means**：**非監督式分群**演算法，K 代表群數
- **[[k-nearest-neighbors|KNN]]**：**監督式分類/回歸**演算法，K 代表最近鄰數量

兩者沒有關聯性，共用字母 K 純屬巧合（通識）。

## 考試出題方向

尚無考古題紀錄。

## 相關來源

（尚無 raw 來源，以上內容為通識，待 Ingest 教科書後補充具體來源）
