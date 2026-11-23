---
title: "Support Vector Machine"
title-zh: "支持向量機"
type: concept
slug: svm
created: 2026-11-24
updated: 2026-11-24
status: developing
tags:
  - ml-fundamentals
  - classification
  - kernel-methods
sources: []
related:
  - "[[k-nearest-neighbors]]"
  - "[[decision-tree]]"
  - "[[regression]]"
confidence: high
contested: false
lang: zh-en
exam-appearance:
  ccu: []
  nsysu: []
  yuntech: []
---

# 支持向量機（Support Vector Machine）

## 基本資訊

| 項目 | 內容 |
|------|------|
| 英文名稱 | Support Vector Machine (SVM) |
| 中文名稱 | 支持向量機 |
| 領域 | 傳統機器學習、分類、回歸 |
| 類型 | 有參數式、積極學習（Eager Learning） |
| 考古題出現紀錄 | 尚無考古題紀錄 |

## 中文概念說明

支持向量機（Support Vector Machine, SVM）是一種強大的監督式學習演算法，最初由 Vapnik 等人於 1990 年代提出，主要用於二元分類任務。SVM 的核心思想是在特徵空間中尋找一個能夠**最大化兩類別之間邊界（Margin）**的超平面（Hyperplane），這個原則稱為**最大邊界分類**（Maximum Margin Classification）。直觀上，邊界越大，模型對未見資料的泛化能力越強（通識）。

在 SVM 中，決定決策邊界的訓練樣本稱為**支持向量**（Support Vector）。支持向量是位於邊界邊緣（即距離超平面最近）的訓練樣本，決策超平面完全由這些少數樣本決定。這一特性使 SVM 具有良好的記憶體效率，推論時只需要支持向量的資訊，而非整個訓練集（通識）。

對於線性不可分（Linearly Inseparable）的資料，SVM 透過**核技巧**（Kernel Trick）將資料隱含地映射至更高維的特徵空間，使其在高維空間中線性可分，而無需顯式計算高維特徵（計算效率高）。同時，**軟邊界**（Soft Margin）引入了超參數 C，允許少數樣本落在邊界內甚至被錯誤分類，以在最大化邊界與最小化分類錯誤之間取得平衡（通識）。

## 英文技術定義

A Support Vector Machine (SVM) is a discriminative classifier that finds the optimal separating hyperplane—the one that maximizes the margin between two classes—in a potentially high-dimensional feature space. For a binary classification problem with labeled training data {(x_i, y_i)}, the SVM solves a constrained quadratic optimization problem to find the hyperplane w·x + b = 0 that separates the classes while maximizing the geometric margin 2/||w||.

The training examples that lie on the margin boundaries are called support vectors; they are the only points that influence the position and orientation of the decision hyperplane. This sparsity of representation gives SVMs favorable memory properties at inference time. When the data are not linearly separable, the kernel trick implicitly maps inputs into a reproducing kernel Hilbert space via a kernel function K(x_i, x_j) = φ(x_i)·φ(x_j), enabling a linear separator to be found in that space without computing the explicit feature mapping φ.

The soft-margin SVM introduces a slack variable ξ_i and a regularization hyperparameter C that controls the tradeoff between maximizing the margin and penalizing margin violations. A large C enforces strict classification at the cost of a smaller margin, while a small C tolerates more misclassifications in exchange for a wider margin.

## 重要詞彙

| 英文術語 | 中文術語 | 簡要說明 |
|----------|----------|----------|
| Hyperplane | 超平面 | n 維空間中的 (n-1) 維子空間，SVM 的決策邊界 |
| Margin | 邊界 | 兩類別樣本到決策超平面的最小距離之和 |
| Maximum Margin | 最大邊界 | SVM 的核心目標：最大化兩類別間的邊界寬度 |
| Support Vector | 支持向量 | 位於邊界上的訓練樣本，決定決策超平面 |
| Kernel Trick | 核技巧 | 隱含地將資料映射至高維空間的技術 |
| RBF Kernel | 徑向基函數核 | 最常用的非線性核函數，K(x,y)=exp(-γ||x-y||²) |
| Polynomial Kernel | 多項式核 | K(x,y)=(x·y+c)^d，用於多項式特徵映射 |
| Linear Kernel | 線性核 | K(x,y)=x·y，等同於線性 SVM |
| Soft Margin | 軟邊界 | 允許部分樣本落在邊界內的 SVM 變體 |
| Hard Margin | 硬邊界 | 要求完全線性可分的 SVM 原始形式 |
| C (Parameter) | 正則化參數 C | 控制邊界寬度與分類錯誤之間的權衡 |
| Slack Variable | 鬆弛變數 | Soft Margin SVM 中允許違反邊界的變數 ξ |

