# /cards：產出 Anki CSV

使用者挑選完畢後：

1. 將選定的詞條輸出成 CSV 檔，格式見 rules/csv-format.md。
2. **同步更新 `README.md` 的目錄表格**，為這份 CSV 新增一列，欄位規格見 rules/readme-index.md。此步驟不可省略，否則目錄會失效。
3. **將逐字稿歸檔到 `transcripts/`**，改名為與 CSV 相同的檔名（副檔名 `.md`），並在檔首寫入 YAML frontmatter（四個欄位，格式見 rules/obsidian.md）。使用者若是把逐字稿貼在對話中而非給檔案，一樣要建檔存入——之後補卡查原句全靠它。歸檔後刪除原本散落在根目錄的檔案（`git mv` 即可一次完成搬移與改名）。
4. 若使用者尚未提供該支影片的標題與連結，主動向他索取，不要留白或自行臆測。
