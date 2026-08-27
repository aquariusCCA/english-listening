# 聽力訓練單字 → Anki 卡片工作流程

本專案用途：從英文聽力逐字稿中整理中難度字彙，製作成可直接匯入 Anki 的 CSV 檔。

使用者以繁體中文溝通，回覆一律使用繁體中文。

## 動作選單

| 指令 | 動作 | 規則檔 |
|---|---|---|
| `/pick` | 從逐字稿挑出中難度字彙，列表後停下等挑選 | rules/pick.md |
| `/cards` | 依挑選編號出 CSV、更新 README 目錄、歸檔逐字稿 | rules/cards.md |
| `/add` | 補卡：使用者指名詞條，直接追加到既有 CSV | rules/patch.md |
| `/recheck` | 補卡：重掃逐字稿，只列尚未收錄的詞條 | rules/patch.md |
| `/end` | 收尾：commit + push | 本檔〈Git 工作流程〉 |

- 主流程是 `/pick` 挑字 → 使用者回覆編號 → `/cards` 出卡；複習後發現漏字才走 `/add` 或 `/recheck`。
- **規則本體在 `rules/`**，本檔只留每個 session 都用得到的核心；執行動作時再讀對應規則檔。`.claude/commands/` 內的指令檔只是薄指標，指令檔與本檔都不重複規則內容——改規則只需改 `rules/` 一處。
- 共用規格檔，由動作規則檔引用：rules/csv-format.md（CSV 格式與卡片背面模板）、rules/readme-index.md（README 目錄表規格與雙索引分工）、rules/obsidian.md（vault 整合、逐字稿 frontmatter、dashboard.base 語法）。維護 README 目錄、逐字稿或 Obsidian 設定前，先讀對應規格檔。
- 跨檔引用規則檔的小節時用名稱、不用編號——編號會因增刪位移而無聲指錯。
- 指令不是必要入口：使用者未下指令但意圖明確時（例如直接貼上逐字稿），依對應規則檔執行即可，不必要求他先打指令。

## 檔案結構

```
README.md                             目錄索引（專案入口，GitHub 首頁會直接顯示）
CLAUDE.md                             本檔：核心規格（動作選單、狀態管理、Git）
rules/                                規則本體：動作規則與共用規格（見「動作選單」）
dashboard.base                        Obsidian 端的自動索引
.gitignore                            白名單制排除 .obsidian/ 易變檔
cards/YYYY-MM-DD-主題英文名.csv        Anki 字卡
transcripts/YYYY-MM-DD-主題英文名.md   對應的來源逐字稿
.claude/commands/                     動作選單的指令檔（薄指標，指向 rules/）
.obsidian/                            Obsidian vault 設定（部分納入版控，見 .gitignore）
```

逐字稿與其 CSV **同名**（僅副檔名不同），一眼即可對應。YouTube 原始檔名通常又長又含 emoji 與全形分隔符，在 Windows 上會觸發路徑長度問題、在 Git 輸出中也會被轉義成難讀的八進位碼，因此一律改名後才入庫；原始標題保留在逐字稿的 frontmatter `title` 欄位與 README 目錄中，資訊不會遺失。

**檔名一律使用 ASCII**（英數字與連字號），理由同上。這條也適用於 `dashboard.base` 這類新增檔案——所以它不叫「總覽.base」。檔案*內容*用中文不受此限。

## 狀態管理：一切以 repo 檔案為準

- **不要使用機器本地的長期記憶（memory 目錄）。** 本 repo 會推上 GitHub 並在多台電腦之間同步，寫在單一機器 memory 的內容只有那台電腦看得到，而且失效時毫無徵兆。需要跨 session 記住的偏好與決策，一律寫進本檔或 `rules/` 的對應規則檔。
- **製卡紀錄以 repo 檔案為準**：CSV 檔案本身就是完整紀錄，README 目錄是索引。`/pick` 挑字前應先查既有 CSV，避免重複出卡。
- **複習排程與記憶狀態歸 Anki／AnkiWeb 管，不進 repo**，也不要試圖在 repo 裡追蹤。使用者以手機 AnkiDroid 複習，進度由 AnkiWeb 在裝置間同步。未來若需分析弱點字，再透過桌面版 Anki 的 AnkiConnect 外掛查詢複習統計，屆時才建對應機制。

## Git 工作流程

- 分支使用 `main`。
- **開工先 `git pull`**：讀任何檔案之前先拉最新狀態。多電腦使用下，本機落後時會依過期內容運作且無從自覺。若 pull 前工作區有未 commit 的變更（上次 session 中斷的殘留），先補一個收尾 commit 再 pull——Git 會拒絕在可能被覆蓋的未提交變更上執行 pull。
- **收尾 `git commit` + `git push`**：未 push 的 commit 對其他電腦等於不存在。使用者已授權收尾時主動 commit + push，無需逐次確認。
- 尚未設定 remote 時只 commit，並提醒使用者設定 remote。
- Commit message 一律用繁體中文，寫明「改了什麼、為什麼」。
