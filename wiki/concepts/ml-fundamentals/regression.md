---
title: "Regression"
title-zh: "回歸分析"
type: concept
slug: regression
created: 2026-11-24
updated: 2026-11-24
status: developing
tags:
  - ml-fundamentals
  - regression
  - classification
  - linear-models
sources: []
related:
  - "[[svm]]"
  - "[[decision-tree]]"
  - "[[k-nearest-neighbors]]"
confidence: high
contested: false
lang: zh-en
exam-appearance:
  ccu: []
  nsysu: []
  yuntech: []
---

# 回歸分析（Regression）

## 基本資訊

| 項目 | 內容 |
|------|------|
| 英文名稱 | Regression |
| 中文名稱 | 回歸分析 |
| 領域 | 傳統機器學習、統計學習 |
| 類型 | 有參數式、積極學習（Eager Learning） |
| 考古題出現紀錄 | 尚無考古題紀錄 |

## 中文概念說明

回歸（Regression）是監督式學習中用於預測連續數值目標的一類方法，其目標是學習輸入特徵（X）與連續輸出值（y）之間的映射函數。最基礎的形式是**線性回歸**（Linear Regression），假設輸入特徵與輸出之間存在線性關係，透過最小化預測值與真實值之間的**均方誤差**（Mean Squared Error, MSE）來估計參數（通識）。

**邏輯回歸**（Logistic Regression）雖名為「回歸」，但實際上是**二元分類**演算法。它在線性模型輸出上套用 Sigmoid 函數，將線性組合映射至 (0, 1) 區間，解釋為樣本屬於正類的機率。訓練目標是最大化似然函數（即最小化交叉熵損失），而非最小化 MSE（通識）。

為了防止過擬合，回歸模型常加入**正則化**（Regularization）項來懲罰過大的參數。**L1 正則化**（Lasso）在損失函數中加入 L1 範數（參數絕對值之和），具有使部分參數縮減至零的效果，可實現特徵選擇（Feature Selection）；**L2 正則化**（Ridge）加入 L2 範數（參數平方和），使參數整體縮小但不為零，對多重共線性（Multicollinearity）問題有良好的處理效果（通識）。

## 英文技術定義

Regression is a class of supervised learning methods for modeling the relationship between input features and a continuous-valued target variable. The simplest form, ordinary least squares (OLS) linear regression, assumes a linear relationship y = X·w + b and estimates the weight vector w by minimizing the mean squared error (MSE) between predictions and targets. The closed-form solution is given by the normal equations w = (X^T X)^{-1} X^T y, which can also be optimized iteratively via gradient descent.

Logistic regression, despite its name, is a binary classification model that applies the sigmoid function σ(z) = 1/(1+e^{-z}) to a linear combination of features, outputting a probability estimate for class membership. It is trained by maximizing the log-likelihood of the training labels, which is equivalent to minimizing binary cross-entropy loss. Logistic regression is a generalized linear model and a special case of the broader family of maximum entropy classifiers.

Regularization is introduced to control model complexity and reduce overfitting. L1 regularization (Lasso) adds the L1 norm of the weight vector to the loss function, promoting sparsity by driving some weights exactly to zero. L2 regularization (Ridge) adds the squared L2 norm, which shrinks all weights proportionally without producing exact zeros. Elastic Net combines both L1 and L2 penalties to balance their respective properties.

## 重要詞彙

| 英文術語 | 中文術語 | 簡要說明 |
|----------|----------|----------|
| Linear Regression | 線性回歸 | 假設輸入與輸出線性關係的基礎回歸模型 |
| Logistic Regression | 邏輯回歸 | 用於二元分類，輸出為類別機率 |
| MSE | 均方誤差 | Mean Squared Error，線性回歸的損失函數 |
| Gradient Descent | 梯度下降 | 迭代最佳化損失函數的通用方法 |
| Sigmoid Function | Sigmoid 函數 | 將實數映射至 (0,1) 的 S 形函數 |
| Binary Cross-Entropy | 二元交叉熵 | 邏輯回歸的損失函數 |
| Regularization | 正則化 | 防止過擬合的損失函數附加項 |
| L1 Regularization (Lasso) | L1 正則化 | 加入參數絕對值懲罰，可產生稀疏解 |
| L2 Regularization (Ridge) | L2 正則化 | 加入參數平方懲罰，使參數整體縮小 |
| Elastic Net | 彈性網路 | L1 + L2 的組合正則化 |
| Overfitting | 過擬合 | 模型在訓練集過度擬合，測試集表現差 |
| Feature Selection | 特徵選擇 | Lasso 的副作用，自動將不重要特徵權重歸零 |

