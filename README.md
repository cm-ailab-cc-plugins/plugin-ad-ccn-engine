# plugin-ad-ccn-engine

通用短影音投放素材切角＆腳本引擎。適用任何 App／產品：貼上產品資訊，就用 CCN 框架自動設定 4 種 TA、產生專屬切角庫、給出投廣策略；並用各產品的 `matrix-state.md` 當記憶，避免重複生成。

這是一個「方法引擎」，本身不綁任何產品——你把產品資料貼進來，它就幫你長出那支產品專屬的 TA、切角、策略與記憶。

## 安裝

```
/plugin install ad-ccn-engine@cm-ailab-cc-plugins
/reload-plugins
```

## 使用方式

安裝後，直接對 Claude 說下列指令（會自動觸發本 skill）：

| 指令 | 作用 |
|------|------|
| 新增產品（＋貼上產品資訊） | 一條龍：整理產品檔 → 設 CPI 目標 → 用 CCN 推 4 個 TA → 生切角庫 → 給投廣策略，最後建立該產品工作區 |
| 切換到 XX 產品 / 換產品 | 讀該產品的資料與記憶，之後動作都對這支 |
| 發想切角 | 讀記憶避免重複，選新切角，對 4 個 TA 各生一支 10 秒短影音腳本 |
| 存進 matrix | 把發想結果寫進該產品記憶檔，並輸出可貼 Google Sheet 的表格 |
| 更新命名（＋廣告命名） | 依平台矩陣建立廣告列，狀態標「跑中」 |
| 更新 CPI（＋數字） | 寫入 CPI，自動判紅綠燈（低於目標保留、達標淘汰） |
| 分析這一輪 | 讀全部數據，回報最佳 TA／平台，推薦下一輪測什麼 |

## 檔案結構

- `skills/ad-ccn-engine/SKILL.md` — 主流程與指令對應
- `skills/ad-ccn-engine/ccn-framework.md` — CCN 方法論與通用切角原型庫
- `skills/ad-ccn-engine/templates/` — 產品檔與記憶檔的空白範本

各產品的資料與記憶存在使用者桌面 `切角引擎/<產品名>/`，與引擎本體分離、互不干擾。

## 作者

daisywang-cmoneywork
