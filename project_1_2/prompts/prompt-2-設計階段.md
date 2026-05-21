# Prompt 2 — 設計階段
> 工具：Claude Design（Web 版）
> 目標：讀取 GitHub Repo 的資料與照片，設計完整網站，完成後 handoff 給 Claude Code

---

> 以下是從 Claude Code 複製過來的交棒說明範本，
> 實際使用時請將 `[REPO_URL]`、`[店家名稱]`、`[店家類型]` 替換為 Prompt 1 輸出的實際內容。

---

請全程以**繁體中文**輸出所有說明與回應。

請讀取這個 GitHub Repo：`[REPO_URL]`

根據以下資料設計一個完整的店家網站：

**資料來源（Repo 內）**
- `data/restaurant.json` — 店家基本資料、營業時間、地圖 embed URL
- `data/menu.json` — 菜單文字資料（若存在）
- `images/exterior/` — 店面外觀照
- `images/interior/` — 室內環境照
- `images/dishes/` — 招牌品項照
- `DESIGN_BRIEF.md` — 完整設計需求說明

**設計需求**

請依照 `DESIGN_BRIEF.md` 的頁面結構設計以下區塊：

1. **Hero 區**
   - 使用 `images/exterior/` 或 `images/interior/` 的第一張照片作為主視覺背景
   - 疊加店家名稱與一句吸引人的標語
   - 加入「查看菜單」與「找到我們」兩個 CTA 按鈕

2. **關於我們**
   - 店家簡介文字（從 restaurant.json 的 description 欄位）
   - Google 評分與評論數顯示

3. **菜單區**（若 menu.json 存在）
   - 以分類 Tab 或區塊方式呈現
   - 所有文字從 menu.json 動態載入，**不使用菜單照片**
   - 版面清晰易讀，適合手機瀏覽

4. **招牌品項**
   - 使用 `images/dishes/` 的照片做成卡片式展示
   - 每張卡片含圖片與品項名稱

5. **環境介紹**
   - 使用 `images/interior/` 照片做輪播或圖片牆

6. **營業資訊**
   - 從 restaurant.json 讀取每日營業時間，以表格方式呈現
   - 顯示電話與地址

7. **Google Maps 嵌入**
   - 使用 restaurant.json 的 `embedUrl` 欄位嵌入地圖
   - iframe 需 responsive（100% 寬度）

8. **頁腳**
   - 店家名稱、版權聲明
   - 若 restaurant.json 有 website 欄位，加入官網連結

**設計風格要求**
- 請依店家類型（[店家類型]）與實際照片的色調與氛圍，自行判斷合適的配色與排版風格
- 響應式設計，優先考慮手機瀏覽體驗
- 避免過度設計，以清晰實用為主

**技術規格**
- 純 HTML / CSS / JS，單一 `index.html` 檔案
- 照片使用相對路徑（例如 `images/dishes/01.jpg`）
- 菜單資料使用 `fetch('data/menu.json')` 動態載入
- 所有文字內容從 `data/restaurant.json` 讀取，不寫死在 HTML 中

---

設計完成後，請執行 **Handoff to Claude Code**，
並告知使用者：

```
✅ 設計完成！
請按下「Handoff to Claude Code」將設計交給 Claude Code 完成最終實作。
```