## 核心原理

### 最大邊界超平面（Hard Margin SVM）

（通識）

**決策邊界**（超平面）：
```
w · x + b = 0
```

**邊界的兩條邊**：
```
w · x + b = +1   （正類支持向量所在）
w · x + b = -1   （負類支持向量所在）
```

**邊界寬度**（Margin）：
```
Margin = 2 / ||w||
```

**最佳化問題**（最大化 Margin，等價於最小化 ||w||）：
```
最小化: (1/2) * ||w||²
約束: y_i * (w · x_i + b) >= 1，for all i
```

### Soft Margin SVM（帶 C 超參數）

（通識）

```
最小化: (1/2) * ||w||² + C * sum(ξ_i)
約束: y_i * (w · x_i + b) >= 1 - ξ_i
      ξ_i >= 0
```

**C 的影響**：
- C 大：強調正確分類，邊界可能較窄，傾向過擬合
- C 小：強調邊界寬度，允許較多錯誤，傾向欠擬合

### 核函數（Kernel Functions）

（通識）

| 核函數 | 公式 | 適用情境 |
|--------|------|---------|
| 線性核 | K(x,y) = x·y | 資料線性可分 |
| 多項式核 | K(x,y) = (γ·x·y + r)^d | 多項式特徵關係 |
| RBF 核 | K(x,y) = exp(-γ·\|\|x-y\|\|²) | 通用，最常用 |
| Sigmoid 核 | K(x,y) = tanh(γ·x·y + r) | 類似神經網路 |

**核技巧**：直接在輸入空間計算核函數值 K(x_i, x_j)，等效於在高維特徵空間計算內積 φ(x_i)·φ(x_j)，無需顯式計算 φ（通識）。

## 實例說明

**二元分類範例**（通識）：

資料集包含兩類郵件（垃圾郵件 vs 正常郵件），特徵為詞頻向量（高維稀疏）。

1. 使用線性 SVM：在詞頻空間中尋找最大邊界超平面
2. 支持向量為最難分類的幾封郵件（位於邊界上）
3. 新郵件送入時，計算 w·x+b：
   - 若 > 0：預測為垃圾郵件
   - 若 < 0：預測為正常郵件

由於 SVM 只需儲存支持向量（通常遠少於訓練集總數），推論效率遠優於 KNN。

## 易混淆概念

### SVM vs Logistic Regression（邏輯回歸）

| 比較項目 | SVM | [[regression|Logistic Regression]] |
|----------|-----|---------------------|
| 決策原則 | 最大化邊界 | 最大化似然（概率） |
| 輸出 | 類別標籤 | 類別機率（Sigmoid） |
| 核技巧 | 支援 | 通常不使用（但可擴展） |
| 異常值敏感性 | 低（由支持向量決定） | 高（所有樣本均影響） |
| 適用資料量 | 中小型 | 大型資料集效率更高 |

### SVM vs Neural Network（神經網路）

| 比較項目 | SVM | 神經網路 |
|----------|-----|---------|
| 訓練目標 | 最大化邊界 | 最小化損失函數 |
| 特徵提取 | 需要人工或核函數 | 自動學習特徵 |
| 可擴展性 | 大資料集效率差 | 適合大資料集 |
| 解釋性 | 中（支持向量可見） | 低（深層網路） |
| 超參數 | 少（C, kernel） | 多（層數、學習率等） |

## 考試出題方向

尚無考古題紀錄。

## 相關來源

（尚無 raw 來源，以上內容為通識，待 Ingest 教科書後補充具體來源）
