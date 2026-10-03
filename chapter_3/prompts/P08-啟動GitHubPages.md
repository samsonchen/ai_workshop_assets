# P8 啟動 GitHub Pages 看前端（你）

照 Claude Code 在上一步告訴你的設定做。一般是：

1. repo → Settings → Pages
2. Build and deployment 的 Source 選 GitHub Actions
3. 到 Actions 分頁，等部署跑完（綠色勾勾）
4. 第一次如果在設定 Source 之前就跑而失敗，按 Re-run all jobs
5. 打開 `https://<你的 GitHub 帳號>.github.io/anonymous-chat/`

確認你看到的效果跟架構文件「階段一」寫的一致。

頁面打不開或是空白，回到 Claude Code：

```
GitHub Pages 的網址 ___ 打開是空白的。Actions 的結果是 ___。請找出原因。
```

---

卡住時：[附錄 B：遇到問題時](附錄B-遇到問題時.md)

下一步：[P9 接上 Supabase（你 + Claude Code）](P09-接上Supabase.md)
