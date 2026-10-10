# Skills

Skill 是一份寫給 Claude 的工作說明。裡面寫好某一類工作要怎麼做、要注意什麼。裝好之後，Claude 遇到符合的工作會自己載入那份說明，你不用每次重新交代一遍。

在 Claude Desktop 或 claude.ai 裝的 skill，Claude Code 也讀得到。反過來不會同步：在 Claude Code 用指令裝的 skill 只留在這台電腦上。兩邊都要用，就在 Claude Desktop 裝。

## 這個資料夾裡的 skill

| 檔案 | 用途 |
|---|---|
| `diagram.skill` | 產生 Mermaid 圖：流程圖、序列圖、ER 圖、架構圖、心智圖等 |
| `obsidian-knowledge-wiki.skill` | 把一個主題整理成一組互相連結的 Obsidian 筆記 |
| `notebooklm-knowledge-wiki.skill` | 用 NotebookLM 做研究，再整理成 Obsidian 筆記，可加簡報、語音導讀、測驗 |
| `samson-voice.skill` | 用平實、不誇大、不編造事實的語氣寫文件或潤稿以模擬 Samson 的口氣 (選配) |

安裝：下載 `.skill` 檔，在 Claude Desktop ▸ Settings ▸ Customize 裡上傳。

## 從 Marketplace 安裝 Skills

路徑：Claude Desktop ▸ Settings ▸ Customize。

先加 marketplace，再從裡面安裝 plugin。下面的文字直接複製貼上，不用照著投影片打。

### Mattpocock skills

裡面的 grilling 會一路追問你的設計與計畫，把沒想清楚的地方問出來。

Marketplace：

```
mattpocock/skills
```

Plugin：

```
mattpocock-skills
```

### Superpowers

一整組開發流程的 skill：brainstorming、test-driven-development、systematic-debugging 等。

Marketplace：

```
obra/superpowers-marketplace
```

Plugin：

```
superpowers
```

### Frontend design

官方的前端設計 skill，裡面是 frontend-design。下一個 project 會用到。

Marketplace：

```
anthropics/claude-plugins-official
```

Plugin：

```
frontend-design
```

## 其他安裝方式

在 Claude Code 裡用 `/plugin` 指令安裝的方式，請參考 [Chapter 2 投影片](../slides/Agentic-AI-Eng-Workshop-Ch2-Hands-on.pdf) 第 3 頁。
