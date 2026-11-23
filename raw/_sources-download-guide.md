# raw/_sources-download-guide.md
# 初始來源下載說明

本文件說明如何取得本專案初始知識來源的原始資料。
建立日期：2026-11-23

每份資料下載後，請放入 raw/ 對應子目錄，並通知 AI 執行 Ingest 操作。

---

## 一、學術論文

### 1. Attention Is All You Need（Vaswani et al., 2017）

- 涵蓋概念：Transformer、Self-Attention、Multi-Head Attention、Positional Encoding、Encoder-Decoder
- 下載網址：https://arxiv.org/pdf/1706.03762
- 存放位置：`raw/papers/vaswani-2017-attention-is-all-you-need.pdf`
- 授權：學術論文，免費公開

---

### 2. Playing Atari with Deep Reinforcement Learning（Mnih et al., 2013）

- 涵蓋概念：Deep Q-Network (DQN)、Experience Replay、Target Network、Q-Learning
- 下載網址：https://arxiv.org/pdf/1312.5602
- 存放位置：`raw/papers/mnih-2013-dqn-atari.pdf`
- 授權：學術論文，免費公開

---

### 3. Language Models are Few-Shot Learners（Brown et al., 2020，GPT-3）

- 涵蓋概念：LLM、Token、In-context Learning、Few-Shot Learning、Prompt
- 下載網址：https://arxiv.org/pdf/2005.14165
- 存放位置：`raw/papers/brown-2020-gpt3-few-shot.pdf`
- 授權：學術論文，免費公開

---

### 4. A Training Algorithm for Optimal Margin Classifiers（Boser, Guyon, Vapnik, 1992）

- 涵蓋概念：Support Vector Machine (SVM)、Maximum Margin、Kernel 概念起源
- 下載網址：https://dl.acm.org/doi/pdf/10.1145/130385.130401
- 存放位置：`raw/papers/boser-1992-svm-original.pdf`
- 備注：可能需要 ACM 機構帳號。若無法取得，可用下方 Burges 1998 教學文作為替代。

---

### 5. A Tutorial on Support Vector Machines for Pattern Recognition（Burges, 1998）

- 涵蓋概念：SVM、Kernel Trick、Soft Margin、VC Dimension
- 下載網址：https://link.springer.com/content/pdf/10.1023/A:1009715923555.pdf
- 備用網址（Microsoft Research）：https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/svmtutorial.pdf
- 存放位置：`raw/papers/burges-1998-svm-tutorial.pdf`
- 授權：免費公開（Springer 開放存取）

---

### 6. Induction of Decision Trees（Quinlan, 1986）

- 涵蓋概念：Decision Tree、ID3 Algorithm、Information Gain、Entropy
- 下載網址：https://link.springer.com/content/pdf/10.1007/BF00116251.pdf
- 存放位置：`raw/papers/quinlan-1986-decision-trees.pdf`
- 備注：Springer 論文，可能需要機構帳號。若無法取得，請記錄原因於 source 頁面。

---

## 二、教科書與公開教材

### 7. Pattern Recognition and Machine Learning（Bishop, 2006）

- 涵蓋概念：KNN、Regression（Linear、Logistic）、Clustering、SVM（第 7 章）、EM Algorithm、PCA
- 下載網址：https://www.microsoft.com/en-us/research/uploads/prod/2006/01/Bishop-Pattern-Recognition-and-Machine-Learning-2006.pdf
- 存放位置：`raw/textbooks/bishop-2006-prml.pdf`
- 授權：作者提供免費 PDF，合法下載

下載後建議先 Ingest 以下章節（其他章節可後續補充）：
- Chapter 1：Introduction（KNN 範例、決策理論）
- Chapter 3：Linear Models for Regression
- Chapter 4：Linear Models for Classification（Logistic Regression）
- Chapter 7：Sparse Kernel Machines（SVM）
- Chapter 9：Mixture Models and EM（Clustering）

---

### 8. Deep Learning（Goodfellow, Bengio, Courville, 2016）

