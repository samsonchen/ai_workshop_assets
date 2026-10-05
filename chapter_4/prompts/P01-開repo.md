# P1 在 GitHub 開 repo（你）

這個 Project 不用任何 template，自己開一個空 repo 就好。

1. GitHub → New repository
2. Repository name：`food-photo-agent`（可以自己取）
3. 選 **Private**（裡面會有同學傳的照片和紀錄，不要公開）
4. 勾 Add a README file
5. `.gitignore` template 選 **Python**
6. Create repository

在終端機把 repo 抓下來：

```
gh repo clone <你的 GitHub 帳號>/food-photo-agent
```

```
cd food-photo-agent
```

> 這個 Project 沒有 GitHub Pages、不會對外發佈網頁。用 GitHub 只是為了版本控管，以及確保 `.env`（金鑰）不會被 commit 上去。

---

卡住時：[附錄 B：遇到問題時](附錄B-遇到問題時.md)

下一步：[P2 寫需求說明（你）](P02-寫需求說明.md)
