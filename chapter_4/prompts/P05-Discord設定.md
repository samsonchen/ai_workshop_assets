# P5 產生 Discord 設定文件並照做（你 + Claude Code）

你需要自己的 Discord Server、一個 bot、以及 bot 的 token。這些步驟因為每個人帳號畫面不同、而且 Discord 網頁會改版，所以**讓 Claude Code 針對你的情況現場產生步驟文件**，你再照著做。

## 5-1 請 Claude Code 產生設定文件

```
我要幫這個 agent 設定 Discord。請先不要動程式，幫我產生兩份步驟文件，放在 docs/ 底下，用我看得懂的中文、一步一個動作、寫到我知道要點畫面的哪裡：

1. docs/discord-server-setup.md
   - 怎麼建立一個我自己的 Discord Server
   - 怎麼建立一個頻道給這個 agent 用，以及怎麼拿到它的 CHANNEL_ID
   - 怎麼產生邀請連結，讓同組同學加入我的 Server（後面測試要用）

2. docs/discord-bot-setup.md
   - 怎麼在 Discord Developer Portal 建立一個 application / bot
   - 一定要打開 Message Content Intent（要說明為什麼）
   - 怎麼拿到 bot token
   - 用哪個權限、怎麼把 bot 邀進我自己的 Server
   - 特別標明：bot token 絕對不能貼給任何人、不能貼進這個對話、不能 commit

我的電腦是（macOS／Windows，擇一告訴它）。
```

## 5-2 照步驟操作（你）

照 Claude Code 列的步驟做。要登入或授權的步驟一定是你自己在瀏覽器做。

拿到 **CHANNEL_ID** 和 **bot token** 之後：

- 在 repo 裡建立 `.env`，自己把這兩個值貼進去，連同 `CLAUDE_MODEL` 和 `DATA_DIR`。
- **bot token 只貼進 `.env` 這個檔案，不要貼進 Claude Code 的對話，也不要貼到任何別的地方。**
- 先確認 `.env` 已經在 `.gitignore` 裡。

`.env` 大概長這樣（值自己填）：

```
DISCORD_TOKEN=你的bot_token
CHANNEL_ID=你的頻道ID
CLAUDE_MODEL=haiku
DATA_DIR=./data
```

## 5-3 跟同組同學互加 Server（你）

用 5-1 產生的邀請連結，和同組同學互相加入對方的 Server。P7、P8 需要「你以外的人」也能傳照片進來。

---

卡住時：[附錄 B：遇到問題時](附錄B-遇到問題時.md)

下一步：[P6 做出 agent（Claude Code）](P06-做出Agent.md)
