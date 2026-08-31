# Methodology

## 1. Research Objective

本研究探討非對稱九子棋中，
棋子數量差、AI 強度與遊戲機制對勝率與遊戲平衡性的影響。

研究目標包括：

- 建立不同 AI 強度與讓子條件的實驗配置
- 分析非對稱局面下的勝率變化
- 設計 Mixed Preemptive Rule 作為額外平衡參數
- 建立讓子勝率矩陣
- 依據勝率資料設計玩家能力分級與關卡
- 分析部分極端局面的理論邊界條件

---

## 2. Base System

本研究基於既有開源 Nine Men's Morris 遊戲與 AI 程式進行修改。

原始遊戲引擎與 AI 並非由本人從零開發。

本人主要修改與新增：

- AI 參數化設定
- 非對稱棋子數量配置
- Mixed Preemptive Rule
- 自動化 AI 對戰流程
- 實驗結果輸出與資料蒐集功能

主要使用語言為 C++。

---

## 3. Experimental Variables

實驗主要控制以下變數：

### AI Strength
透過不同 AI 參數設定建立不同強度的 AI。

### Piece Handicap
調整雙方初始棋子數量，建立不同程度的非對稱局面。

### Game Mechanism

使用兩種主要規則：

- **Synchronous Rule**：雙方完成放置階段後再進入移動階段
- **Mixed Preemptive Rule**：少子方完成放置後可提前進入移動／飛行階段

---

## 4. Mixed Preemptive Rule

Mixed Preemptive Rule 的設計目的，
是讓非對稱讓子不只由棋子數量差決定。

在少子方率先完成放置後，
允許其提前進入移動或飛行階段。

因此，遊戲平衡同時受到：

- Spatial Handicap
- Temporal Advantage
- AI Strength

三種因素影響。

此機制可提供比單純棋子數量差更細緻的勝率調整能力。

---

## 5. Automated Self-Play

針對不同 AI 強度、棋子數量與遊戲機制組合，
執行大量自動化 AI 對戰。

每組實驗記錄：

- P1 勝場
- P2 勝場
- 和局數
- 勝率
- 和局率
- Expected Score

實驗結果輸出後，
再進一步整理成 Excel 與 LaTeX 表格進行分析。

---

## 6. Win-Rate Matrix

根據不同 AI 強度與讓子條件下的對戰結果，
建立 Handicap Win-Rate Matrix。

矩陣用於描述不同遊戲配置之間的平衡程度，
並作為玩家能力推估與關卡設計的依據。

---

## 7. Player Tier Design

根據勝率與 Expected Score，
建立不同難度的玩家能力評估關卡。

關卡設計同時考量：

- AI 強度
- 棋子數量差
- 是否使用 Mixed Preemptive Rule
- 勝率
- 和局率
- Expected Score

部分關卡以 Expected Score 接近 50% 為平衡目標。

---

## 8. Theoretical Analysis

除了模擬結果外，
本研究亦針對部分極端非對稱局面進行數學分析。

例如在 3-vs-4 的同步放置規則下，
可證明 4-piece side 在最佳策略下必勝。

但在 Mixed Preemptive Rule 下，
3-piece side 可提前進入 Flying Phase，
使原本的必勝邊界不再直接成立。

此分析用來說明：
遊戲規則中的時間優勢能有效改變純粹由空間資源差所形成的理論邊界。
