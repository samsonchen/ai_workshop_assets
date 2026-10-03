# P9 接上 Supabase（你 + Claude Code）

## 9-1 請 Claude Code 說明要做什麼

```
現在要進入 @docs/architecture.md 的「階段二」，接上 Supabase。

我不知道 Supabase 要怎麼設定。請先不要動程式，告訴我：
1. Supabase 那邊要建立什麼、設定什麼
2. 不要使用 Supabase CLI
3. 哪些步驟你可以用指令幫我做，哪些需要我自己在網頁上操作
4. 需要我自己操作的，請一步一步寫給我，寫到我知道要點哪裡
5. 我的電腦是（macOS／Windows）
```

## 9-2 照步驟操作（你）

照 Claude Code 列的步驟做。要登入或授權的步驟（例如在瀏覽器登入 Supabase）一定是你自己做。

拿到金鑰之後，回去對照架構文件「金鑰」那一節：哪一把可以用、哪一把不能用。不能公開的那一把，不要貼到 Claude Code 的對話裡，也不要貼到任何地方。

## 9-3 讓 Claude Code 把前端接上（Claude Code）

```
Supabase 那邊已經設定好了。請依照 docs/architecture.md 的「階段二」把前端接上 Supabase：

1. 金鑰照架構文件「金鑰」那一節的方式存放，告訴我你放在哪個檔案、為什麼那個檔案不會被 commit
2. GitHub Pages 部署時需要的金鑰設定，能用指令做的你做，需要我在 GitHub 網頁上做的告訴我步驟
3. 只修改架構文件說要換掉的部分
4. 做完確認可以 build，啟動本機預覽

先不要 commit。
```

## 9-4 本機測試（你）

用兩個不同的瀏覽器（或一般視窗加無痕視窗）測試，對照架構文件「驗證清單」的階段二。

## 9-5 上線（Claude Code）

```
本機測試沒問題。請：
1. 確認金鑰檔案沒有被加進 git
2. 在整個 repo 裡搜尋，確認沒有任何不能公開的金鑰
3. 把 CLAUDE.md 的「目前階段」改成階段二
4. commit「connect Supabase」並 push
```

等 Actions 跑完，用電腦和手機同時打開 GitHub Pages 網址，互傳訊息。

---

卡住時：[附錄 B：遇到問題時](附錄B-遇到問題時.md)

下一步：[P10 收尾（Claude Code）](P10-收尾.md)
