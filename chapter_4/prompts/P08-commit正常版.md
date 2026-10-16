# P8 commit 正常版（Claude Code）

P7 測完、四段防護都開著，先把這個正常版 commit 起來。P9 攻防會改動程式，有了這個 commit，攻防做完才能一鍵回到這裡。

```
P7 測試都通過了。請：
1. 確認四段防護（防護 1～4）全部都在開啟狀態，沒有任何一段被註解掉
2. 確認 .env 沒有被加進 git
3. 在整個 repo 裡搜尋，確認沒有任何地方寫死了 bot token 或其他不能公開的金鑰
4. 確認 DATA_DIR（照片與 Excel）沒有被 commit 進 git
5. 把程式對照 docs/architecture.md 檢查一遍：架構文件說的行為和程式一致，不一致的地方告訴我
6. commit「food photo agent」並 push
```

commit 完成後，這就是你的正常版。之後 P9 攻防做的所有改動都**不會 commit**。

---

卡住時：[附錄 B：遇到問題時](附錄B-遇到問題時.md)

下一步：[P9 攻防：關掉防護看看（你 + 同學 + 老師）](P09-攻防.md)
