# LLM 研究 Wiki 網站 - Agent 協作與開發規範 (AGENTS.md)

本檔案定義了 AI Agent 在本專案中的角色、知識庫維護規則以及靜態網頁開發標準。本專案結合 **Karpathy LLM Wiki（持續累積式知識庫）** 與 **GitHub Pages 靜態網站架構**，目標是讓 AI 每次吸收新論文或概念後，不僅更新 Markdown 知識庫，也能自動生成風格一致、可直接在網頁瀏覽的研究頁面。

---

## 1. 專案目的與核心原則

- **專案目的**：打造一個專注於「大型語言模型（LLM）研究」的個人數位花園與研究講義網站，將艱澀的論文與技術概念轉化為結構化、相互連結的網頁知識庫。
- **知識累積 (Compounding)**：每次新增資料（`Ingest`）時，必須同步更新 Markdown 知識庫（`wiki/`）與對應的網頁檔案（`docs/`），並更新總目錄與導覽列，避免孤島頁面。
- **前後端分離、純靜態部署**：
  - `raw/` 與 `wiki/` 為知識庫底層資料（Markdown 格式，適合用 Obsidian 檢視）。
  - `docs/` 為 GitHub Pages 的發佈根目錄（純 HTML + CSS + Vanilla JS，無需複雜建置工具即可直接開啟與部署）。
- **設計一致性**：所有由 AI 新增的 HTML 頁面，必須嚴格遵守本文件的版面結構與 CSS 變數規範，維持與首頁相同的閱讀體驗。

---

## 2. 目錄與檔案命名規範

```text
llm-research-wiki-web/
├── AGENTS.md                 # 本規範檔（AI 每次任務前必讀）
├── README.md                 # GitHub 專案說明文件
├── index.md                  # 知識庫內容總目錄
├── log.md                    # 知識庫演進時間軸日誌（Append-only）
├── raw/                      # 原始文獻區（唯讀，不可修改）
├── wiki/                     # Obsidian 知識庫 Markdown 層
│   ├── concepts/             # 技術概念筆記（小寫英文與連字號，如 kv-cache.md）
│   ├── models/               # 模型架構筆記（如 deepseek-r1.md）
│   └── papers/               # 論文精讀筆記（如 attention-is-all-you-need.md）
└── docs/                     # GitHub Pages 公開網頁根目錄
    ├── index.html            # 網站首頁（知識地圖、最新更新、分類入口）
    ├── assets/
    │   └── style.css         # 全站共用樣式表（禁止在單一 HTML 寫大量內嵌 CSS）
    ├── concepts/             # 對應 wiki/concepts/ 的 HTML 頁面
    ├── models/               # 對應 wiki/models/ 的 HTML 頁面
    └── papers/               # 對應 wiki/papers/ 的 HTML 頁面
```

- **命名規則**：一律使用**全小寫英文字母與連字號（kebab-case）**，例如 `mixture-of-experts.md` 對應 `docs/concepts/mixture-of-experts.html`。禁止使用空白或中文檔名，確保網址乾淨且跨平台相容。

---

## 3. 網頁視覺風格與 HTML 結構規範

### 3.1 視覺風格（學術筆記風 / 現代極簡）
- **配色系統**（定義於 `docs/assets/style.css`）：
  - 背景色：`#f8fafc`（淺灰白護眼底色）、卡片背景：`#ffffff`
  - 主色調：`#2563eb`（學術藍，用於連結、強調與標籤）
  - 文字主色：`#1e293b`、次要文字：`#64748b`
  - 邊框色：`#e2e8f0`
- **排版規範**：正文最大寬度 `860px` 置中閱讀，行高 `1.75`，支援響應式設計（手機與桌機皆可正常瀏覽）。

### 3.2 新增子頁面（`docs/<category>/<slug>.html`）標準模板
AI 在建立任何新頁面時，必須複製以下骨架並填入內容：

```html
<!DOCTYPE html>
<html lang="zh-Hant">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[頁面標題] | LLM Research Wiki</title>
  <link rel="stylesheet" href="../assets/style.css">
</head>
<body>
  <header class="site-header">
    <div class="container">
      <a href="../index.html" class="brand">🧠 LLM Research Wiki</a>
      <nav class="nav-links">
        <a href="../index.html">總覽首頁</a>
        <a href="../index.html#papers">論文精讀</a>
        <a href="../index.html#concepts">核心概念</a>
        <a href="../index.html#models">模型架構</a>
      </nav>
    </div>
  </header>

  <main class="container article-container">
    <div class="breadcrumb">
      <a href="../index.html">首頁</a> / <span>[分類名稱]</span>
    </div>
    <article class="wiki-article">
      <header class="article-header">
        <div class="meta-tags">
          <span class="badge">[分類標籤]</span>
          <span class="date">更新日期：YYYY-MM-DD</span>
        </div>
        <h1>[頁面標題]</h1>
        <p class="lead">[一句話核心摘要]</p>
      </header>

      <section class="article-body">
        <!-- 正文區塊：使用 h2, h3, p, ul, table, blockquote -->
      </section>

      <footer class="related-links">
        <h3>🔗 相關知識連結 (Wiki Links)</h3>
        <ul>
          <li><a href="...">相關頁面標題</a></li>
        </ul>
      </footer>
    </article>
  </main>

  <footer class="site-footer">
    <div class="container">
      <p>Maintained by AI Agent under <code>AGENTS.md</code> · Powered by LLM Wiki Pattern</p>
    </div>
  </footer>
</body>
</html>
```

---

## 4. 標準工作流程 (Workflows)

當使用者要求「新增一篇論文/概念/模型」時，AI 必須依序執行以下 5 個步驟：

1. **撰寫知識庫筆記**：在 `wiki/<category>/<slug>.md` 建立繁體中文筆記，包含 YAML Frontmatter、核心摘要、技術細節與 `[[Wiki Links]]`。
2. **生成對應網頁**：依據第 3.2 節的 HTML 模板，在 `docs/<category>/<slug>.html` 建立對應網頁，確保 CSS 路徑（`../assets/style.css`）與首頁返回連結正確。
3. **建立雙向連結**：檢查現有相關頁面，在彼此的「相關知識連結」區塊加入超連結。
4. **更新總目錄與首頁**：
   - 在 `index.md` 新增條目與一句話摘要。
   - 在 `docs/index.html` 對應分類的卡片列表中新增該頁面的連結卡片。
5. **寫入演進日誌**：在 `log.md` 最下方附加一筆紀錄：`## [YYYY-MM-DD] ingest | <頁面標題>`，列出本次新增與修改的檔案清單。
