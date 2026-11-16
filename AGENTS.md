# AGENTS.md — 動漫介紹 LLM Wiki 維護規範

本規範參考 Andrej Karpathy 的 [LLM Wiki](https://gist.github.com/karpathy) 架構設計，專為**動漫介紹、作品評析與補番知識庫**打造。

> **核心理念**：傳統 RAG 每次提問都要重新檢索零散片段；本專案由 LLM 持續編撰並維護一個**持久化、會複利成長的 Markdown 知識庫（Wiki）**。你負責挑選來源、觀看作品與提出好問題；LLM 負責閱讀、摘要、建立交叉連結、標記觀點衝突與日常簿記。  
> **協作心法**：Obsidian 是你的 IDE，LLM 是程式設計師，整個 Wiki 就是程式碼庫。

---

## 一、三層架構與研究輸出分流

1. **原始資料層（`raw/`）— 唯讀、不可變的事實來源**
   - 存放剪藏的動漫評論、製作人員訪談、設定集摘錄、新聞稿、個人觀後感與圖片附件。
   - **鐵則**：LLM 只能讀取 `raw/`，**絕對不可修改或刪除**其中的任何檔案。
2. **知識庫層（`wiki/`）— 由 LLM 全權編撰與維護**
   - 存放所有結構化的 Markdown 頁面，包括作品介紹、創作者／製作公司、題材概念、跨作比較與來源摘要。
   - 由 LLM 負責建立新頁、更新既有頁面、維護 `[[雙向連結]]` 與保持全庫一致性。
3. **研究輸出層（`notes/` 與 `presentations/`）— 衍生研究與簡報產物**
   - `notes/`：嚴格依據 `wiki/` 既有內容撰寫的專題研究筆記（`.md`），與 `raw/notes/` 個人隨筆明確分流。
   - `presentations/`：由研究筆記轉化而成的簡報逐頁腳本（`scripts/`）與單一自包含 HTML 簡報（`html/`）。
4. **規範層（`AGENTS.md`）— 本檔案**
   - 定義資料夾結構、頁面格式、寫作慣例與操作流程。隨著知識庫演進，你與 LLM 可共同迭代修改本檔案。

---

## 二、目錄結構

```text
.
├── AGENTS.md                       # 本規範文件（Schema）
├── index.md                        # 內容導向總目錄（依分類列出所有頁面與一句話摘要）
├── log.md                          # 時間軸導向日誌（只增不減的 ingest / query / research / lint 紀錄）
├── raw/                            # 【唯讀】原始資料層
│   ├── articles/                   # Web Clipper 剪藏文章、訪談、評論
│   ├── notes/                      # 個人觀後隨筆、零散摘錄
│   └── assets/                     # 本地圖片、海報、截圖（Obsidian 附件路徑）
├── wiki/                           # 【LLM 維護】知識庫層
│   ├── works/                      # 動漫作品介紹頁（TV、劇場版、OVA）
│   ├── creators/                   # 監督、腳本家、原畫師、聲優、動畫製作公司
│   ├── themes/                     # 題材、流派、敘事母題、作畫與音樂風格
│   ├── comparisons/                # 跨作品比較、補番推薦清單、專題分析（含確認後回存之研究筆記）
│   └── sources/                    # 對應 raw/ 各來源的精煉摘要頁
├── notes/                          # 【研究輸出】依 Wiki 產出的專題研究筆記（.md，支援 LaTeX）
├── presentations/                  # 【研究輸出】簡報產物
│   ├── scripts/                    # 簡報逐頁文字稿（.md，支援 LaTeX）
│   └── html/                       # 單一自包含離線 HTML 簡報（.html，內建離線 LaTeX 渲染）
└── templates/                      # 頁面與研究輸出範本
    ├── work-template.md
    ├── creator-template.md
    ├── theme-template.md
    ├── source-summary-template.md
    ├── research-note-template.md
    └── slide-script-template.md
```

---

## 三、頁面格式與動漫領域慣例

### 1. YAML Frontmatter（供 Obsidian Dataview 查詢）
每個 `wiki/` 頁面頂部皆須包含標準 YAML Frontmatter：
- **作品頁（`wiki/works/`）**：`type: work`、`title`、`aliases`（含日文原名、英文名、常見簡稱）、`year`、`season`（如 `2024-春`）、`studio`、`director`、`genres`、`watch_status`（`想看` / `追番中` / `已看完` / `擱置`）、`rating`、`sources_count`、`updated`
- **創作者／公司頁（`wiki/creators/`）**：`type: creator` 或 `type: studio`、`name`、`aliases`、`roles`、`updated`
- **題材概念頁（`wiki/themes/`）**：`type: theme`、`name`、`aliases`、`updated`
- **來源摘要頁（`wiki/sources/`）**：`type: source`、`raw_file`、`author`、`date`、`ingested`

### 2. 劇透隔離原則（Spoiler Policy）
動漫介紹最重要的原則是**讓未看過的讀者能安心閱讀**：
- **作品頁前半部**（基本資訊、劇情簡介、無雷看點與適合族群）**嚴禁包含劇透**。
- **核心劇情解析、結局討論、伏筆回收**必須統一收錄在頁面後半部的摺疊區塊（Obsidian Callout）中：
  ```markdown
  > [!warning]- 劇透解析（點擊展開）
  > 此處放置涉及核心反轉、結局與深度劇透的分析內容。
  ```
- **簡報劇透預警**：若簡報中有頁面涉及核心劇情劇透，必須在該頁之前安插一張明確的「劇透預警頁」。

### 3. 連結、命名與 LaTeX 語法慣例
- **雙向連結**：文中首次提及已存在或值得建頁的作品、創作者、製作公司、題材時，使用 `[[頁面名稱]]` 建立連結。
- **譯名統一**：以台灣／繁體中文常用正式譯名為主要檔名（例如 `葬送的芙莉蓮.md`），並在 `aliases` 中補齊原文與別名（如 `葬送のフリーレン`、`Frieren`）。
- **引用來源**：所有客觀數據（集數、播映年份、製作人員）或他人評論觀點，皆須標註來自哪一份 `[[wiki/sources/...]]` 或原始檔案路徑。
- **LaTeX 支援**：所有 `.md` 檔案（`wiki/`、`notes/`、`presentations/scripts/`）皆支援標準 LaTeX 語法（行內 `$...$` 與區塊 `$$...$$`），可用於科學設定／化學式、時間尺度比例或評分模型公式；轉為 HTML 簡報時須預編譯為瀏覽器原生支援的 MathML／SVG 並搭配內嵌樣式，確保單檔離線開啟時正常顯示。

---

## 四、核心操作流程（Operations）

### 1. Ingest（資料匯入與知識編譯）
當使用者貼上新文章或在 `raw/` 放入新資料（文章、訪談、筆記）並要求匯入時：
1. **保存原文與閱讀**：先將原文完整保存至 `raw/articles/` 或 `raw/notes/`（不可修改）；完整閱讀內容並向使用者簡述重點與擬更新頁面（預設採單篇精讀模式；若使用者明確要求批次匯入，則直接執行）。
2. **建立來源摘要頁**：在 `wiki/sources/` 建立對應摘要頁（參考 `templates/source-summary-template.md`），記錄出處、關鍵論點與引用資訊。
3. **更新知識庫頁面**：
   - 建立或更新對應的 `wiki/works/` 作品頁、`wiki/creators/` 創作者／公司頁與 `wiki/themes/` 題材頁（單次匯入通常會連動更新多個頁面）。
   - 若新來源與既有頁面內容矛盾（例如不同訪談對製作幕後的說法不同，或評價出現兩極分歧），**不要直接覆蓋舊說法**，應在頁面中並列呈現並標註來源差異。
4. **更新索引與日誌**：
   - 更新 `index.md` 中對應分類的條目、一句話摘要與來源計數。
   - 在 `log.md` 底部追加一筆紀錄。

### 2. Query（一般查詢與知識沉澱）
當使用者針對動漫知識庫提問時：
1. **先讀索引**：先讀取 `index.md` 定位相關頁面，再深入讀取對應的 `wiki/` 檔案（不依賴單純關鍵字猜測，亦不需每次重掃整個 `raw/`）。
2. **合成回答**：結合多個頁面資訊回答問題，並清楚附上所引用的 Wiki 頁面連結。
3. **沉澱回知識庫**：若產出了有價值的比較或分析，可依指示整理存入 `wiki/comparisons/`，並同步更新 `index.md` 與 `log.md`。

### 3. Research & Presentation（專題研究筆記與 HTML 簡報流程）
當使用者提出一個研究主題或問題，要求進行專題整理並製作簡報時，依以下三階段執行：

#### 階段 A：產出研究筆記（`notes/`）
1. **嚴格依 Wiki 撰寫正文**：研究筆記（`.md`）的正文內容**僅能使用 `wiki/` 內已收錄的資料**，參考 `templates/research-note-template.md` 存於 `notes/` 資料夾（支援 LaTeX 公式）。
2. **缺口標註原則**：當研究問題超出目前 Wiki 收錄範圍時，正文中遇到缺漏處必須直接標註 `【Wiki 尚無資料，待補充】`，**嚴禁將尚未匯入 Wiki 的網路搜尋內容寫入正文**。
3. **上網搜尋與一鍵下載／Wiki 化建議文獻**：同步上網搜尋該主題的相關資料，將尚未放入 Wiki 的外部來源整理於筆記最末端的「建議參考文獻」區塊，並逐筆提供：
   - **可點擊連結與下載／Wiki 化入口**：附上原始網址 `[來源標題](URL)` 與可下載為 `.md` 草稿的對應檔名，讓使用者點選或指定編號（如「Wiki 化建議文獻 #1」）即可自動抓入 `raw/articles/` 並執行完整 `ingest` 流程。
   - **建議補入的 Wiki 頁面／目的**：例如「建議補入 `wiki/works/Dr.STONE 新石紀.md`（補齊動畫製作公司與監督）」或「建議新建 `wiki/works/...`（作為同類作品比較）」。
4. **確認後回存 Wiki**：當使用者確認對該研究筆記滿意後，依規範同步複製或精煉一份存入 `wiki/comparisons/`，並登記至 `index.md` 與 `log.md`。

#### 階段 B：產出簡報逐頁腳本（`presentations/scripts/`）
根據定案的 `notes/` 研究筆記，參考 `templates/slide-script-template.md` 產出簡報逐頁文字稿並存於 `presentations/scripts/`（支援 LaTeX 語法）。每一頁**必須固定包含以下四個欄位**：
- **頁面標題**（含頁碼，如 `Slide 01｜標題`）
- **畫面上呈現的精簡重點**（控制字數，避免長段落）
- **視覺／排版建議**（含是否預留海報或劇照插槽，對應 `raw/assets/`）
- **講者備忘錄（口語補充）**
> 若簡報內容涉及核心劇情劇透，務必在劇透頁之前安插一張獨立的「劇透預警頁」。

#### 階段 C：產出單一自包含 HTML 簡報（`presentations/html/`）
根據逐頁腳本，生成**單一自包含的 `.html` 檔案**存於 `presentations/html/`，並遵循以下定案風格與互動規格：
- **視覺風格（淺色雜誌風）**：米白紙質底色搭配細框線、清晰雙欄／卡片排版與大量留白；採 **16:9 寬螢幕畫布**，並依作品主題調整 CSS 強調色變數。
- **離線與 LaTeX 支援**：不依賴外部 CDN，雙擊即可離線開啟與分享；LaTeX 公式預編譯為瀏覽器原生 **MathML**（搭配內嵌樣式），離線狀態下也能正確顯示。
- **完整互動控制**：
  - 鍵盤 `←` / `→` 與畫面按鈕換頁、頁碼進度條顯示。
  - `N` 鍵（或按鈕）開關底部「講者備忘錄」。
  - `G` 鍵（或按鈕）開啟「投影片總覽網格」快速跳頁。
  - `F` 鍵（或按鈕）切換全螢幕簡報模式。
- **圖片插槽與建議文獻一鍵下載**：
  - 依逐頁腳本於指定頁面預留 `2:3` 直式海報或 `16:9` 橫式劇照插槽（對應 `raw/assets/` 相對路徑；無圖時顯示標註建議檔名的簡潔佔位框）。
  - 末頁「建議參考文獻」提供**一鍵下載 `.md` 文獻草稿按鈕**（離線可觸發瀏覽器下載，方便放入 `raw/articles/` 進行 Wiki 化）。

### 4. Lint（定期健康檢查）
當使用者要求對 Wiki 進行健康檢查（Lint）時，全面掃描 `wiki/`、`index.md` 與 `raw/`，檢查並回報以下項目：
- **矛盾與過時資訊**：例如續作已播映但作品頁仍寫「未定」、不同頁面記載的監督或集數不一致。
- **孤兒頁面（Orphan Pages）**：沒有任何其他頁面連入的 Wiki 頁面，建議應補上的關聯連結。
- **缺頁實體（Missing Pages）**：在多篇文章中被頻繁提及（如某位重要監督或關鍵題材），但尚未擁有獨立頁面的條目。
- **資料缺口與建議**：結合 `notes/` 中標註的「待補充缺口」與建議參考文獻，提出下一波優先蒐集或匯入的來源方向。

---

## 五、索引與日誌規範

### `index.md`（內容導向）
- 依「作品（Works）」、「創作者與製作公司（Creators & Studios）」、「題材與風格（Themes）」、「專題比較（Comparisons）」、「來源摘要（Sources）」分類。
- 每條格式：`- [[頁面名稱]] — 一句話摘要（更新：YYYY-MM-DD）`

### `log.md`（時間軸導向，只增不減）
- 每筆紀錄必須以固定前綴開頭，方便使用 `grep "^## \[" log.md | tail -5` 等指令快速檢索近期動態：
  ```markdown
  ## [YYYY-MM-DD] <ingest|query|research|lint|init> | <簡短標題>
  - 操作摘要與受影響頁面列表
  ```

---

## 六、Obsidian 搭配建議（選用）

- **Web Clipper**：使用 Obsidian Web Clipper 瀏覽器擴充功能，一鍵將巴哈姆特、Anitrendz、Sakugabooru Blog 或訪談文章轉存為 Markdown 放入 `raw/articles/`。
- **本地圖片下載**：在 Obsidian 設定中將「附件預設存放路徑」設為 `raw/assets/`，剪藏後下載圖片至本地，避免外部圖床失效，並可供 HTML 簡報插槽直接引用。
- **Graph View**：透過 Obsidian 關聯圖譜觀察哪些題材或創作者成為連結核心（Hub），以及哪些作品處於孤立狀態。
- **Dataview**：搭配本規範的 YAML Frontmatter，可在首頁或自訂看板自動生成「已看完高分清單」、「特定製作公司作品年表」等動態表格。
- **Git 版本控制**：整個資料夾即為純文字 Git Repository，建議每次完成 `ingest`、`research` 回存或 `lint` 後提交一次 commit，保留知識庫演化歷史。