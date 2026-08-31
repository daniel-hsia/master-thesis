# Methodology

## 1. Research Objective

本研究探討 **非對稱九子棋（Asymmetric Nine Men's Morris）** 中，
棋子數量差、AI 強度與遊戲機制對勝率與遊戲平衡性的影響。

研究主要目標包括：

- 建立不同 AI 強度與讓子條件的實驗配置
- 分析非對稱局面下的勝率與和局率變化
- 設計額外遊戲機制，使讓子系統具有更細緻的勝率調整能力
- 建立讓子勝率矩陣
- 依據勝率資料設計玩家能力分級與關卡
- 建立可用於玩家評分與公平配對的分析框架
- 分析部分極端非對稱局面的理論邊界條件

---

## 2. Base System

本研究基於既有開源 **Nine Men's Morris** 遊戲與 AI 程式進行修改與擴充。

原始遊戲引擎與 AI 實作並非由本人從零開發。

本人主要針對研究需求閱讀並修改既有 C++ codebase，
新增或擴充：

- AI 強度相關參數設定
- 非對稱棋子數量配置
- Mixed Preemptive Rule
- 自動化 AI 對戰流程
- 實驗結果紀錄
- 對戰資料蒐集與後續分析流程

主要使用語言為 **C++**。

---

## 3. Experimental Variables

實驗主要控制三類變數：

### 3.1 AI Strength

透過不同 AI 參數設定建立不同強度的 AI，
並以不同強度 AI 間的自動化對戰結果，
分析其相對實力差異。

AI 強度同時作為後續玩家能力分級與關卡設計的重要參考變數。

### 3.2 Piece Handicap

透過調整雙方初始棋子數量，
建立不同程度的非對稱遊戲配置。

此變數代表玩家與 AI 之間的 **Spatial Handicap**，
即由棋子數量差造成的空間資源優勢或劣勢。

### 3.3 Game Mechanism

研究主要比較兩種規則：

#### Synchronous Rule

雙方依照傳統規則進行放置，
並在完成相應放置流程後進入後續移動階段。

此機制主要呈現由棋子數量差本身造成的遊戲平衡變化。

#### Mixed Preemptive Rule

Mixed Preemptive Rule 為本研究提出的非對稱遊戲機制。

若少子方率先完成其棋子放置，
即可提前進入 Movement Phase 或 Flying Phase，
不需等待多子方完成所有棋子的放置。

此機制引入額外的 **Temporal Advantage**，
用以補償少子方的 Spatial Handicap。

---

## 4. Mixed Preemptive Rule

Mixed Preemptive Rule 的主要設計目的，
是解決單純以棋子數量進行讓子時，
不同設定之間勝率變化可能過於離散的問題。

若只有棋子數量差作為調整參數，
遊戲平衡往往只能以較大的離散幅度變動。

因此，本研究在 Spatial Handicap 之外，
再加入 Temporal Advantage 作為額外調整維度。

在 Mixed Preemptive Rule 中：

1. 少子方率先完成放置後，即可提前進入下一階段。
2. 多子方仍可能處於 Placement Phase。
3. 少子方可利用提前取得的移動能力進行防守、阻擋或主動進攻。
4. 在特定棋子數量下，少子方甚至可提前進入 Flying Phase，取得高度移動自由。

因此，遊戲平衡可視為由以下三個主要因素共同決定：

- **Spatial Handicap**：棋子數量差
- **Temporal Advantage**：提前移動所帶來的行動優勢
- **AI Strength**：不同 AI 強度造成的決策能力差異

此機制將空間資源差與時間優勢分離，
使非對稱讓子能建立更細緻的勝率梯度，
並提供更多可供玩家配對與難度控制的調整組合。

---

## 5. Automated Self-Play

針對不同 AI 強度、棋子數量與遊戲機制的組合，
本研究執行自動化 AI self-play 實驗。

**每組實驗固定執行 1,000 場對局**，
並記錄：

- P1 勝場數
- P2 勝場數
- 和局數
- Win Rate
- Draw Rate
- Expected Score

若以玩家一方為評估對象，
Expected Score 定義為：

\[
E = WinRate + 0.5 \times DrawRate
\]

此定義將勝利視為 1 分、和局視為 0.5 分、失敗視為 0 分，
用於衡量特定遊戲配置下的整體預期表現。

透過固定每組 1,000 場對戰，
使不同 AI 強度、棋子數量與規則配置之間的結果具有一致的比較基準。

實驗結果由自動化對戰系統產生，
再整理至 Excel 與 LaTeX 中進行後續分析與展示。

---

## 6. Win-Rate Matrix

根據不同 AI 強度、棋子數量與遊戲機制下的對戰結果，
本研究建立 **Handicap Win-Rate Matrix**。

矩陣主要用於描述：

- 不同 AI 強度之間的實力差
- 不同棋子數量讓子所造成的影響
- Mixed Preemptive Rule 對遊戲平衡的修正效果
- 不同配置下玩家的預期勝率與 Expected Score

此矩陣進一步作為：

- 玩家能力推估
- 難度分級
- 關卡設計
- 公平配對

的基礎。

