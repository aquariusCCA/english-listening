# Obsidian Vault 整合

本 repo 的根目錄**同時就是 Obsidian vault 的根目錄**，不另建 vault 資料夾。用 Obsidian 的「開啟資料夾作為儲存庫」直接指向本 repo 即可。用途定位為**閱讀與檢視**（讀逐字稿、翻索引、搜尋查字），不在 Obsidian 裡做字卡編輯——字卡的唯一產出格式仍是 `cards/` 底下的 CSV。

## 逐字稿的 frontmatter

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

## `.obsidian/` 的版控界線

採**白名單制**：`.gitignore` 先以 `.obsidian/*` 全部排除，再逐項 `!` 放行共用設定。理由是新版 Obsidian 隨時會新增狀態檔，白名單制預設擋掉、黑名單制預設放行——前者出錯只是少同步一個設定，後者出錯是把逐次變動的 UI 狀態推上 git，多電腦必衝突。

此規則與使用者另外兩個 vault（`grammar-in-charts`、`java-se-17-technical-guide`）完全一致，跨專案只需記一種心智模型。

## `dashboard.base`

Obsidian 核心 Bases 功能產生的自動索引表，語法要點（已對 Obsidian 1.12.7 驗證）：

- `sort` 的條目是 `property` + `direction`，`direction` **只接受大寫** `ASC` / `DESC`。
- `order` 是欄位顯示順序（陣列），與 `sort` 是兩回事。
- frontmatter 欄位以 `note.` 前綴引用（`note.date`），檔案內建屬性用 `file.` 前綴（`file.name`）。

Bases 是核心外掛（Obsidian 1.9+ 內建、預設啟用），**不依賴 Dataview 等社群外掛**。若在某台電腦上 `dashboard.base` 打不開，先確認「設定 → 核心外掛 → Bases」是開啟的（`core-plugins.json` 依白名單制不進版控，故各機獨立）。
