# 安裝與串接指南（gws + 行銷 MCP + 成效表）

這個引擎「發想切角、寫腳本」的部分**裝好 plugin 就能用**。但如果你要用到
**自動追蹤 CPI / 成效**（讀寫 Google Sheet、抓 Meta/Google 廣告數據），需要先
把下面「三隻腳」準備好。三隻腳缺一，自動追蹤就跑不起來（發想／腳本不受影響）。

> 對象：多為無技術背景的行銷／產品人。照著做即可；有 ⚠️ 的步驟需要本人或工程師出手。

---

## 三隻腳總覽

| 腳 | 做什麼 | 誰能裝 |
|----|--------|--------|
| ① **gws CLI** | 讓 AI 能讀寫你的 Google Sheet | 你自己（含一次 Google 授權）|
| ② **行銷 MCP**（`cm-mcp-marketing`）| 讓 AI 能抓 Meta／Google 廣告成效 | ⚠️ 需 xlab 團隊／工程師協助 |
| ③ **成效追蹤表** | 存放各廣告的 CPI 與狀態 | AI 幫你建（說「建立成效追蹤表」）|

---

## ① 安裝 gws CLI 並授權

`gws` 是 **Google Workspace CLI**（Google 官方開源工具），讓引擎能代你讀寫
Google Sheet。
- 官方 repo：<https://github.com/googleworkspace/cli>

**安裝（擇一，macOS 建議用 Homebrew）：**

```bash
# 方式 A：Homebrew（macOS / Linux）— 最簡單
brew install googleworkspace-cli

# 方式 B：下載官方預編譯執行檔（解壓後放進 PATH）
#   https://github.com/googleworkspace/cli/releases

# 方式 C：npm
npm install -g @googleworkspace/cli
```

**授權（本人在瀏覽器同意一次，AI 無法代做——帳號安全關卡）：**

```bash
# 有裝 gcloud CLI：首次用 setup（會建 GCP 專案、開 API、登入）
gws auth setup
#   之後要再登入時用：
gws auth login
```
- 沒有 gcloud 的話：到 Google Cloud Console 建 OAuth 憑證，存到
  `~/.config/gws/client_secret.json`，再跑 `gws auth login`。
- 授權範圍＝你在 Google Sheet 看得到的範圍。

**確認成功：**
```bash
gws sheets --help
```
有印出說明就代表裝好了。

---

## ② 接上行銷 MCP（`cm-mcp-marketing`）

這個 MCP 讓引擎能抓 Meta／Google 廣告的花費與安裝數（算 CPI 用）。

- 內部 repo（CMoney GitLab）：<http://gitlab.cmoney.tw/xlab/cm-mcp-marketing>
- ⚠️ 這是 **CMoney 內部的遠端 MCP 服務，且部署時綁定特定廣告帳戶**——不是公開
  下載就能用。請**依該 repo 的 README 部署／設定**，把它接進你的 Claude Code
  MCP 設定；遇到權限、連線或帳戶綁定問題，**聯繫 xlab 團隊／工程師**。
- 接好後，Claude Code 裡會出現 `cm-marketing-remote` 相關工具（如
  `get_ad_insights`、`gads_get_ad_insights`）。

> 只想用「發想切角／寫腳本」的話，這隻腳可以先跳過。

---

## ③ 建立你自己的成效追蹤表

裝好 ① 之後，直接跟引擎說「**建立成效追蹤表**」，它會幫你開一張乾淨的表：

- **表A「主控台」**：一支廣告一列，顯示最新 CPI 與燈號（就地更新）。
- **表B「CPI 紀錄」**：流水帳，每次更新往下加一批（只增不改，可看趨勢）。

⚠️ 這張表是**你自己的**（存在你的 Google Drive）。引擎只會把「你這張表」的 ID
記在你本機的產品記憶檔，**不會外流、也不會寫進這個 plugin**。

### 表的關鍵設計（引擎自動遵守）
- **認欄位「標題」不認欄位字母**：你可以自由刪／加／搬欄，只要別改標題名稱。
- **「我的決策」欄留給你手填**（保留／加碼／關閉），引擎永遠不碰。
- **動錢的事只標記、不自動關廣告**——超標廣告由你自己決定關不關。

---

## 完成檢查清單

- [ ] `gws sheets --help` 有正常輸出（① 完成）
- [ ] Claude Code 裡看得到 `cm-marketing-remote` 的工具（② 完成，需 xlab 協助）
- [ ] 說「建立成效追蹤表」後，Google Drive 出現一張含兩分頁的新表（③ 完成）

三項都打勾後，就能對引擎說「**更新 CPI**」，它會自動抓數據、算 CPI、判燈號、
先給你預覽、確認後才寫回你的表。

---

## 常見問題

- **只裝了 plugin，喊「更新 CPI」沒反應？** → 多半是 ① 或 ② 還沒好。先跑
  `gws sheets --help` 確認 gws；再確認已依 ② 接上行銷 MCP。
- **iOS 的安裝數看起來是 0？** → 這是 Apple 隱私限制（ATT/SKAN）造成的正常
  低估，不是壞掉；引擎會把安裝太少的先標「觀察中」，不急著判生死。
- **不想追成效，只想發想切角？** → 完全不用裝 gws／MCP，直接用「發想切角」即可。
