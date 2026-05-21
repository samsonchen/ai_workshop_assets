# Prompt 1 — 資料收集階段
> 工具：Claude Code Desktop
> 目標：從 Google Maps 取得店家資料、收集使用者補充資料與照片，產生結構化資料並 push 到 GitHub

---

請全程以**繁體中文**輸出所有說明、回應與提示訊息。

## 第一步：取得 Google Maps 網址

請向使用者顯示：

```
歡迎使用餐廳網站產生器！

請提供店家的 Google Maps 網址。

取得方式：
1. 在 Google Maps 找到您的店家
2. 點選店家後，複製瀏覽器網址列的完整 URL
   （格式類似：https://www.google.com/maps/place/店家名稱/...）
3. 或直接複製分享連結也可以

請貼上 Google Maps 網址：
```

等待使用者輸入網址後，繼續下一步。

---

## 第二步：從 Google Maps 頁面抓取公開資料

使用 web fetch 抓取使用者提供的 Google Maps URL 頁面內容。

請從頁面中嘗試擷取以下資訊：

| 欄位 | Google Maps 上的位置 |
|------|-------------------|
| 店家名稱 | 頁面標題／h1 |
| 地址 | 地址區塊 |
| 電話 | 聯絡資訊區塊 |
| 營業時間 | 營業時間區塊（含每日） |
| 店家類型 | 類別標籤（餐廳／咖啡廳／烘焙坊等） |
| Google 評分 | 星等數字 |
| 評論數量 | 評論則數 |
| 網站連結 | 官網連結（若有） |

抓取完成後，以表格方式列出成功取得與未能取得的欄位：

```
✅ 成功取得：店家名稱、地址、電話、評分
❌ 未能取得：營業時間、店家類型、網站連結
```

---

## 第三步：互動式補齊缺漏資料

針對第二步**未能取得**的欄位，逐一向使用者詢問。
**每次只問一個欄位，等使用者回答後再問下一個。**
若使用者回答「不知道」或「略過」，該欄位填入空字串，繼續下一個。

詢問範例格式：

```
📋 營業時間
請輸入店家的營業時間，例如：
  週二～週日  11:30–14:00 / 17:30–21:00
  週一公休

（不知道可輸入「略過」）
```

所有欄位補齊後，向使用者顯示完整資料預覽，詢問是否需要修改：

```
以下是目前收集到的店家資料，請確認是否正確：

店家名稱：○○○
地　　址：○○○
電　　話：○○○
營業時間：○○○
店家類型：○○○
Google 評分：○○○（○○○ 則評論）
官方網站：○○○

是否需要修改任何欄位？（請指定欄位名稱與正確內容，或輸入「確認」繼續）
```

等待使用者確認後再繼續。

---

## 第四步：收集照片

向使用者顯示以下說明：

```
📸 請將店家照片上傳到 GitHub Repo

請先在 GitHub 上將照片依類別放入對應資料夾，
完成後再回來繼續：

  images/exterior/　← 店面外觀照（讓訪客認識您的店）
  images/interior/　← 室內環境照（展示用餐氛圍）
  images/dishes/　　← 招牌品項照（吸引客人的主打菜色或飲品）
  images/menu/　　　← 菜單照片（若有實體菜單，請拍清楚每一頁）

上傳方式：
1. 前往 GitHub Repo 頁面
2. 進入對應資料夾（若資料夾不存在，可點「Add file」建立）
3. 點選「Upload files」直接拖曳上傳
4. 每個資料夾上傳完成後 commit

每類至少 1 張，可同時上傳多張。
全部上傳完畢後，請輸入「上傳完畢」繼續。

（若目前沒有照片，輸入「略過」，網站將使用文字為主的版面）
```

等待使用者上傳照片至 GitHub 並輸入「上傳完畢」或「略過」後，
執行 `git pull` 將最新照片同步到本機，再繼續下一步。

---

## 第五步：處理菜單照片

若使用者有上傳菜單照片，針對每張菜單照片執行以下動作：

1. 仔細辨識照片中所有文字，包含：
   - 品項名稱（中文／英文）
   - 價格
   - 品項描述（若有）
   - 分類標題（若有，例如：前菜、主食、飲品）

2. 將辨識結果整理成結構化格式存入 `data/menu.json`：

```json
{
  "categories": [
    {
      "name": "分類名稱",
      "items": [
        {
          "name": "品項名稱",
          "price": "價格",
          "description": "描述（若有）"
        }
      ]
    }
  ]
}
```

3. 向使用者顯示辨識結果，請確認是否有辨識錯誤：

