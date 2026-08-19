# 聽力訓練單字 → Anki 卡片工作流程

本專案用途：從英文聽力逐字稿中整理中難度字彙，製作成可直接匯入 Anki 的 CSV 檔。

使用者以繁體中文溝通，回覆一律使用繁體中文。

## 檔案結構

```
README.md                             目錄索引（專案入口，GitHub 首頁會直接顯示）
CLAUDE.md                             本規格文件
dashboard.base                        Obsidian 端的自動索引
.gitignore                            白名單制排除 .obsidian/ 易變檔
cards/YYYY-MM-DD-主題英文名.csv        Anki 字卡
transcripts/YYYY-MM-DD-主題英文名.md   對應的來源逐字稿
.obsidian/                            Obsidian vault 設定（部分納入版控，見 .gitignore）
```

逐字稿與其 CSV **同名**（僅副檔名不同），一眼即可對應。YouTube 原始檔名通常又長又含 emoji 與全形分隔符，在 Windows 上會觸發路徑長度問題、在 Git 輸出中也會被轉義成難讀的八進位碼，因此一律改名後才入庫；原始標題保留在逐字稿的 frontmatter `title` 欄位與 README 目錄中，資訊不會遺失。

**檔名一律使用 ASCII**（英數字與連字號），理由同上。這條也適用於 `dashboard.base` 這類新增檔案——所以它不叫「總覽.base」。檔案*內容*用中文不受此限。

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
3. **將逐字稿歸檔到 `transcripts/`**，改名為與 CSV 相同的檔名（副檔名 `.md`），並在檔首寫入 YAML frontmatter（四個欄位，格式見下方「Obsidian Vault」）。使用者若是把逐字稿貼在對話中而非給檔案，一樣要建檔存入——之後補卡查原句全靠它。歸檔後刪除原本散落在根目錄的檔案（`git mv` 即可一次完成搬移與改名）。
4. 若使用者尚未提供該支影片的標題與連結，主動向他索取，不要留白或自行臆測。

### 補卡：追加詞條到既有 CSV

使用者複習後常會發現「這個字我不會，逐字稿裡也有，但卡片沒收到」——可能是階段一表格沒列出（挑字門檻判斷），也可能是他挑選時漏回覆編號。此時**不重做整份 CSV，直接追加**：

1. 確認要追加到哪一份 CSV。字取自哪支影片的逐字稿，就進哪一份；同一支影片不另開新檔。
2. **追加前先掃該 CSV 的第一欄**，確認該詞條尚未存在，避免重複列。
3. 新卡一律**接在檔尾**，不重排既有列。既有卡列的內容不要順手改動——那會讓 diff 難以檢視。
4. 「影片」列仍須引用該支逐字稿的原句，回 `transcripts/` 底下的同名 `.md` 查即可——**這正是逐字稿要保留在 repo 的理由**。CSV 的「影片」列只存了已出卡詞條的原句，尚未出卡的字（也就是補卡要處理的那些）其上下文只存在於逐字稿裡，刪掉就得重新取得。若該支逐字稿不在 repo，向使用者索取，不要自行編造原句。
5. **同步更新 `README.md` 該列的卡片數**（重新以 `grep -c '^"' 檔名.csv` 取得），這步同樣不可省略。

兩種子情境的處理方式不同：

- **使用者直接指名要哪些字** → 不必再跑階段一表格，直接出卡追加。
- **使用者要你「再檢查有無遺漏」** → 重跑階段一，但**只列出該 CSV 尚未收錄的詞條**，照常編號後停下等他挑選。

追加後使用者只要在 Anki 重新匯入整份 CSV 即可：Anki 以第一欄（正面）比對，既有卡會被更新而非重複建立，**複習進度不受影響**；只需確認匯入畫面的「Existing notes」維持在「Update」。因此追加時不需要另外產出「只含新卡」的檔案。

## CSV 格式規格

- 路徑與檔名：`cards/YYYY-MM-DD-主題英文名.csv`（日期用當天日期，如 `cards/2026-08-20-airport-english.csv`）。CSV 一律放在 `cards/`，不放根目錄——Obsidian 開不了 CSV，散在根目錄會把 vault 的檔案總管洗版。
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
- IPA 音標列為必要，不可省略。

## 目錄頁

所有 CSV 與其來源聽力影片的對照表記錄在 `README.md`，**不另開 `INDEX.md`**。原因是本專案結構單純（一份規格文件加一批 CSV），多一個檔案就多一處要同步維護；而 README 是專案入口，推上 GitHub 後也會直接顯示在首頁，最容易被看到。

表格欄位：日期、主題、影片標題、連結、CSV 檔案、卡片數。

