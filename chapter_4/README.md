# 食物照片記錄 Agent：實作前導覽

這個 Project 做一個在你電腦上執行的 agent：只要它開著，Discord 頻道裡任何人傳一張食物照片進來，它就用 `claude -p` 判斷、估熱量、建檔，再回覆上傳者。

這一頁是動手前講解用的圖。實際操作的步驟在 [prompts/](prompts/README.md)，一步一個檔案。

和前一個 Project 不同的地方：

- 沒有 GitHub Pages、沒有前端網頁。成果是一支在**你自己電腦上跑**的程式，關掉就不處理。
- 要處理的照片經過 `claude -p`。agent 會把「使用者傳進來的東西」交給模型，這就帶出一個新主題：**別人傳進來的內容，可不可以當成指令？** 這是這個 Project 後半的重點。

## 1. 系統裡有哪些角色

```mermaid
flowchart TB
    user["頻道裡的任何人<br/>手機 / 電腦 Discord"]

    subgraph dc["Discord"]
        ch["你指定的頻道<br/>CHANNEL_ID"]
    end

    subgraph pc["你的電腦（agent 開著才會運作）"]
        agent["agent 程式<br/>discord bot"]
        cli["claude -p<br/>看照片、估熱量"]
        data[("DATA_DIR<br/>Excel 紀錄 + 照片檔")]
        agent --> cli
        agent --> data
    end

    user -->|"上傳食物照片"| ch
    ch <-->|"WebSocket（Gateway）"| agent
    agent -->|"回覆結果"| ch

    classDef you fill:#fff3cd,stroke:#b8860b,color:#333
    classDef cloud fill:#eef2f7,stroke:#6b7a94,color:#333
    classDef cli fill:#e1f5ff,stroke:#0288d1,color:#333
    class agent,data you
    class user,ch cloud
    class cli cli
```

- 每個人開自己的 Discord Server，把 agent 接到自己 Server 的一個頻道（`CHANNEL_ID`）。
- agent 用 Gateway 模式連 Discord（對外的 WebSocket，不需要 ngrok，也不用對外開埠）。
- 照片、Excel 紀錄都存在**你電腦上的 `DATA_DIR`**，不上傳到任何雲端。
- 每處理一張照片就花掉你自己的 Claude 額度。agent 一關，頻道裡再怎麼傳都不會有反應。

## 2. 一張照片怎麼被處理

```mermaid
sequenceDiagram
    autonumber
    participant U as 上傳者
    participant C as Discord 頻道
    participant A as agent（你的電腦）
    participant P as claude -p
    participant D as DATA_DIR

    U->>C: 傳一張食物照片
    C-->>A: 收到有附件的訊息
    A->>C: 先加上 ⏳（排隊中）
    A->>A: 一張一張排隊處理
    A->>P: 把照片交給 claude -p 分析
    P-->>A: 回傳固定格式 JSON<br/>（是否食物 / 成份 / 重量 / 熱量）
    A->>A: 檢查 JSON、清洗檔名、算當日總熱量
    A->>D: 寫入 Excel 一列 + 存照片檔
    A->>C: 回覆上傳者：這餐的結果 + 當日已攝取總熱量
```

- 好幾個人同時傳，照片會**排隊一張一張處理**，先加 ⏳ 讓上傳者知道收到了。
- 模型回來的東西 agent **不直接相信**：要先確認格式對、數字是數字，才寫檔、才回覆。
- 「當日已攝取熱量」是把當天這個頻道處理過的所有熱量加起來（這一版不分人；要分人是作業）。
- 如果判定**不是食物**，一樣建檔、一樣回覆，只是食物名稱那欄寫「不是食物」，成份欄寫它到底是什麼（例如「馬克杯」）。

## 3. 設定從哪裡來

```mermaid
flowchart LR
    env["本機 .env（不 commit）"]
    subgraph keys[" "]
        tok["DISCORD_TOKEN<br/>絕對不能公開"]
        cid["CHANNEL_ID<br/>要聽哪個頻道"]
        mdl["CLAUDE_MODEL<br/>haiku / sonnet…"]
        dir["DATA_DIR<br/>紀錄與照片放哪"]
    end
    env --- tok
    env --- cid
    env --- mdl
    env --- dir

    classDef secret fill:#ffebee,stroke:#c62828,color:#333
    classDef cfg fill:#fff3cd,stroke:#b8860b,color:#333
    class tok secret
    class cid,mdl,dir cfg
```

- `DISCORD_TOKEN` 是你 Server 的 bot 金鑰，拿到別人手上就能冒用你的 bot，**不能貼進任何對話、不能 commit**。
- `CLAUDE_MODEL` 可以換，課堂後半會讓你拿不同 model 各打一次做對照。
- 這些都放 `.env`，`.env` 要在 `.gitignore` 裡。

## 4. 後半段在練什麼：別人傳的東西能不能當指令

照片和上傳者打的字，都是「別人送進來的內容」。agent 把這些交給 `claude -p` 的時候，如果程式寫得不好，別人就能用一張照片或一句話**改變 agent 的行為**——這叫 prompt injection。

這個 Project 的 agent 會把幾類防護**各自包成一段**、標好開始與結束。前半段先把防護全部做好、確認正常運作；後半段（P8）老師會帶你**一段一段關掉**防護，看 agent 被影響成什麼樣子，再打開。目的是讓你親眼看到：

> 安全不能靠「模型夠不夠聰明」，要靠程式有沒有把「別人送進來的資料」和「給模型的指令」分開。

```mermaid
flowchart LR
    subgraph on["防護開著"]
        a1["別人送的內容<br/>當成『資料』"]
        a2["照片 / 字句"]
        a2 --> a1 --> ok["照常分析、照常建檔"]
    end

    subgraph off["防護關掉（P8 對照用）"]
        b1["別人送的內容<br/>被當成『指令』"]
        b2["照片 / 字句"]
        b2 --> b1 --> bad["行為被改掉：<br/>熱量被竄改、檔案被亂翻…"]
    end

    on ==>|"P8：關掉一段看看"| off

    classDef good fill:#e8f5e9,stroke:#2e7d32,color:#333
    classDef bad fill:#ffebee,stroke:#c62828,color:#333
    class a1,a2,ok good
    class b1,b2,bad bad
```

這張圖只講「會差很多」，**怎麼差、怎麼打**留到 P8 由老師帶。

## 開始動手

從 [prompts/README.md](prompts/README.md) 的 P0 開始。
