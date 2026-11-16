# 動漫介紹 LLM Wiki 操作日誌

> 本檔案採只增不減（Append-Only）原則記錄每次 `init`、`ingest`、`query` 與 `lint` 操作。
> 可用 `grep "^## \[" log.md | tail -5` 快速查看最近 5 筆紀錄。

## [2026-11-16] init | 建立動漫 LLM Wiki 初始架構
- 建立 `AGENTS.md` 規範文件、`index.md`、`log.md`。
- 建立 `raw/`（`articles/`、`notes/`、`assets/`）、`wiki/`（`works/`、`creators/`、`themes/`、`comparisons/`、`sources/`）與 `templates/` 目錄。

## [2026-11-16] ingest | 匯入《葬送的芙莉蓮》故事簡介
- 保存原始來源至 `raw/articles/2026-11-16-葬送的芙莉蓮-故事簡介.md`。
- 新建來源摘要頁 `wiki/sources/來源-葬送的芙莉蓮故事簡介.md`、作品頁 `wiki/works/葬送的芙莉蓮.md`、題材頁 `wiki/themes/奇幻冒險.md` 與 `wiki/themes/生命與時間.md`。
- 同步更新 `index.md`。

## [2026-11-16] ingest | 補充《葬送的芙莉蓮》播映年份與製作團隊
- 保存原始筆記至 `raw/notes/2026-11-16-葬送的芙莉蓮-播映與製作資訊.md`。
- 新建來源摘要頁 `wiki/sources/來源-葬送的芙莉蓮播映與製作資訊.md` 及創作者／公司頁 `MADHOUSE.md`、`山田鐘人.md`、`阿部司.md`。
- 更新 `wiki/works/葬送的芙莉蓮.md` 與 `index.md`。

## [2026-11-16] ingest | 補充《葬送的芙莉蓮》第 1、2 季導演與集數
- 保存原始筆記至 `raw/notes/2026-11-16-葬送的芙莉蓮-各季導演與集數.md`。
- 新建來源摘要頁 `wiki/sources/來源-葬送的芙莉蓮各季導演與集數.md` 及導演條目 `齋藤圭一郎.md`、`北川朋哉.md`。
- 更新 `wiki/works/葬送的芙莉蓮.md`、`wiki/creators/MADHOUSE.md` 與 `index.md`。

## [2026-11-16] ingest | 匯入《Dr.STONE 新石紀》作品介紹
- 保存原始文章至 `raw/articles/2026-11-16-Dr.STONE新石紀-作品介紹.md`。
- 新建來源摘要頁 `wiki/sources/來源-Dr.STONE新石紀作品介紹.md`、作品頁 `wiki/works/Dr.STONE 新石紀.md`。
- 新建創作者頁 `稻垣理一郎.md`、`Boichi.md` 與題材頁 `科學題材.md`、`文明重建.md`。
- 同步更新 `index.md`。

## [2026-11-16] init | 擴充研究筆記（notes/）與 HTML 簡報（presentations/）工作流
- 更新 `AGENTS.md`，新增「研究筆記 → 簡報逐頁腳本 → 單檔離線 HTML 簡報」流程、Wiki 缺口標註規則與 `wiki/comparisons/` 回存機制。
- 建立 `notes/`、`presentations/scripts/`、`presentations/html/` 目錄。
- 新增 `templates/research-note-template.md` 與 `templates/slide-script-template.md`。

## [2026-11-16] init | 補充建議文獻下載／Wiki 化與 LaTeX 支援規範
- 更新 `AGENTS.md` 與 `templates/research-note-template.md`，加入全流程 LaTeX 數學語法支援與建議文獻點擊下載／Wiki 化流程。

## [2026-11-16] init | 定案 HTML 簡報風格（淺色雜誌風）與進階互動規格
- 更新 `AGENTS.md`：確定 HTML 簡報採「淺色雜誌風、16:9 畫布、離線 MathML 渲染」，並保留總覽網格（`G`）、全螢幕（`F`）、講者備忘錄（`N`）與一鍵下載 `.md` 文獻按鈕。
