# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## chrispan2026

萬金村村長候選人「潘譽仁」競選政見與政績文宣靜態網站（主軸：「屬於我們的未來萬金」）：

- 正式環境是純靜態網站（GitHub Pages），沒有任何後端、沒有資料庫、沒有登入機制。
- 包含兩個主要頁面：
  - 首頁（`frontend/src/pages/HomeView.vue`）：展示候選人簡介與三大願景、四大政見主張（建設萬金、福利萬金、文化萬金、產業觀光），以及政績統計摘要。
  - 政績頁（`frontend/src/pages/AchievementsView.vue`）：展示學經歷、政績分類篩選（全部／建設／福利／文化）、各分類代表照片與已完成政績條列。
- 技術棧比照同帳號 `show7587` 下的姊妹專案（`LinChengHung2026`、`WanluanDmap`）：前台 Vue 3 + Vuetify 3 + Vue Router + Vite，部署由 GitHub Actions（`.github/workflows/deploy.yml`）自動建置並發布到 GitHub Pages。

## 上線網址

`https://show7587.github.io/chrispan2026/`（GitHub Pages 預設網址，未設定自訂網域）

## 開發

```bash
cd frontend
npm install
npm run dev       # http://localhost:3456/chrispan2026/
npm run build     # 產出 dist/，模擬正式站建置
npm run preview   # 預覽 build 結果
npm run lint      # ESLint（standard + vue3-recommended），注意會自動 --fix 改檔
```

- 沒有任何測試框架或測試檔；驗證變更的方式是 `npm run build` 成功 + `npm run dev` 目視確認。
- CI 用 Node 20 + `npm ci`，新增/升級套件時必須一併提交更新後的 `frontend/package-lock.json`，否則部署會失敗。
- ESLint 規則：`comma-dangle: always-multiline`（多行物件/陣列結尾一定要逗號）、關閉 `vue/multi-word-component-names`。

## 架構重點

- 啟動流程：`main.js` → `plugins/index.js` 的 `registerPlugins()` 統一註冊 Vuetify 與 router。新的全域 plugin 加在這裡，不要直接改 `main.js`。
- 路由在 `src/router/index.js` **手動定義**（`MainLayout` 為外框，子路由為 `HomeView` 與 `AchievementsView`）。`vite.config.mjs` 雖然掛了 `unplugin-vue-router` 的 `VueRouter()` plugin，但程式沒有使用它的檔案式自動路由；新增頁面時要自己在 `router/index.js` 加路由。
- Vuetify 元件由 `vite-plugin-vuetify`（`autoImport: true`）自動引入，模板中直接用 `v-*` 元件即可，不需 import。icon 使用 `@mdi/font`（`mdi-*` 名稱）。
- 主題色依據競選文宣配色（primary 深綠 `#1F5135`、secondary 金色 `#C9A45C`、background 米白 `#FAF7F0`）與 `zhHant` locale 定義在 `src/plugins/vuetify.js`；`src/styles/settings.scss` 是 Vuetify 的 SASS `configFile`，也放了全域 body 樣式。

## 資料檔與事實來源說明

- 網站所有文字、統計數字與圖片清單均轉錄自競選文宣，唯一事實依據為 `docs/source-content.md`。
- 前端展示完全由 `frontend/src/data/` 下的三個 JSON 資料檔驅動（由 `docs/source-content.md` 整理產生）：
  - `profile.json`：候選人基本資料、號次、口號、三大願景（`pillars`）、學經歷、任期與金牌農村榮譽、頭像路徑。
  - `platform.json`：四大政見主張（標題、副標、icon、主題色、3 張縮圖、政見條目）。
  - `achievements.json`：8 項統計數字（`stats`）與三大政績分類（建設、福利、文化，各含 4 張附圖說照片與 10 項政績條目）。
- **若需修改網站上的政見或政績內容，請先對照 `docs/source-content.md` 並更新 `frontend/src/data/*.json` 資料檔**，不要在 Vue 元件模板中硬編碼事實、數字、日期或事蹟。
- `frontend/public/images/` 下的照片皆為從文宣裁切的低解析度縮圖（約 140×70），版面上應以小圖卡呈現，不可過度放大（不超過約 2 倍）以免模糊。

## 部署

push 到 `main` 分支後，GitHub Actions 會自動建置並部署到 GitHub Pages，無需手動操作。

## 已知的坑

1. `vite.config.mjs` 的 `base: '/chrispan2026/'` 必須跟 repo 名稱一致，且所有靜態資源／圖片路徑都要用 `` `${import.meta.env.BASE_URL}${src}` `` 組合（JSON 中的 `src` 不帶開頭 `/`），不能寫死 `/`。
2. 路由使用 `createWebHashHistory`（不是 history 模式），因為 GitHub Pages 靜態託管不方便處理 SPA 404 重導向。
