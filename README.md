# 英文聽力單字卡

從英文聽力影片的逐字稿中整理中難度字彙，製作成可匯入 Anki 的字卡。

製作流程與 CSV 格式規格記錄於 [CLAUDE.md](CLAUDE.md)。

## 目錄

| 日期 | 主題 | 影片標題 | 連結 | CSV 檔案 | 卡片數 |
|---|---|---|---|---|---:|
| 2026-08-18 | 餐廳英文 | 🎧保母級聽力訓練｜輕鬆掌握餐廳對話，提升英語自信｜餐廳英文｜進步神速的英文訓練方法｜零基礎學英文｜輕鬆學英文｜一小時聽英文｜English Listening｜One Hour English | [YouTube](https://www.youtube.com/watch?v=muGybHP7eVs) | [2026-08-18-restaurant-english.csv](2026-08-18-restaurant-english.csv) | 88 |

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
