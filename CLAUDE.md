# 聽力訓練單字 → Anki 卡片工作流程

本專案用途：從英文聽力逐字稿中整理中難度字彙，製作成可直接匯入 Anki 的 CSV 檔。

使用者以繁體中文溝通，回覆一律使用繁體中文。

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

使用者挑選完畢後，將選定的詞條輸出成 CSV 檔。

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
