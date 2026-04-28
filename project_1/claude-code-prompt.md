# Claude Code 執行指令：餐廳線上菜單

## 專案概述

你是一個專業的資深 Front-End 工程師，基於提供的設計稿與設計規格文件的內容，使用 **React + Vite + TypeScript** 建置一個手機專用的靜態餐廳菜單網站，部署在本 Repository 的 GitHub Pages 上。

---

## 技術棧

- **框架**：React 18 + TypeScript
- **建置工具**：Vite
- **樣式**：CSS Modules 或 Tailwind CSS（依設計稿複雜度選擇，優先 Tailwind）
- **部署**：GitHub Pages（透過 GitHub Actions 自動部署）
- **不使用**任何後端、資料庫、或 SSR

---

## 專案結構

```
├── .github/
│   └── workflows/
│       └── deploy.yml          # GitHub Actions：build → 部署至 GitHub Pages
├── public/
│   └── images/                 # 菜品照片，檔名對應 menu-raw.js 中的 image 欄位
│       └── .gitkeep
├── src/
│   ├── data/
│   │   ├── menu-raw.ts         # 店家維護的簡化菜單原始資料（純中文）
│   │   ├── menu-i18n.ts        # 完整四語言菜單資料（build script 或手動產生）
│   │   ├── tags.ts             # 標記系統定義（代碼 → 圖示 + 四語言文字）
│   │   └── site-info.ts        # 店家資訊、用餐規則等多語言常量
│   ├── components/
│   │   ├── Header.tsx           # 店名 + 形象圖 + 用餐資訊
│   │   ├── LanguageSwitcher.tsx # 語言切換按鈕（sticky）
│   │   ├── CategoryNav.tsx      # 分類快捷跳轉列（sticky，在語言切換下方）
│   │   ├── MenuSection.tsx      # 單一分類區塊（標題 + 品項卡片列表）
│   │   ├── MenuCard.tsx         # 品項卡片（照片/emoji、名稱、價格、標記 badge）
│   │   ├── ItemDetailModal.tsx  # 品項詳情 Overlay（大圖、縮放、說明、備註）
│   │   └── TagBadge.tsx         # 標記小徽章元件
│   ├── hooks/
│   │   └── useLanguage.ts       # 語言狀態管理（Context + hook）
│   ├── types/
│   │   └── menu.ts              # TypeScript 型別定義
│   ├── App.tsx
│   ├── main.tsx
│   └── index.css
├── index.html
├── vite.config.ts
├── tsconfig.json
├── package.json
└── README.md
```

---

## 資料流

```
menu-raw.ts（店家編輯，純中文）
       │
       ▼
menu-i18n.ts（完整四語言資料，翻譯後放入此檔）
       │
       ▼
  React 元件讀取 menu-i18n.ts → 依使用者選擇的語言渲染
```

### menu-raw.ts

格式與**設計規格文件**中「8-1. 簡化輸入格式」一致。這是店家人員實際會編輯的檔案。請從該文件中複製完整的 `menuRaw` 陣列作為初始資料。

### menu-i18n.ts

完整的四語言資料。初次建立時，請根據 `menu-raw.ts` 的中文內容，將所有品項名稱、規格標籤（大/小、杯/壺、烤雞腿/牛肉/蔬食等）、備註翻譯為英文、日文、韓文，寫入此檔。格式依**設計規格文件**中「8-2. 完整渲染格式」。

---

## 設計參考

1. **優先**從使用者輸入的設計稿讀取設計稿的色彩、字體、間距、元件佈局
  - 設計稿若是 pencil 直透過 pencil MCP 讀取
  - 設計稿若是 Claude Design, 則透過 Claude Design URL 取得
2. 若設計稿中未定義的細節，參考**設計規格文件**中的設計規範（第二節）
3. 整體視覺以 375px 寬手機為基準，不需處理桌面或平板適配

---

## 功能需求

請完整實作**設計規格文件**中第三至第九節的所有功能，重點摘要：

### 必要功能清單

- [ ] **Header**：店名、形象圖片、地址、電話（多語言）
- [ ] **語言切換**：繁中/英/日/韓，sticky 定位，切換不重載頁面
- [ ] **分類快捷列**：sticky，點擊可跳轉至對應分類區塊（smooth scroll）
- [ ] **品項卡片**：照片（或 emoji fallback）、多語言名稱、價格含規格、標記 badge
- [ ] **品項詳情 Modal**：Overlay 開啟、大圖可 pinch-to-zoom、說明與備註欄位、點擊外部或關閉鈕回到原位
- [ ] **標記系統**：依**設計規格文件**第七節的標記對照表實作
- [ ] **圖片 lazy loading**
- [ ] **所有價格顯示 NT$ 前綴**

### 圖片處理

- 品項的 `image` 欄位若有值，從 `/images/{檔名}` 載入
- 若 `image` 為 `null`，以該品項的 `emoji` 欄位放大居中顯示作為替代
- 之後店家只要將照片放入 `public/images/` 並更新 `menu-raw.ts` 的 `image` 欄位即可

---

## GitHub Pages 部署

### vite.config.ts
                                           
設 `base: './'`，產出的資源路徑為相對路徑，
同一份 `dist/` 可同時部署於以下三種情境而不需重 build：                                                                                                                                                                             

  - 預設 GitHub Pages 子路徑：`https://<username>.github.io/<repo>/`
  - 自訂網域根路徑：`https://menu.example.com/`
  - 本機 `npm run preview` 或直接開啟 `dist/index.html` 

```ts
  export default defineConfig({ 
    base: './',
    plugins: [react()],
  })
```

### GitHub Actions Workflow（.github/workflows/deploy.yml）

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - uses: actions/checkout@v4

      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm

      - run: npm ci
      - run: npm run build

      - uses: actions/upload-pages-artifact@v3
        with:
          path: dist

      - id: deployment
        uses: actions/deploy-pages@v4
```

### GitHub Repository 設定

部署後需在 GitHub 上確認以下設定：

1. 進入 Repository → **Settings** → **Pages**
2. **Source** 選擇 **GitHub Actions**（不是選 branch）
3. 首次 push 到 main 後 workflow 會自動執行，完成後即可透過 `https://<username>.github.io/<repo-name>/` 存取

---

## 本地開發

```bash
npm install
npm run dev      # 啟動 Vite dev server，預設 http://localhost:5173
npm run build    # 建置至 dist/
npm run preview  # 本地預覽 build 結果
```

---

## 注意事項

- 所有翻譯（英文、日文、韓文）請盡量準確，菜名翻譯以觀光客能理解為優先，不需要逐字直譯
- **設計規格文件**中的標記定義表（第七節）是唯一的標記來源，不要自行新增或推測品項的標記
- 確保 Modal 開啟時 body 不可捲動，關閉後恢復捲動位置
- 整個專案不應有任何 API 呼叫或外部資料依賴，所有資料都從 `src/data/` 靜態載入
