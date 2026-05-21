# Prompt 3 — 完成實作與上線
> 工具：Claude Code Desktop（接收 Claude Design 的 Handoff）
> 目標：接收設計稿、補完功能、確保響應式支援、啟用 GitHub Pages、網站上線

---

請全程以**繁體中文**輸出所有說明、回應與提示訊息。

## 第一步：確認 Handoff 內容

確認從 Claude Design 收到的 Handoff 包含以下內容，
若有缺漏請告知使用者：

```
✅ 已收到 Claude Design 的 Handoff，包含：
   - index.html（主頁面）
   - 相關 CSS 樣式
   - 相關 JavaScript 邏輯

正在與 GitHub Repo 的現有資料整合...
```

---

## 第二步：整合 Handoff 與現有 Repo 資料

將 Handoff 的檔案與 Prompt 1 已建立的 Repo 整合，
確認以下整合項目全部正確：

- [ ] `index.html` 中讀取 `data/restaurant.json` 的路徑正確
- [ ] `index.html` 中讀取 `data/menu.json` 的路徑正確（若有菜單）
- [ ] `images/` 資料夾中的照片路徑與 `index.html` 中的引用一致
- [ ] Google Maps embed iframe 使用 `restaurant.json` 的 `embedUrl` 欄位
- [ ] 菜單區塊使用 `menu.json` 的文字資料，非圖片
- [ ] 店家名稱、地址、電話、營業時間均從 JSON 動態載入

若有任何路徑不符或資料未正確載入，請自動修正。

---

## 第三步：補完功能

**3-1. 菜單動態載入**

```javascript
fetch('data/menu.json')
  .then(res => res.json())
  .then(data => renderMenu(data.categories))
  .catch(err => console.error('菜單載入失敗', err));
```

**3-2. 店家資料動態載入**

```javascript
fetch('data/restaurant.json')
  .then(res => res.json())
  .then(data => renderRestaurantInfo(data));
```

**3-3. Google Maps Embed**

```javascript
document.getElementById('google-map').src = data.embedUrl;
```

```html
<div class="map-container">
  <iframe id="google-map" width="100%" height="100%"
    style="border:0;" allowfullscreen="" loading="lazy"
    referrerpolicy="no-referrer-when-downgrade">
  </iframe>
</div>
```

**3-4. 照片輪播（純 JavaScript，不引入外部套件）**

```javascript
let currentSlide = 0;
const slides = document.querySelectorAll('.slide');
function nextSlide() {
  slides[currentSlide].classList.remove('active');
  currentSlide = (currentSlide + 1) % slides.length;
  slides[currentSlide].classList.add('active');
}
setInterval(nextSlide, 4000);
```

---

## 第四步：響應式設計實作

### 目標裝置規格

| 裝置 | 目標機型 | 視窗寬度 | 像素密度 |
|------|---------|---------|---------|
| 手機 | iPhone 13 以後（含 iPhone 13/14/15 全系列） | 390px（邏輯像素） | @2x / @3x |
| 平板 | iPad（一般） | 768px | @2x |
| 桌機 | 一般筆電 / 桌機 | 1280px 以上 | @1x |

### HTML Head 必要設定

```html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0,
        viewport-fit=cover">
  <!-- viewport-fit=cover 支援 iPhone 的 Dynamic Island 與圓角安全區域 -->
  <meta name="theme-color" content="#你的主色">
  <meta name="apple-mobile-web-app-capable" content="yes">
  <meta name="apple-mobile-web-app-status-bar-style" content="default">
</head>
```

### CSS 基本框架

```css
/* ===== Reset ===== */
*, *::before, *::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

/* ===== 字體縮放：手機可讀性優先 ===== */
html {
  font-size: 16px;
  -webkit-text-size-adjust: 100%; /* 防止 iOS 自動放大字體 */
}

/* ===== 安全區域（iPhone 劉海 / Dynamic Island / 底部 Home Bar）===== */
body {
  padding-top: env(safe-area-inset-top);
  padding-bottom: env(safe-area-inset-bottom);
  padding-left: env(safe-area-inset-left);
  padding-right: env(safe-area-inset-right);
}

/* ===== 圖片響應式 ===== */
img {
  max-width: 100%;
  height: auto;
  display: block;
}

/* ===== 斷點設定（Mobile First）===== */

/* 手機：390px（iPhone 13/14/15 標準寬度） */
/* 此為預設樣式，無需 media query */

/* 平板：768px 以上 */
@media (min-width: 768px) {
  /* 平板樣式覆蓋 */
}

/* 桌機：1280px 以上 */
@media (min-width: 1280px) {
  /* 桌機樣式覆蓋 */
}
```

### 各區塊響應式規範

**Hero 區**
```css
.hero {
  min-height: 100svh; /* svh：支援 iOS Safari 動態網址列 */
  background-size: cover;
  background-position: center;
}

/* 手機：標題字體較小 */
.hero h1 { font-size: clamp(1.75rem, 5vw, 3.5rem); }

/* 手機：CTA 按鈕全寬，容易點擊 */
.hero .cta-group {
  display: flex;
  flex-direction: column;
  gap: 12px;
  width: 100%;
}

@media (min-width: 768px) {
  .hero .cta-group {
    flex-direction: row;
    width: auto;
  }
}
```

**菜單區**
```css
/* 手機：單欄 */
.menu-grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 16px;
}

/* 平板：雙欄 */
@media (min-width: 768px) {
  .menu-grid { grid-template-columns: repeat(2, 1fr); }
}

/* 桌機：三欄 */
@media (min-width: 1280px) {
  .menu-grid { grid-template-columns: repeat(3, 1fr); }
}
```