```
📋 菜單辨識結果（第 1 張）：

【前菜】
  招牌沙拉　　NT$180
  季節湯品　　NT$120

【主食】
  泰式打拋豬　NT$220
  ...

如有辨識錯誤，請告知需要修正的品項。
確認無誤請輸入「確認」。
```

等待使用者確認後繼續。

---

## 第六步：產生 Google Maps Embed URL

根據使用者提供的 Google Maps URL，產生可嵌入網站的 embed 連結。

轉換方式：
- 若 URL 含有 `place_id`，使用：
  `https://www.google.com/maps/embed/v1/place?key=&q=place_id:XXXXX`
- 若為一般 Google Maps URL，嘗試轉換為：
  `https://maps.google.com/maps?q=店家名稱+地址&output=embed`
- 或直接在原始 URL 加上 `?output=embed` 參數

將最終可用的 embed URL 存入資料中。

---

## 第七步：整理所有資料並 Push 到 GitHub

詢問使用者 GitHub Repo 名稱：

```
請輸入要建立的 GitHub Repo 名稱
（建議使用英文，例如：my-restaurant 或 cafe-demo）：
```

建立以下專案結構：

```
[REPO_NAME]/
├── .gitignore
├── data/
│   ├── restaurant.json    ← 店家基本資料
│   └── menu.json          ← 菜單資料（若有菜單照片）
├── images/
│   ├── exterior/          ← 店面外觀照
│   ├── interior/          ← 室內環境照
│   ├── dishes/            ← 品項照片
│   └── menu/              ← 原始菜單照片
└── DESIGN_BRIEF.md        ← 給 Claude Design 的設計說明
```

`data/restaurant.json` 格式：

```json
{
  "name": "店家名稱",
  "type": "店家類型",
  "address": "完整地址",
  "phone": "電話",
  "hours": {
    "mon": "公休",
    "tue": "11:30–21:00",
    "wed": "11:30–21:00",
    "thu": "11:30–21:00",
    "fri": "11:30–21:00",
    "sat": "11:00–21:30",
    "sun": "11:00–21:30"
  },
  "rating": 4.5,
  "reviewCount": 128,
  "website": "官網連結或空字串",
  "googleMapsUrl": "原始 Google Maps URL",
  "embedUrl": "Google Maps embed URL",
  "hasMenu": true,
  "photos": {
    "exterior": ["images/exterior/01.jpg"],
    "interior": ["images/interior/01.jpg"],
    "dishes":   ["images/dishes/01.jpg"],
    "menu":     ["images/menu/01.jpg"]
  }
}
```

`.gitignore` 內容：

```
.DS_Store
Thumbs.db
.env
.env.*
node_modules/
.vscode/
.idea/
```

產生 `DESIGN_BRIEF.md`：

```markdown
# Design Brief

## 店家資訊
- 名稱：[店家名稱]
- 類型：[店家類型]
- 風格建議：請依店家類型與照片氛圍判斷

## 照片資產
- 店面外觀：[張數] 張
- 室內環境：[張數] 張
- 招牌品項：[張數] 張
- 菜單照片：[張數] 張（文字已辨識存於 menu.json）

## 頁面結構需求
1. Hero 區（店家名稱、標語、主視覺照片）
2. 關於我們（店家簡介、評分）
3. 菜單區（從 menu.json 以文字呈現，非圖片）
4. 品項展示（招牌品項照片）
5. 環境介紹（室內照片輪播）
6. 營業資訊（時間、電話、地址）
7. Google Maps 嵌入地圖
8. 頁腳

## 技術規格
- 純 HTML / CSS / JS（無需框架，方便 GitHub Pages 直接 hosting）
- 響應式設計（mobile-first）
- 照片資料來源：images/ 資料夾
- 菜單資料來源：data/menu.json（以 JavaScript 動態載入）
- 地圖嵌入：data/restaurant.json 的 embedUrl 欄位

## 部署目標
- GitHub Pages
- Repo：[REPO_NAME]
```

全部建立完成後執行：

```bash
git add .gitignore data/ images/ DESIGN_BRIEF.md
git commit -m "feat: add restaurant data, photos and design brief"
git push origin main
```

完成後以繁體中文輸出：
- Repo GitHub URL
- `data/restaurant.json` Raw URL
- `DESIGN_BRIEF.md` Raw URL
- 照片統計（各類別幾張）
- 菜單辨識是否完成
- 下一步提示：「請複製以下說明，開啟 Claude Design 貼入」

並自動產生給 Claude Design 的交棒說明（見 Prompt 2 開頭格式）。

