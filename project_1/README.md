# Project 1

## Original Design Prompts

```text
你是一個專業的網頁平面設計師, menu 是一個餐廳的手寫菜單, 為考慮觀光客的使用，要將此 menu 製做成以
以手機瀏覽器為主的畫面設計，由於只給手機使用，不需要考慮手機以外的瀏覽器如電腦或平板。
配色以目前工作區內的色彩元素為基準。
畫面的最一開始要有一個區塊寫明店名搭配形象圖片。
線上菜單要符合國際觀光客使用，應包含「繁體中文」「英文」「日文」「韓文」四種語言。
語言切換按鈕應該頁面還未捲動時就能看到的位置。
畫面一次只需顯示一種語言，但可以讓使用者以點擊切換至其它語言的頁面。
同時此菜單也具備可以顯示每道菜的照片以及價格，如果照片尚未拍攝，以 emoji 圖案代替。
同樣的品項如果有分大小，應使用同一品項但標明大與小，杯與壺，整尾半尾的價格。
部份的菜有特別標示，應以小圖示帶文字顯示在該品項項目，這些小圖示的文字同樣是多國語言與使用者選用的語言相同來顯示，
特別標示包含底下項目：
 - 本店推薦
 - 小辣
 - 大辣
 - 含有豬肉
 - 含有牛肉
 - 素食
 - 清真認證

品項分為底下幾大類來規類，相同的類別的品項要放在同一區塊：
 - 主餐類
 - 單點類
 - 甜點類
 - 飲料類

每一個品項還可以獨立點選，使用者點下後會開另一個頁面，透過 Overlay 方式顯示，該頁面的照片顯示面積更大，並且允許使用者縮放。該頁面關閉時不需要重新載入主頁面。
點入後除了原本的基本資訊外，同時還具備附注項目，如果該品項有需要特別說明可以寫在內，或是可以用更多的文字對該品項做說明。
頁面的一開頭需標明用餐時長與基本低消，這也需要以多國語言標示。

品項資料將以 js 格式如下：

``` js
// ═══════════════════════════════════════════════════════════════════════════
//  菜單項目設定 — 餐廳業者請在此處修改菜單
//
//  每個項目的欄位說明：
//    names  多語言名稱，必填 { zh: 中文, en: 英文, ja: 日文, ko: 韓文 }
//    price  價格字串陣列，例如不分大小時 ['NT$350']；若為時價填 ['market']；無標價填 null；若有分大小時 ['大': 'NT$400', '小': 'NT$200']
//    image  圖片檔名，例如 'hsf-1.jpg'，圖片放在 public/images/ 資料夾
//           若尚未有圖片，填 null，系統會顯示下方 emoji 代替
//    emoji  無圖片時顯示的圖示（圖片備用）
//    tags   標記陣列，填入標記字母，留空填 []
//           R 推薦   1 小辣  2 中辣  3 大辣
//           P 含豬肉  B 含牛肉  L 含羊肉  V 素菜
//    desc   多語言餐點說明 { zh, en, ja, ko }，不需要填寫時留空字串 ''
//    note   多語言備註    { zh, en, ja, ko }，不需要填寫時留空字串 ''
//
//  新增項目範例：
//    { names: { zh: '蒜炒螃蟹', en: 'Garlic Crab', ja: 'ニンニクカニ', ko: '마늘게' },
//      price: 'NT$480', image: 'crab-garlic.jpg', emoji: '🦀',
//      tags: ['R'],
//      desc: { zh: '', en: '', ja: '', ko: '' },
//      note: { zh: '', en: '', ja: '', ko: '' } }
// ═══════════════════════════════════════════════════════════════════════════

const _  = { zh: '', en: '', ja: '', ko: '' }  // empty multilingual field shorthand

// ── 海鮮類 ──────────────────────────────────────────────────────────────────
export const seafoodItems = [
  {
    names: { zh: '三杯透抽',     en: 'Squid · Three Cup',      ja: 'イカ·三杯炒め',         ko: '오징어·삼배'      },
    price: ['NT$400'],      image: '三杯透抽.jpeg', emoji: '🦑', tags: ['R'],
    desc: _, note: _,
  }
]
```

Claude Asked:

```text
JS 格式問題

price 欄位定義前後矛盾——註解裡寫了三種格式：字串陣列 ['NT$350']、分大小時用物件語法 ['大': 'NT$400', '小': 'NT$200']（這不是合法 JS），但範例裡又用了 'NT$480'（純字串）和 ['NT$400']（陣列）。需要統一。那個 ['大': 'NT$400'] 語法在 JS 裡根本跑不了，陣列裡不能用 key-value。
品項類別沒有在資料結構裡體現——你 prompt 裡定義了主餐類、單點類、甜點類、飲料類四個分類，但 JS 範例裡 export 的是 seafoodItems（海鮮類），這跟你定義的四大分類對不上。設計師不會知道哪些品項屬於哪個類別。
market 和 null 的語意沒有在 UI 層說明——時價要顯示什麼文字？null 無標價的品項價格欄要怎麼呈現？

建議的修正方案：
js// price 欄位格式統一為陣列，每個元素是 { label, value }
//   不分大小：  [{ label: null, value: 'NT$350' }]
//   分大小：    [{ label: '大', value: 'NT$510' }, { label: '小', value: 'NT$270' }]
//   分主食選擇：[{ label: '烤雞腿', value: 'NT$270' }, { label: '牛肉', value: 'NT$340' }, { label: '蔬食', value: 'NT$250' }]
//   時價：      [{ label: null, value: 'market' }]  → 顯示「時價/Market Price/時価/시가」
//   無標價：    null