## 核心原理

### 線性回歸（Linear Regression）

（通識）

**模型**：
```
y_hat = w · x + b = w_1*x_1 + w_2*x_2 + ... + w_d*x_d + b
```

**損失函數（MSE）**：
```
L(w, b) = (1/n) * sum((y_i - y_hat_i)^2)
```

**閉合解（Normal Equation）**：
```
w = (X^T * X)^(-1) * X^T * y
```

**梯度下降更新規則**：
```
w := w - alpha * (dL/dw)
b := b - alpha * (dL/db)

其中 dL/dw = -(2/n) * X^T * (y - y_hat)
```

### 邏輯回歸（Logistic Regression）

（通識）

**Sigmoid 函數**：
```
sigma(z) = 1 / (1 + exp(-z))
```

**模型**：
```
P(y=1|x) = sigma(w · x + b)
```

**損失函數（Binary Cross-Entropy）**：
```
L(w, b) = -(1/n) * sum(y_i * log(y_hat_i) + (1 - y_i) * log(1 - y_hat_i))
```

**決策邊界**：
```
若 P(y=1|x) >= 0.5，預測類別 1
若 P(y=1|x)  < 0.5，預測類別 0
（等價於 w · x + b >= 0 時預測類別 1）
```

### 正則化形式

（通識）

**Ridge Regression（L2）**：
```
L_ridge = MSE + lambda * sum(w_i^2)
```

**Lasso Regression（L1）**：
```
L_lasso = MSE + lambda * sum(|w_i|)
```

**Elastic Net（L1 + L2）**：
```
L_elastic = MSE + lambda_1 * sum(|w_i|) + lambda_2 * sum(w_i^2)
```

**lambda（正則化強度）的影響**：
- lambda 大：參數縮減幅度大，模型更簡單，可能欠擬合
- lambda 小：接近無正則化，可能過擬合

## 實例說明

**線性回歸範例**（通識）：房價預測

- 輸入特徵：房屋面積（x_1）、房間數（x_2）、距市中心距離（x_3）
- 模型：y_hat = 2.5*x_1 + 15*x_2 - 0.8*x_3 + 50
- 訓練：最小化所有訓練房屋的預測價格與實際成交價格的 MSE

**邏輯回歸範例**（通識）：垃圾郵件分類

- 輸入特徵：各詞彙出現頻率
- 輸出：P(垃圾郵件) = sigma(w · x + b)
- 若機率 >= 0.5，分類為垃圾郵件

## 易混淆概念

### Regression vs Classification（分類）

| 比較項目 | 回歸（Regression） | 分類（Classification） |
|----------|-------------------|----------------------|
| 輸出類型 | 連續數值（如 3.14、100.5） | 離散類別（如「貓」、「狗」、0、1） |
| 損失函數 | MSE、MAE | Cross-Entropy、Hinge Loss |
| 評估指標 | RMSE、R²、MAE | Accuracy、F1-Score、AUC |
| 典型模型 | Linear Regression | Logistic Regression、SVM |

**注意**：Logistic Regression 是**分類**模型，名稱雖含「Regression」但本質是分類器（通識）。

### Linear Regression vs Logistic Regression

| 比較項目 | Linear Regression | Logistic Regression |
|----------|------------------|---------------------|
| 任務類型 | 回歸（預測連續值） | 分類（預測類別機率） |
| 輸出範圍 | (-∞, +∞) | (0, 1) |
| 激活函數 | 無（恆等函數） | Sigmoid |
| 損失函數 | MSE | Binary Cross-Entropy |
| 決策邊界 | 不適用 | 線性超平面 |

### L1 vs L2 正則化

| 比較項目 | L1（Lasso） | L2（Ridge） |
|----------|------------|------------|
| 懲罰形式 | 絕對值 | 平方 |
| 稀疏性 | 是（部分 w 歸零） | 否（所有 w 縮小） |
| 特徵選擇 | 有（隱含） | 無 |
| 對異常值 | 更穩健 | 較敏感 |
| 適用情境 | 高維稀疏特徵 | 多重共線性 |

## 考試出題方向

尚無考古題紀錄。

## 相關來源

（尚無 raw 來源，以上內容為通識，待 Ingest 教科書後補充具體來源）