矩陣的核心用途並非單純呈現勝率，
而是將大量模擬結果轉化為可供後續系統決策使用的結構化資料。

---

## 7. Player Rating and Tier Design

本研究進一步根據實驗勝率與 Expected Score，
設計不同層級的玩家能力評估關卡。

關卡不單純依照 AI 強度排序，
而是同時考量：

- AI 強度
- 玩家與 AI 的棋子數量差
- 遊戲機制
- Win Rate
- Draw Rate
- Expected Score

因此，同一 AI 強度可以透過不同棋子數量與規則設定，
形成不同難度的評估環境。

部分代表性關卡如下：

| Tier | Target Skill | Representative Setting | Experimental Result |
|---|---:|---|---|
| Bronze | 2 | Player 7 vs AI 1, Sync | Win Rate 56.7% |
| Silver | 3 | Player 8 vs AI 1, Sync | Win 21.6%, Draw 78.4%, E = 60.8% |
| Gold | 4 | Player 7 vs AI 2, Sync | Win 17.2%, E = 50.6% |
| Platinum | 5 | Player 7 vs AI 2, Mixed | Win 4.4%, Draw 89.1%, E = 48.95% |
| Diamond | 6 | Player 8 vs AI 5, Mixed | Win 22.8%, Draw 55.7%, E = 50.65% |

其中部分關卡以 **Expected Score 接近 50%**
作為相對平衡的能力評估條件。

這使得關卡系統不只是難度遞增，
也能反映不同類型的策略能力，例如：

- 防守能力
- 空間規劃
- 節奏利用
- 長期策略
- 高壓局面下的計算與風險控制

---

## 8. Theoretical Analysis

除了自動化模擬外，
本研究亦針對部分極端非對稱局面進行數學分析，
以建立實驗結果的理論邊界。

### 8.1 3-vs-4 under Synchronous Placement

考慮：

- P1 持有 3 枚棋子
- P2 持有 4 枚棋子

在傳統同步放置規則下，
可證明：

> 若 P2 採取最佳策略，4-piece side P2 可以強制取得勝利。

證明核心如下：

1. P2 可優先選擇具有較高連接度的位置建立進攻結構。
2. 當 P1 嘗試形成 mill 時，P2 可直接進行阻擋。
3. P2 可利用剩餘棋子建立新的成線威脅。
4. P1 在同步放置規則下缺乏提前進入 Movement 或 Flying Phase 的能力。
5. P1 必須被動回應 P2 的威脅，無法建立足夠的反制結構。
6. P2 最終可強制形成 mill，移除 P1 一枚棋子。
7. P1 因棋子數降至無法繼續遊戲的狀態而失敗。

此結果提供 3-vs-4 非對稱局面在傳統規則下的理論邊界。

---

## 9. Effect of Mixed Preemptive Rule on the 3-vs-4 Boundary

採用 Mixed Preemptive Rule 後，
3-piece side 在完成第三枚棋子的放置時，
即可提前進入 Flying Phase。

此時：

- 4-piece side 尚未完成所有棋子的放置
- 3-piece side 已取得高度移動自由
- 少子方可快速移動至任意合法位置
- 可主動阻擋對手形成 mill
- 亦可能利用飛行能力形成自身的 mill

因此，
同步規則下建立的 4-piece side 必勝策略，
無法直接套用到 Mixed Preemptive Rule。

此結果說明：

> **Temporal Advantage 能在部分極端非對稱局面中補償 Spatial Handicap。**

這也是本研究設計 Mixed Preemptive Rule 的核心理論動機之一。

---

## 10. Research Workflow

整體研究流程如下：

1. 閱讀並理解既有開源 C++ 九子棋程式碼
2. 修改系統以支援非對稱棋子配置
3. 建立 AI 強度相關參數設定
4. 設計並實作 Mixed Preemptive Rule
5. 建立自動化 AI self-play 實驗流程
6. 執行不同 AI、棋子數量與遊戲機制組合
7. 蒐集勝負、和局與 Expected Score 資料
8. 建立 Handicap Win-Rate Matrix
9. 分析不同配置下的遊戲平衡
10. 設計玩家能力 Tier 與評估關卡
11. 將實驗結果應用於玩家評分與公平配對
12. 進行部分極端非對稱局面的數學邊界分析

---

## 11. Research Output

本研究最終產出包括：

- Mixed Preemptive Rule
- 修改後的 C++ 實驗平台
- 自動化 AI self-play 流程
- 不同 AI 強度與讓子配置的實驗資料
- Handicap Win-Rate Matrix
- 玩家能力分級與關卡設計
- 玩家評分與配對分析框架
- 3-vs-4 等非對稱局面的理論邊界分析

---

## 12. Data and Result Files

本 repository 中相關資料包括：

- `../results/raw-match-results.xlsx`  
  原始／整理後的各組對戰統計資料

- `../results/summarized-results.xlsx`  
  經整理後的實驗結果

- `../results/summarized-analysis.tex`  
  論文中使用的 LaTeX 分析內容

- `../figures/win-rate-matrix.png`  
  讓子勝率矩陣

- `../figures/automated-self-play.png`  
  自動化 AI 對戰執行示意
