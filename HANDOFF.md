# HANDOFF：chrispan2026 網站實作規格（給 agy）

你在 `~/github/chrispan2026` 實作「萬金村村長候選人 潘譽仁」的靜態政見網站。
參考架構（唯讀，**不得修改**）：`~/github/LinChengHung2026`，請先讀它的 `CLAUDE.md`、`frontend/` 與 `.github/workflows/deploy.yml`。

## 檔案擁有權（嚴格遵守，完成後會用 git 檢查）
- **你可以建立／修改**：`frontend/**`（下列例外除外）、`.github/workflows/deploy.yml`、`CLAUDE.md`、`AGENTS.md`。
- **不得建立、修改或刪除**：
  - `frontend/src/data/**`（資料檔已由別人產出，只能讀取）
  - `frontend/public/images/**`
  - `docs/**`、`HANDOFF.md`、根目錄 `.gitignore`
  - `frontend/package-lock.json`（不要執行 `npm install`／`npm ci`，lockfile 由別人產生）
  - `~/github/LinChengHung2026` 底下任何檔案
- 不要執行 git commit／push。

## 技術架構（照抄參考 repo，只改必要處）
- Vue 3 + Vuetify 3 + Vue Router + Vite，所有前端程式放在 `frontend/`。
- `frontend/package.json`：`name` 改成 `chrispan2026`，dependencies／devDependencies 與 scripts 跟參考 repo 完全一樣。
- `frontend/vite.config.mjs`：照抄，`base` 改成 `'/chrispan2026/'`，port 3456。
- `frontend/.eslintrc`、`frontend/index.html`（`<title>` 改成「萬金村村長候選人 潘譽仁｜屬於我們的未來萬金」，`lang="zh-Hant"`）。
- `src/main.js` → `src/plugins/index.js` 的 `registerPlugins()` 統一註冊 Vuetify 與 router。
- `src/router/index.js`：手動定義路由，`createWebHashHistory`，`MainLayout` 為外框，子路由 `''`（HomeView）與 `'achievements'`（AchievementsView），`scrollBehavior: () => ({ top: 0 })`。
- `src/plugins/vuetify.js`：保留 `zhHant` locale，主題改成文宣配色：primary 深綠 `#1F5135`、secondary 金色 `#C9A45C`，背景偏米白（例如 `#FAF7F0`），其他顏色自行搭配。
- `src/styles/settings.scss`：照參考 repo 的寫法。
- `.github/workflows/deploy.yml`：照抄參考 repo，不要改。
- ESLint：standard + vue3-recommended、`comma-dangle: always-multiline`、關閉 `vue/multi-word-component-names`；程式碼要符合這些規則。
- icon 用 `@mdi/font`（`mdi-*`），Vuetify 元件自動引入。

## 資料（唯讀，從 `@/data/*.json` import）
- `profile.json`：`name`、`title`、`number`、`slogan`、`tagline`、`intro`（串場文字，可能沒有）、`pillars[{title,desc}]`、`education[]`、`experience[]`、`tenure`、`achievementsHeading`、`honor`、`photo`。
- `platform.json`：陣列，每組 `{key,title,subtitle,icon,color,images[{src,alt}],items[]}`，共 4 組。
- `achievements.json`：`{stats[{value,unit,label}], categories[{key,label,icon,color,photos[{src,caption}],items[]}]}`。
- 所有圖片 `src`／`photo` 都是相對於 `frontend/public/` 的路徑（例如 `images/profile/pan.jpg`），顯示時一律寫成 `` `${import.meta.env.BASE_URL}${src}` ``，不能寫死 `/`。
- 頁面上不得出現資料檔以外的事實（數字、事蹟、日期、英文名）；版面上的標題、按鈕文字可以自訂。

## 頁面
### `MainLayout.vue`
app bar（primary 色）：左邊 router-link 回首頁，文字「萬金村村長候選人 潘譽仁」；右邊連到「政見」（首頁錨點或首頁）與「政績」（`/achievements`）。

### `/` HomeView
1. **Hero**：頭像（`profile.photo`，圓形）、`title`、大字 `name`、圓形號次徽章 `number`、`tagline`（屬於我們的未來萬金）、`slogan`、`intro`（有才顯示）；下面三張願景卡（`pillars`）。
2. **政見主張**：4 組卡片，每組顯示序號（01–04）、`title`、`subtitle`、3 張照片縮圖（小圖，`object-fit: cover`）、`items` 條列。手機上單欄，桌機兩欄。
3. **政績摘要**：`tenure` 標題、`honor` 橫幅、8 項 `stats`（數字大字＋單位＋說明，手機 2 欄、桌機 4 欄）、按鈕「看完整政績」連到 `/achievements`。

### `/achievements` AchievementsView
1. 標題 `tenure` + `achievementsHeading`；學歷、經歷兩欄卡片。
2. 分類篩選（全部／建設／福利／文化，用 `v-chip-group` 或 `v-btn-toggle`）。
3. 每個分類一個區塊：4 張照片卡（圖＋`caption`）與 `items` 條列（帶 check icon）。

## 設計要求
- 整體風格參考文宣：深綠底塊、金色點綴、白色卡片、圓角。
- RWD：手機寬度（375px）不得出現水平捲軸，照片是低解析度縮圖（約 140×70），**不要放大超過約 2 倍**，用小卡片呈現。
- 中文字體用系統字體即可（Roboto 保留給英數）。

## 文件
- 新增 `CLAUDE.md`：比照參考 repo 的結構，寫本專案的定位、上線網址 `https://show7587.github.io/chrispan2026/`、開發指令、架構重點、資料檔說明（內容由 `docs/source-content.md` 產生，改內容要先改資料檔）、已知的坑。
- 新增 `AGENTS.md`：一行 `See CLAUDE.md`，或摘要同樣內容給非 Claude 的 agent。

## 完成時回報
列出建立／修改的檔案、未完成或有疑慮的地方。不要執行 npm 指令、不要 commit。
