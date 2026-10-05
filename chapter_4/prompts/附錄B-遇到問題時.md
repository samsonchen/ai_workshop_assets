# 附錄 B：遇到問題時

卡住時，先把「你做了什麼、看到什麼」描述給 Claude Code，多半它能幫你查。下面是這個 Project 常見的狀況。

- **`claude --version` 沒反應 / 找不到指令**：Claude Code 沒裝好或沒在 PATH 裡。回課前安裝清單重裝。

- **bot 連上了，但傳照片沒反應**：最常見是 **Message Content Intent 沒打開**，bot 讀不到附件。回 `docs/discord-bot-setup.md` 把它打開。也確認 `.env` 的 `CHANNEL_ID` 是你實際測試的那個頻道。

- **agent 一啟動就跳錯、說少了套件**：把錯誤訊息整段貼給 Claude Code，請它補。

- **claude -p 很慢或超時**：一張照片要幾十秒是正常的。如果常常失敗，跟 Claude Code 說，請它加上重試。

- **回覆的熱量看起來亂猜**：模型本來就是估計，不是量出來的。這是這個 Project 的限制，不是 bug。

- **token 不小心貼到對話或 commit 上去了**：立刻到 Discord Developer Portal 把那個 bot token **Reset**（作廢重發），再把新的只貼進 `.env`。

- **攻防那步把程式改壞了**：請 Claude Code「把四段防護全部還原成開啟、其他不要動」。

---

回到 [目錄](README.md)
