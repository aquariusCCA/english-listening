# CSV 格式與卡片背面模板

/cards 與補卡（/add、/recheck）共用的產出規格。

## CSV 檔案格式

- 路徑與檔名：`cards/YYYY-MM-DD-主題英文名.csv`（日期用當天日期，如 `cards/2026-08-20-airport-english.csv`）。CSV 一律放在 `cards/`，不放根目錄——Obsidian 開不了 CSV，散在根目錄會把 vault 的檔案總管洗版。
- 編碼：UTF-8
- 檔案開頭兩行固定為：

  ```
  #separator:Comma
  #html:true
  ```

- 每列兩個欄位（皆以雙引號包住，內部的 `"` 以 `""` 跳脫）：
  - **正面**：`單字 (詞性縮寫)`，如 `"reservation (n.)"`；片語無詞性可省略，如 `"catch of the day"`
  - **背面**：HTML 內容，見下方「卡片背面 HTML 模板」

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
