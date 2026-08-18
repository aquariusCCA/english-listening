# 聽力訓練單字 → Anki 卡片工作流程

本專案用途：從英文聽力逐字稿中整理中難度字彙，製作成可直接匯入 Anki 的 CSV 檔。

使用者以繁體中文溝通，回覆一律使用繁體中文。

## 狀態管理：一切以 repo 檔案為準

- **不要使用機器本地的長期記憶（memory 目錄）。** 本 repo 會推上 GitHub 並在多台電腦之間同步，寫在單一機器 memory 的內容只有那台電腦看得到，而且失效時毫無徵兆。需要跨 session 記住的偏好與決策，一律寫進本檔（CLAUDE.md）。
- **製卡紀錄以 repo 檔案為準**：CSV 檔案本身就是完整紀錄，README 目錄是索引。階段一挑字前應先查既有 CSV，避免重複出卡。
- **複習排程與記憶狀態歸 Anki／AnkiWeb 管，不進 repo**，也不要試圖在 repo 裡追蹤。使用者以手機 AnkiDroid 複習，進度由 AnkiWeb 在裝置間同步。未來若需分析弱點字，再透過桌面版 Anki 的 AnkiConnect 外掛查詢複習統計，屆時才建對應機制。

## Git 工作流程

- 分支使用 `main`。
- **開工先 `git pull`**：讀任何檔案之前先拉最新狀態。多電腦使用下，本機落後時會依過期內容運作且無從自覺。若 pull 前工作區有未 commit 的變更（上次 session 中斷的殘留），先補一個收尾 commit 再 pull——Git 會拒絕在可能被覆蓋的未提交變更上執行 pull。
- **收尾 `git commit` + `git push`**：未 push 的 commit 對其他電腦等於不存在。使用者已授權收尾時主動 commit + push，無需逐次確認。
- 尚未設定 remote 時只 commit，並提醒使用者設定 remote。
- Commit message 一律用繁體中文，寫明「改了什麼、為什麼」。

## 整體流程（兩階段）

### 階段一：提取中難度字彙

使用者提供聽力的「主題名稱」與「逐字稿內容」後：

1. 從逐字稿挑出**中難度**字彙（適合聽力與字彙進階學習的程度，約 CEFR B1–C1；排除過於基礎或過於冷僻的字）。
2. 不限單一單字，以下都應列入候選：
   - 多字詞條與片語（如 `catch of the day`、`in advance`、片語動詞 `heat up`）
   - 常見搭配（collocation）
   - **熟字生義**（常見字的非常用義，如 `light` 清淡的、`dressing` 沙拉醬）——這類是聽力難點，優先收錄
3. 以表格呈現，欄位如下：

| 欄位 | 說明 |
|---|---|
| `#` | 編號（使用者之後以編號挑選） |
| Word | 單字或片語 |
| POS | 詞性 |
| IPA | 美式 IPA（KK 式） |
| Chinese Meaning | 繁體中文釋義 |
| Context Sentence | 逐字稿中的原句（太長可節錄關鍵子句） |
| Note | 熟字生義、易混淆發音、常見搭配等補充（可留空） |

4. 呈現表格後**停下等待**，讓使用者回覆要收錄的編號（例如「2, 5, 9」）。

### 階段二：產出 Anki CSV

使用者挑選完畢後：

1. 將選定的詞條輸出成 CSV 檔（格式見下方「CSV 格式規格」）。
2. **同步更新 `README.md` 的目錄表格**，為這份 CSV 新增一列。此步驟不可省略，否則目錄會失效。
3. 若使用者尚未提供該支影片的標題與連結，主動向他索取，不要留白或自行臆測。

## CSV 格式規格

- 檔名：`YYYY-MM-DD-主題英文名.csv`（日期用當天日期，如 `2026-08-13-restaurant-english.csv`）
- 編碼：UTF-8
- 檔案開頭兩行固定為：

  ```
  #separator:Comma
  #html:true
  ```

- 每列兩個欄位（皆以雙引號包住，內部的 `"` 以 `""` 跳脫）：
  - **正面**：`單字 (詞性縮寫)`，如 `"reservation (n.)"`；片語無詞性可省略，如 `"catch of the day"`
  - **背面**：HTML 內容，見下方模板

## 卡片背面 HTML 模板

結構：中文釋義（大字）→ IPA 音標（小字灰色）→ 標籤例句區。

```html
<div style="font-size:1.3em;font-weight:700;line-height:1.4;margin:2px 0 4px;">中文釋義</div><div style="font-size:.85em;color:#9ca3af;margin:0 0 14px;">/ˌrɛzɚˈveʃən/</div><div style="display:inline-block;text-align:left;line-height:1.55;max-width:95%;">（標籤列…）</div>
```

標籤列共四種，每列格式：

```html
<div style="margin:7px 0;"><span style="display:inline-block;font-size:.72em;font-weight:700;padding:1px 7px;border-radius:9px;margin-right:7px;background:{BG};color:{FG};">{標籤}</span>句子，關鍵字用 <b style="color:{FG};">粗體同色</b></div>
```

| 標籤 | 用途 | FG 色 | BG 色 | 必要性 |
|---|---|---|---|---|
| 例 | 由 Claude 另造的通用例句 | `#3b82f6` | `rgba(59,130,246,.16)` | 必有 |
| 影片 | 逐字稿原句（節錄亦可） | `#10b981` | `rgba(16,185,129,.16)` | 必有 |
| 補充 | 搭配詞、衍生字、相關用法 | `#9ca3af` | `rgba(156,163,175,.20)` | 視情況 |
| 注意 | 熟字生義、易混淆發音等提醒 | `#f59e0b` | `rgba(245,158,11,.16)` | 視情況 |

注意事項：

- 「例」與「影片」列中的目標字都要以 `<b style="color:…">` 加粗上色（顏色同該列 FG 色）。
- 「補充」「注意」列可為純中文說明，不一定含例句。
- 寫入 CSV 時，模板中所有 `"` 都要改成 `""`。
- 既有檔案 `2026-08-13-restaurant-english.csv` 是格式參考範例（該檔製作於加入音標規則之前，故無 IPA 列；新檔一律要有）。

## 目錄頁

所有 CSV 與其來源聽力影片的對照表記錄在 `README.md`，**不另開 `INDEX.md`**。原因是本專案結構單純（一份規格文件加一批 CSV），多一個檔案就多一處要同步維護；而 README 是專案入口，推上 GitHub 後也會直接顯示在首頁，最容易被看到。

表格欄位：日期、主題、影片標題、連結、CSV 檔案、卡片數。

- 日期與 CSV 檔名開頭的日期一致。
- 影片標題與連結由使用者提供，不可自行推測。
- CSV 檔案欄位使用 Markdown 相對連結，例如 `[2026-08-13-restaurant-english.csv](2026-08-13-restaurant-english.csv)`。
- 卡片數為 CSV 中的字卡列數，即總行數扣掉開頭兩行標頭。可用 `grep -c '^"' 檔名.csv` 取得。

何時改為獨立檔案：當目錄累積到約 40–50 筆以上，或需要增加分類、學習進度等欄位而使表格過寬時，再將目錄搬到獨立的 `INDEX.md`，並於 README 保留連結指向它。在那之前維持現狀。