**品項照片卡片**
```css
.dish-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr); /* 手機：兩欄 */
  gap: 12px;
}

@media (min-width: 768px) {
  .dish-grid { grid-template-columns: repeat(3, 1fr); }
}

@media (min-width: 1280px) {
  .dish-grid { grid-template-columns: repeat(4, 1fr); }
}
```

**Google Maps Embed**
```css
.map-container {
  width: 100%;
  height: 300px; /* 手機高度 */
  border-radius: 12px;
  overflow: hidden;
}

@media (min-width: 768px) {
  .map-container { height: 400px; }
}

@media (min-width: 1280px) {
  .map-container { height: 480px; }
}
```

**觸控友善（手機操作體驗）**
```css
/* 所有可點擊元素最小點擊區域 44x44px（Apple HIG 規範） */
a, button, .clickable {
  min-height: 44px;
  min-width: 44px;
  display: inline-flex;
  align-items: center;
  justify-content: center;
}

/* 移除 iOS 按鈕預設樣式 */
button {
  -webkit-appearance: none;
  appearance: none;
  cursor: pointer;
}

/* 防止 iOS 雙擊縮放 */
* { touch-action: manipulation; }
```

### 響應式驗證清單

完成實作後，請逐一確認以下項目（可使用瀏覽器開發者工具模擬）：

**iPhone 13 / 14 / 15（390px）**
- [ ] Hero 圖片填滿全螢幕，不變形
- [ ] 標題文字清晰可讀，不溢出
- [ ] CTA 按鈕容易點擊（高度 ≥ 44px）
- [ ] 菜單以單欄呈現，文字不截斷
- [ ] 營業時間表格不水平溢出
- [ ] Google Maps 地圖正常顯示
- [ ] 頁面不出現水平捲軸

**iPhone 15 Pro Max（430px）**
- [ ] 版面比 390px 版本更寬鬆，但比例正確

**iPad（768px）**
- [ ] 菜單改為雙欄
- [ ] 圖片卡片改為三欄
- [ ] 整體版面有適當留白

**桌機（1280px）**
- [ ] 最大內容寬度限制在 1200px 以內並置中
- [ ] 菜單改為三欄
- [ ] 圖片卡片改為四欄

---

## 第五步：建立 GitHub Pages 自動部署

建立 `.github/workflows/deploy.yml`：

```yaml
name: Deploy to GitHub Pages

on:
  push:
    branches: [ main ]

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: true

jobs:
  deploy:
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/configure-pages@v4
      - uses: actions/upload-pages-artifact@v3
        with:
          path: '.'
      - uses: actions/deploy-pages@v4
        id: deployment
```

---

## 第六步：Push 所有檔案

確認最終專案結構：

```
[REPO_NAME]/
├── .gitignore
├── .github/
│   └── workflows/
│       └── deploy.yml
├── index.html
├── data/
│   ├── restaurant.json
│   └── menu.json
└── images/
    ├── exterior/
    ├── interior/
    ├── dishes/
    └── menu/
```

執行 Push：

```bash
git add .
git commit -m "feat: complete responsive website with Claude Design handoff"
git push origin main
```

---

## 第七步：引導使用者開啟 GitHub Pages

向使用者顯示：

```
🎉 檔案已成功 Push！

現在需要在 GitHub 上開啟 GitHub Pages，請依照以下步驟：

1. 前往 Repo 頁面：
   https://github.com/[GITHUB_USERNAME]/[REPO_NAME]

2. 點選上方「Settings」分頁

3. 左側選單點選「Pages」

4. 在「Build and deployment」區塊：
   Source → 選擇「GitHub Actions」

5. 點選「Save」

完成後請輸入「已開啟」。
```

等待使用者輸入「已開啟」後繼續。

---

## 第八步：確認上線並輸出成果

向使用者顯示：

```
✅ 網站正在部署中！

您的店家網站網址：
👉 https://[GITHUB_USERNAME].github.io/[REPO_NAME]/

首次部署約需 1～2 分鐘，請稍後再前往確認。

💡 測試建議：
   - 電腦：直接用瀏覽器開啟網址
   - iPhone：用 Safari 開啟網址，確認手機版面正常
   - 可使用 Chrome 開發者工具（F12）→ 手機模擬模式確認 390px 版面

之後每次 push 到 main branch，網站就會自動更新。
```

---

## 最終確認清單

請逐項確認並標注 ✅ 或 ❌：

- [ ] Handoff 設計稿已正確整合
- [ ] 店家資料從 restaurant.json 動態載入
- [ ] 菜單從 menu.json 動態載入（以文字呈現）
- [ ] 所有照片路徑正確
- [ ] Google Maps iframe 正常嵌入
- [ ] viewport meta tag 含 viewport-fit=cover
- [ ] safe-area-inset 已套用（支援 iPhone 劉海與 Home Bar）
- [ ] 手機（390px）版面正常，無水平捲軸
- [ ] 所有可點擊元素高度 ≥ 44px
- [ ] 平板（768px）版面正常
- [ ] 桌機（1280px）版面正常
- [ ] GitHub Actions deploy.yml 已建立
- [ ] 所有檔案已 Push 到 main branch
- [ ] GitHub Pages 已由使用者手動開啟
- [ ] 網站網址已提供給使用者

全部 ✅ 後輸出：

```
🎊 完成！

店家名稱：[店家名稱]
網站網址：https://[GITHUB_USERNAME].github.io/[REPO_NAME]/
GitHub Repo：https://github.com/[GITHUB_USERNAME]/[REPO_NAME]

網站同時支援：
✅ 電腦瀏覽器
✅ iPhone 13 / 14 / 15 全系列（含 Pro / Plus / Pro Max）

感謝使用餐廳網站產生器，祝生意興隆！🎉
```

