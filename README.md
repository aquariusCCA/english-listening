# 英文聽力單字卡

從英文聽力影片的逐字稿中整理中難度字彙，製作成可匯入 Anki 的字卡。

核心規格記錄於 [CLAUDE.md](CLAUDE.md)，各動作規則與 CSV 格式規格在 [rules/](rules/)。

## 製卡流程

字卡由 [Claude Code](https://claude.com/claude-code) 依 [rules/](rules/) 的規則產出。在本資料夾開啟 Claude Code 後，用以下指令操作：

| 指令 | 用途 |
|---|---|
| `/pick` | 從逐字稿挑出中難度字彙並列表，停下等你回覆要收錄的編號 |
| `/cards` | 依你挑的編號產出 CSV、更新本頁目錄、歸檔逐字稿 |
| `/add` | 補卡：直接指名漏掉的字，追加到既有 CSV |
| `/recheck` | 補卡：重掃逐字稿，只列出尚未收錄的字讓你挑 |
| `/end` | 收尾：commit 並 push |

一般流程是給主題名稱與逐字稿後 `/pick` → 回覆編號（如 `2, 5, 9`）→ `/cards`。複習時發現漏字，再用 `/add` 或 `/recheck` 追加到原本那份 CSV，重新匯入 Anki 即可，複習進度不受影響（見下方「匯入 Anki 的方式」）。

指令不是必須的——直接把逐字稿貼進對話說要挑字，一樣會依規則走。

## 目錄

| 日期 | 主題 | 影片標題 | 連結 | CSV 檔案 | 卡片數 |
|---|---|---|---|---|---:|
| 2026-08-18 | 餐廳英文 | 🎧保母級聽力訓練｜輕鬆掌握餐廳對話，提升英語自信｜餐廳英文｜進步神速的英文訓練方法｜零基礎學英文｜輕鬆學英文｜一小時聽英文｜English Listening｜One Hour English | [YouTube](https://www.youtube.com/watch?v=muGybHP7eVs) | [2026-08-18-restaurant-english.csv](cards/2026-08-18-restaurant-english.csv) | 92 |

## 在 Obsidian 中閱讀

本 repo 的根目錄同時就是一個 Obsidian vault。Obsidian →「開啟資料夾作為儲存庫」→ 選本資料夾即可。

- `dashboard.base`：自動索引表，讀 `transcripts/` 各逐字稿的 frontmatter，新增影片後自動出現，不需手動維護。
- `transcripts/`：逐字稿，上方 Properties 面板顯示影片標題、連結、日期與主題。
- `cards/`：Anki CSV。Obsidian 開不了 CSV，這裡只是存放處，實際使用請匯入 Anki。

## 複習方式

複習進度由 Anki 管理，不記錄在本 repo。

1. 手機安裝 AnkiDroid（Android）或 AnkiMobile（iOS），電腦安裝桌面版 Anki。
2. 註冊免費的 [AnkiWeb](https://ankiweb.net) 帳號並在各裝置登入，複習進度即可自動同步。
3. 本 repo 只負責產出字卡內容；哪張卡什麼時候該複習，交由 Anki 的排程演算法處理。

## 匯入 Anki 的方式

1. Anki 選單列 →「檔案」→「匯入」，選擇要匯入的 CSV。
2. 檔案開頭的 `#separator:Comma` 與 `#html:true` 會讓 Anki 自動套用正確的分隔符號與 HTML 設定，通常不需手動調整。
3. 欄位對應：第一欄為正面（單字），第二欄為背面（釋義與例句）。
4. 選擇目標牌組與筆記類型（Basic 即可），確認後匯入。