- 涵蓋概念：Neural Network、Perceptron、Backpropagation、CNN、RNN、LSTM
- 線上閱讀：https://www.deeplearningbook.org/
- 下載網址：本書無官方 PDF 下載，請透過以下方式取得：
  - 合法方式：購買紙本或電子版（MIT Press）
  - 線上閱讀章節 HTML 版本：https://www.deeplearningbook.org/contents/
- 存放位置：若取得 PDF，存放於 `raw/textbooks/goodfellow-2016-deep-learning.pdf`
- 若僅能取得線上版，以各章節 HTML 內容為準，Ingest 時記錄 `ingest-status: partial-read`

建議 Ingest 章節：
- Chapter 6：Deep Feedforward Networks（Neural Network、Backpropagation）
- Chapter 9：Convolutional Networks（CNN）
- Chapter 10：Sequence Modeling（RNN、LSTM）

---

### 9. CS229 Machine Learning Lecture Notes（Andrew Ng, Stanford）

- 涵蓋概念：監督式學習（Supervised Learning）、Linear Regression、Logistic Regression、SVM、非監督式學習（Unsupervised Learning）、K-Means、EM、PCA、RL
- 下載網址：https://cs229.stanford.edu/main_notes.pdf
- 存放位置：`raw/textbooks/ng-cs229-lecture-notes.pdf`
- 授權：公開教材，免費下載

---

## 三、官方文件

### 10. scikit-learn 使用者指南（官方文件）

- 涵蓋概念：KNN、SVM、Decision Tree、Clustering（K-Means 等）、Regression、Dimensionality Reduction
- 網址：https://scikit-learn.org/stable/user_guide.html
- 取得方式：線上瀏覽（HTML 格式），或下載 PDF 版本
  - PDF 下載：https://scikit-learn.org/stable/_downloads/scikit-learn-docs.pdf
- 存放位置：`raw/official-docs/scikit-learn-user-guide.pdf`
- 授權：BSD 授權，免費公開

---

## 四、考古題（資訊管理研究所為主要來源）

> 本專案以各校資訊管理研究所（資管所）的計算機概論考古題為主要收錄對象。
> 資訊工程研究所（資工所）的題目若有參考價值，另置於跨所補充題區，不可混入資管所歷屆考題。
> 若確認找不到特定系所、年度或科目的考古題，應如實標記「未找到」，不以其他系所題目代替。

---

### 中正大學（CCU）資管所

考古題主要來源：資管系系所官方網站（mis.ccu.edu.tw）

**資訊管理學系碩士班（資管所）**

- 組別設定：
  - 甲組（管理組）：計算機概論 + 管理資訊系統（MIS）
  - 乙組（科技組）：計算機概論 + 資料結構
- 官方考古題頁面導航：
  - 進入 https://mis.ccu.edu.tw
  - 點選「招生資訊 → 考古題」
  - 直接連結（依官網 HTML 取得，請先確認可用性）：
    https://mis.ccu.edu.tw/p/412-1120-3339.php?Lang=zh-tw
- 收錄年度：約民國 104 學年度（2015 年）起至近年
- PDF 下載：有提供（依年度、科目分類）
- 官方解答：無

下載後存放：
- `raw/exams/ccu/`（依年度命名，例如：`ccu-mis-114-cs.pdf`）

**核對說明（2026-11-23 查詢）**
- 以 mis.ccu.edu.tw 官網 HTML 原始碼確認考古題連結路徑存在
- 原先記錄的 URL（/p/412-1120-22125.php）回傳 404，已改為由官網導航取得的正確路徑
- 若連結再次失效，請從 https://mis.ccu.edu.tw 首頁重新導航至「招生資訊 → 考古題」

---

### 中山大學（NSYSU）資管所

考古題來源：圖書與資訊處（圖書館）集中管理

**資訊管理學系碩士班（資管所）**

- 組別設定：
  - 甲組（資訊管理組）：計算機概論 + 管理資訊系統（MIS）
  - 乙組（商業智慧組）：計算機概論 + 資料結構
  - 備注：民國 108 學年度起英文成績不列入總分