- 日期與 CSV 檔名開頭的日期一致。
- 影片標題與連結由使用者提供，不可自行推測。
- CSV 檔案欄位使用 Markdown 相對連結，例如 `[2026-08-20-airport-english.csv](cards/2026-08-20-airport-english.csv)`（顯示文字不含 `cards/`，連結路徑含）。
- 卡片數為 CSV 中的字卡列數，即總行數扣掉開頭兩行標頭。可用 `grep -c '^"' cards/檔名.csv` 取得。

何時改為獨立檔案：當目錄累積到約 40–50 筆以上，或需要增加分類、學習進度等欄位而使表格過寬時，再將目錄搬到獨立的 `INDEX.md`，並於 README 保留連結指向它。在那之前維持現狀。

### 雙索引分工

`dashboard.base` 看起來像第二份目錄，但它**不牴觸**上面「不另開 INDEX.md」的原則——因為它不需要人維護：

| | `README.md` 目錄表 | `dashboard.base` |
|---|---|---|
| 面向 | GitHub 網頁 | Obsidian |
| 維護方式 | **手動**，出卡與補卡時同步更新 | **自動**，讀 `transcripts/` 的 frontmatter |
| 卡片數 | 有，且為**唯一權威來源** | 無（Bases 讀不到 CSV 行數） |

「不另開 INDEX.md」擋的是「多一處要**同步維護**的地方」。`dashboard.base` 零維護成本，故不在此列。**但不要把卡片數搬進 frontmatter 讓它顯示**——那會讓它退化成第二個要手動同步的地方，正是本原則要防的事。

## Obsidian Vault

本 repo 的根目錄**同時就是 Obsidian vault 的根目錄**，不另建 vault 資料夾。用 Obsidian 的「開啟資料夾作為儲存庫」直接指向本 repo 即可。用途定位為**閱讀與檢視**（讀逐字稿、翻索引、搜尋查字），不在 Obsidian 裡做字卡編輯——字卡的唯一產出格式仍是 `cards/` 底下的 CSV。

### 逐字稿的 frontmatter

`transcripts/*.md` 檔首一律寫入以下四個欄位：

```yaml
---
title: "影片完整標題（去掉結尾的 - YouTube）"
url: https://www.youtube.com/watch?v=XXXXXXXXXXX
date: YYYY-MM-DD
topic: 主題中文名
---
```

- `title` 加雙引號：標題常含 emoji 與全形分隔符 `｜`，引號起來最保險。內容須與 README 目錄表的「影片標題」欄位一字不差。
- frontmatter 之後空一行，接原本的時間戳 bullet list。**不再另寫 H1 標題與裸 URL 行**——那些資訊已在 frontmatter，重複寫就是同一份資料存兩處。
- GitHub 會把 YAML frontmatter 渲染成表格，所以改用 frontmatter 不會讓 GitHub 端少看到東西。

**刻意不放的兩個欄位，請勿好心加回去：**

- **卡片數**：會隨補卡變動，放進來就成了 README 之外第二個要手動同步的地方。唯一權威來源是 README 目錄表。
- **`csv:` 路徑**：逐字稿與 CSV 依規格同名，路徑可直接推導（`transcripts/X.md` ↔ `cards/X.csv`），記下來是冗餘。

### `.obsidian/` 的版控界線

採**白名單制**：`.gitignore` 先以 `.obsidian/*` 全部排除，再逐項 `!` 放行共用設定。理由是新版 Obsidian 隨時會新增狀態檔，白名單制預設擋掉、黑名單制預設放行——前者出錯只是少同步一個設定，後者出錯是把逐次變動的 UI 狀態推上 git，多電腦必衝突。

此規則與使用者另外兩個 vault（`grammar-in-charts`、`java-se-17-technical-guide`）完全一致，跨專案只需記一種心智模型。

### `dashboard.base`

Obsidian 核心 Bases 功能產生的自動索引表，語法要點（已對 Obsidian 1.12.7 驗證）：

- `sort` 的條目是 `property` + `direction`，`direction` **只接受大寫** `ASC` / `DESC`。
- `order` 是欄位顯示順序（陣列），與 `sort` 是兩回事。
- frontmatter 欄位以 `note.` 前綴引用（`note.date`），檔案內建屬性用 `file.` 前綴（`file.name`）。

Bases 是核心外掛（Obsidian 1.9+ 內建、預設啟用），**不依賴 Dataview 等社群外掛**。若在某台電腦上 `dashboard.base` 打不開，先確認「設定 → 核心外掛 → Bases」是開啟的（`core-plugins.json` 依白名單制不進版控，故各機獨立）。
