# plugin-ad-ccn-engine

通用短影音投放素材切角＆腳本引擎。適用任何 App／產品：貼上產品資訊，就用 CCN 框架自動設定 4 種 TA、產生專屬切角庫、給出投廣策略；並用各產品的 `matrix-state.md` 當記憶，避免重複生成。**進階還能自動追蹤每支廣告的 CPI 成效**（需前置設定，見下）。

這是一個「方法引擎」，本身不綁任何產品——你把產品資料貼進來，它就幫你長出那支產品專屬的 TA、切角、策略與記憶。

## 安裝

```
/plugin install ad-ccn-engine@cm-ailab-cc-plugins
/reload-plugins
```

## 功能總覽

本引擎分兩大類功能：

- **A. 發想／腳本** — 裝好 plugin **即可用**，不需任何額外設定。
- **B. 成效追蹤** — 自動抓 Meta／Google 廣告數據、算 CPI、寫回 Google Sheet。
  **需要先完成前置設定**（安裝 gws CLI＋接行銷 MCP＋建立追蹤表），
  📄 **設定步驟見：[`skills/ad-ccn-engine/references/setup-gws-mcp.md`](skills/ad-ccn-engine/references/setup-gws-mcp.md)**

## 使用方式（指令一覽）

安裝後，直接對 Claude 說下列指令（會自動觸發本 skill）。

### A. 發想／腳本（裝好即可用）

| 指令 | 作用 |
|------|------|
| 新增產品（＋貼上產品資訊） | 一條龍：整理產品檔 → 設 CPI 目標 → 用 CCN 推 4 個 TA → 生切角庫 → 給投廣策略，最後建立該產品工作區 |
| 切換到 XX 產品 / 換產品 | 讀該產品的資料與記憶，之後動作都對這支 |
| 發想切角 | 讀記憶避免重複，選新切角，對 4 個 TA 各生一支 10 秒短影音腳本 |
| 存進 matrix | 把發想結果寫進該產品記憶檔 |
| 分析這一輪 | 讀全部數據，回報最佳 TA／平台，推薦下一輪測什麼 |

### B. 成效追蹤（需先完成 [前置設定](skills/ad-ccn-engine/references/setup-gws-mcp.md)）

| 指令 | 作用 | 需要 |
|------|------|------|
| 建立成效追蹤表 / 建表 | 從零建一張乾淨的追蹤表（表A 主控台 + 表B CPI 紀錄）| gws |
| 採用這批素材 / 加進表A | 把這輪確定要投的廣告 append 進主控台（廣告ID 先留空）| gws |
| 補廣告ID / 對應廣告 | 上架後，用廣告命名經 MCP 比對回填廣告ID（會先給你預覽確認）| gws＋MCP |
| 更新 CPI | 自動抓今日＋近7天數據、算 CPI、判燈號、先預覽 → 寫回表（主控台就地更新＋紀錄頁 append）| gws＋MCP |

> 燈號依「近7天平均 CPI」判定：🟡 觀察中（安裝<10）／🟢 保留（<目標）／🔴 超標（≥目標）。
> 引擎只標記、**不自動關廣告**；「我的決策」欄留給你手填，引擎永不碰。

## 檔案結構

- `skills/ad-ccn-engine/SKILL.md` — 主流程與指令對應
- `skills/ad-ccn-engine/ccn-framework.md` — CCN 方法論與通用切角原型庫
- `skills/ad-ccn-engine/references/setup-gws-mcp.md` — **成效追蹤的安裝與串接教學（gws＋MCP＋建表）**
- `skills/ad-ccn-engine/templates/` — 產品檔與記憶檔的空白範本

各產品的資料與記憶存在使用者桌面 `切角引擎/<產品名>/`，與引擎本體分離、互不干擾。

## 作者

daisywang-cmoneywork