- 官方考古題頁面：
  - 圖資處首頁：https://lis.nsysu.edu.tw
  - 導航路徑：館藏服務 → 教材與考古題 → 中山入學考古題
  - 直接連結（依官網 HTML 取得，請先確認可用性）：
    https://lis.nsysu.edu.tw/p/412-1001-2949.php
- 收錄年度：約民國 87 學年度（1998 年）起至近年
- PDF 下載：有提供（依學制、系所、學年度分類）
- 官方解答：無

下載後注意：
- 下載前務必確認系所欄位為「資訊管理學系」而非「資訊工程學系」
- 下載後存放：`raw/exams/nsysu/`（依年度命名，例如：`nsysu-mis-114-cs.pdf`）

**核對說明（2026-11-23 查詢）**
- 以 lis.nsysu.edu.tw 官網 HTML 原始碼確認「教材與考古題 → 中山入學考古題」連結存在
- 若直接連結失效，請從 https://lis.nsysu.edu.tw 首頁重新導航

---

### 雲林科技大學（YunTech）資管所

考古題來源：圖書館集中管理

**資訊管理研究所**

- 組別設定：近年不分組，計算機概論為唯一筆試科目（佔 70%）
  - 早期舊制曾設甲組、乙組、丙組，科目名稱依年份不同（資訊管理、資料結構、統計學等）
- 官方考古題頁面：
  - 圖書館首頁：https://www.lib.yuntech.edu.tw
  - 導航路徑：資源查詢 → 電子資源 → 考古題
  - 直接連結（依官網 HTML 取得，請先確認可用性）：
    https://www.lib.yuntech.edu.tw/lp?ctNode=4371&mp=1
- 收錄年度：民國 83 學年度（1994 年）起至近年（三校中收錄最早）
- PDF 下載：有提供（分學院、系所瀏覽）
- 官方解答：無

下載後注意：
- 進入後需選擇「管理學院 → 資訊管理研究所」
- 注意科目名稱版本較多，分別以「計算機概論」「計算機概論（2）」「計算機概論（3）」查詢
- 若上述版本名稱找不到，核對是否為「資訊管理」或其他科目名稱
- 若確認某年度確無資管所考古題，標記「未找到」，不以資工所題目代替
- 下載後存放：`raw/exams/yuntech/`（依年度命名，例如：`yuntech-mis-114-cs.pdf`）

**核對說明（2026-11-23 查詢）**
- 以 lib.yuntech.edu.tw 官網 HTML 原始碼確認「電子資源 → 考古題」連結為 ctNode=4371
- 雲科大考古題為分類瀏覽形式（非單一直連 URL），需進入後選擇管理學院對應系所

---

### 跨所補充題（選用）

以下系所題目若有參考學習價值，可另外存放於 `raw/exams/[學校]/cs-supplementary/`，但不可混入資管所正式歷屆考題，也不可建立 authentic/ 考題頁面時誤標為資管所題目。

| 學校 | 系所 | 說明 |
|------|------|------|
| 中正大學 | 資訊工程研究所 | 導航：https://cs.ccu.edu.tw → 招生資訊 → 歷屆考題 |
| 中山大學 | 資訊工程研究所 | 透過 lis.nsysu.edu.tw 圖書館系統查詢，科目包含作業系統與資料結構（合考） |
| 雲林科技大學 | 資訊工程研究所 | 透過 lib.yuntech.edu.tw 圖書館查詢，選擇工程學院對應系所 |

---

## 五、取得困難的資料處理方式

| 情況 | 處理方式 |
|------|---------|
| 需要機構帳號的論文 | 尋找 arXiv 或作者個人頁面的免費版本 |
| 連結失效 | 嘗試 Google Scholar、語義學者（semanticscholar.org）或 Unpaywall |
| 完全無法取得 PDF | 在 source 頁面的 ingest-note 記錄原因，改用可取得的替代來源，不可用摘要代替完整資料 |
| 考古題頁面改版 | 記錄查詢日期和錯誤，從各校官網首頁重新導航 |

---

最後更新：2026-11-23