// category 欄位：'main' 主餐類 | 'side' 單點類 | 'dessert' 甜點類 | 'drink' 飲料類
然後範例改成：
jsconst _ = { zh: '', en: '', ja: '', ko: '' }

export const menuItems = [
  {
    category: 'main',
    names: { zh: '手作泰式紅咖哩（小辣）', en: 'Thai Red Curry', ja: 'タイレッドカレー', ko: '태국 레드커리' },
    price: [
      { label: { zh: '烤雞腿', en: 'Chicken', ja: 'チキン', ko: '치킨' }, value: 'NT$270' },
      { label: { zh: '牛肉',   en: 'Beef',    ja: '牛肉',   ko: '소고기' }, value: 'NT$340' },
      { label: { zh: '蔬食',   en: 'Veggie',  ja: 'ベジ',   ko: '채식' }, value: 'NT$250' },
    ],
    image: null, emoji: '🍛', tags: ['R', '1', 'B'],
    desc: _, note: _,
  },
  {
    category: 'drink',
    names: { zh: '泰式奶茶', en: 'Thai Milk Tea', ja: 'タイミルクティー', ko: '태국 밀크티' },
    price: [
      { label: { zh: '杯', en: 'Cup',  ja: 'カップ', ko: '컵' }, value: 'NT$70' },
      { label: { zh: '壺', en: 'Pot',  ja: 'ポット', ko: '포트' }, value: 'NT$160' },
    ],
    image: null, emoji: '🧋', tags: [],
    desc: _, note: _,
  },
]
其他建議

label 也做成多語言物件，這樣「大/小」「杯/壺」「整尾/半尾」都能跟著語言切換翻譯
把所有品項放同一個 menuItems 陣列，用 category 欄位分類，比分散成多個 export 變數更容易維護
標籤裡你原本 prompt 寫了「中辣」但菜單上沒看到中辣的品項，確認一下是否需要保留
你 prompt 裡有「清真認證」標籤但 tags 定義裡沒有對應字母，要補上（例如 H for Halal）
```

My answers:

```text
這樣的 JS 如果用人工補上項目，會很不容易作業嗎? 請幫我改成對人類友善的作業方式.
```

## Original Claude Code Prompts

--> Attach the Pencil design file and the design prompts. <--

```text
@menu.pen 是香茅廚房的線上菜單， ＠menu-design-prompt.md 是提供給 pencil.dev UI 設計的 prompt 做為參考。
請使用 TypeScript 執行這個 UI 的設計, 其中 Mobile Menu 是主頁面，而 Item Detail Modal 是點擊後頁面。
請設計成 Static 頁面，並且透過這個 Repository 將用於 GitHub page 之上。
Repository 應具備 workflow, 之後在內容變更後，可以自動將 Push 上 GitHub 的內容更新到 GitHub Page 之中。
```

Claude Asked

```text
1. 沒指定技術棧——「TypeScript 的 Static 頁面」有很多種做法：純 TS 編譯成 JS、React + Vite、Next.js static export、Astro、甚至手寫 HTML + TS。你需要明確指定，不然 Claude Code 會自己挑一個你不一定想要的。
2. GitHub repo 資訊完全沒有——repo 名稱？是新建還是已有？GitHub 帳號/org？GitHub Pages 用的是 gh-pages branch 還是 docs/ 資料夾？還是用 GitHub Actions deploy？
3. menu-raw.js 的角色不清楚——是要直接把資料嵌在 static 頁面裡？還是 build 時讀取？翻譯怎麼處理？是你先手動準備好四語言的完整資料，還是要 Claude Code 在 build 時呼叫 AI API 翻譯？
4. @menu.pen 是什麼——Claude Code 不一定知道 .pen 是 Pencil 的設計稿格式，也不知道怎麼從中提取設計 token（顏色、字體、間距）。需要說明是要它參考設計稿的截圖，還是你會另外提供色碼等規格。
5. 圖片資源的處理——目前所有 image 都是 null，那 emoji fallback 的顯示邏輯要寫在哪？之後圖片放哪個路徑？
6. 沒有提到建置與開發指令——npm run dev 本地預覽？npm run build 輸出到哪？
```

My answers:

```text
1. 請幫我挑一個最適合用在 GitHub Page 的技術, 如果都一樣, 那我會選擇, React+vite
2. 這個已經有了, 將會在該 respotiry 內執行 Claude Code
3. 在 build 中讀取
4. @menu.pen 是 Pencil.dev 的產出檔, 將會一併提供，並且有 MCP 讀取
5. 圖片資源都會放在 public/images 內，包含目前還沒放但未來新放入的
6. npm run dev 本地預覽OK但npm build要可以透過 GitHub Pages 從外部讀取到，如果需要，請一併將 GitHub 上面的設定寫出
請幫我寫出新的 prompt
```

## Confirm the steps

- GitHub MCP
- Github Personal Access Token
- Create new repository
  - PAT Select repository
- Github pages
  - Source: GitHub Actions
- If GitHub action errors, work with Claude to find out the error

## Additional Commands to try:

- /install-github-app
- Claude Code Remote