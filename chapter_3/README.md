# 匿名聊天室：實作前導覽

這個 Project 從你已經知道的東西開始，你還不知道的部分交給 Claude Code 補上。架構文件 `docs/architecture.md` 要在流程中由你和 Claude Code 一起寫出來，不會事先給你。

這一頁是動手前講解用的圖。實際操作的步驟在 [prompts/](prompts/README.md)，一步一個檔案。

## 1. 系統裡有哪些角色

```mermaid
flowchart TB
    pc["電腦瀏覽器"]
    phone["手機瀏覽器"]

    subgraph supa["Supabase"]
        rt["Realtime<br/>WebSocket 即時連線"]
        db[("Database<br/>保存最近的訊息")]
        bc["Broadcast<br/>把訊息轉給所有人"]
        pr["Presence<br/>誰在線上"]
        rt --- bc
        rt --- pr
    end

    pc <-->|"wss"| rt
    phone <-->|"wss"| rt
    pc -.-> db
    phone -.-> db

    classDef browser fill:#fff3cd,stroke:#b8860b,color:#333
    classDef rtNode fill:#e1f5ff,stroke:#0288d1,color:#333
    classDef dbNode fill:#eef2f7,stroke:#6b7a94,color:#333
    class pc,phone browser
    class rt,bc,pr rtNode
    class db dbNode
```

- 電腦和手機的瀏覽器打開同一個網頁，各自跟 Supabase 的 Realtime 建立一條 WebSocket 連線（`wss`，加密的即時連線）。
- Broadcast：一個人送出訊息，Realtime 轉給所有連著的人。
- Presence：記錄誰在線上，有人進來或離開時通知所有人。
- Database：保存最近的訊息，新進來的人可以看到之前的訊息。瀏覽器直接讀寫，不經過 Realtime。要不要用、保存多少則，看你在 P4 寫出來的架構文件，所以圖上用虛線。

## 2. 做完之後放在哪裡

上一張圖只畫了執行時的角色。這張加上你的電腦和 GitHub：網頁放在 GitHub Pages，每個人用自己的瀏覽器打開。訊息不經過 GitHub，由 Supabase 轉給其他人。

```mermaid
flowchart LR
    subgraph dev["你的電腦"]
        cc["Claude Code"]
        code["anonymous-chat<br/>程式與文件"]
        envLocal["本機金鑰檔<br/>不 commit"]
        cc --> code
        envLocal -.-> code
    end

    subgraph gh["GitHub"]
        repo["repo<br/>anonymous-chat"]
        actions["Actions<br/>自動 build 與部署"]
        secret["GitHub 上的金鑰設定"]
        pages["GitHub Pages<br/>公開網址"]
        repo --> actions --> pages
        secret -.-> actions
    end

    subgraph users["使用者"]
        a["A 的瀏覽器<br/>電腦"]
        b["B 的瀏覽器<br/>手機"]
    end

    supa[("Supabase<br/>即時通訊 WebSocket")]

    code -->|"git push"| repo
    pages -->|"載入網頁"| a
    pages -->|"載入網頁"| b
    a <-->|"送出 / 收到訊息"| supa
    b <-->|"送出 / 收到訊息"| supa

    classDef local fill:#fff3cd,stroke:#b8860b,color:#333
    classDef cloud fill:#eef2f7,stroke:#6b7a94,color:#333
    classDef secretNode fill:#ffebee,stroke:#c62828,color:#333
    class cc,code local
    class repo,actions,pages,supa,a,b cloud
    class envLocal,secret secretNode
```

紅色是金鑰。哪一把可以放在前端、哪一把不能出現在任何地方，寫在架構文件的「金鑰」那一節。

## 3. 分兩個階段做

先做不連 Supabase 的版本，畫面和操作都確認過，再把「通訊」換成 Supabase。畫面的程式不用重寫。

```mermaid
flowchart LR
    subgraph s1["階段一：P6～P8"]
        ui1["畫面<br/>輸入代號、聊天、線上名單"]
        com1["通訊<br/>不連外"]
        ui1 <--> com1
    end

    subgraph s2["階段二：P9"]
        ui2["畫面<br/>同一份程式"]
        com2["通訊<br/>改接 Supabase"]
        ui2 <--> com2
        com2 <--> supa2[("Supabase")]
    end

    s1 ==>|"只換通訊"| s2

    classDef same fill:#eef2f7,stroke:#6b7a94,color:#333
    classDef changed fill:#fff3cd,stroke:#b8860b,stroke-width:2px,color:#333
    class ui1,ui2,com1 same
    class com2,supa2 changed
```

## 4. 一則訊息怎麼從 A 到 B

P4-3 檢查清單的第一題：「訊息從我的瀏覽器送出後，經過哪裡、怎麼到別人的瀏覽器？」可以對照這張圖回答。

```mermaid
sequenceDiagram
    autonumber
    participant A as A 的瀏覽器
    participant S as Supabase Realtime
    participant B as B 的瀏覽器

    A->>S: 用代號進入聊天室
    B->>S: 用代號進入聊天室
    S-->>A: 線上名單更新
    S-->>B: 線上名單更新
    A->>S: 送出訊息
    S-->>B: 轉送訊息
    Note over B: 先檢查內容<br/>當成純文字顯示，不當 HTML
    S-->>A: 轉送訊息
    Note over A,B: 斷線時顯示「重新連線中」，連回來自動補上
```

## 5. 整個步驟的流程

四個階段由左到右，每個階段裡由上往下。一步做完再進下一步。黃色是你做，藍色是 Claude Code 做，綠色是你和 Claude Code 一起做。

```mermaid
flowchart LR
    subgraph prep["準備"]
        direction TB
        p0["P0 開始前<br/>裝好工具、確認 gh 權限"]
        p1["P1 在 GitHub 開 repo"]
        p2["P2 寫需求說明<br/>docs/requirements.md"]
        p3["P3 啟動 Claude Code"]
        p0 --> p1 --> p2 --> p3
    end

    subgraph plan["規劃與設計"]
        direction TB
        p41["P4-1 Claude Code 問問題<br/>grilling"]
        p42["P4-2 Claude Code<br/>寫架構文件"]
        p43{"P4-3<br/>你看得懂嗎？"}
        p44["P4-4 CLAUDE.md<br/>+ commit"]
        p5["P5 /design 視覺設計<br/>docs/design.md"]
        p41 --> p42 --> p43
        p43 -->|"看不懂 / 要改"| p42
        p43 -->|"確認"| p44 --> p5
    end

    subgraph stage1["階段一：只有前端"]
        direction TB
        p6["P6 /frontend-design<br/>做前端"]
        p6t{"本機測試<br/>OK？"}
        p7["P7 commit / push"]
        p8{"P8 GitHub Pages<br/>看得到嗎？"}
        p6 --> p6t
        p6t -->|"有問題"| p6
        p6t -->|"OK"| p7 --> p8
        p8 -->|"空白 / 失敗"| p7
    end

    subgraph stage2["階段二：接上 Supabase"]
        direction TB
        p91["P9-1 Claude Code<br/>說明要設定什麼"]
        p92["P9-2 你在 Supabase<br/>網頁上操作、拿金鑰"]
        p93["P9-3 Claude Code<br/>把前端接上"]
        p94{"P9-4 兩個瀏覽器<br/>互傳 OK？"}
        p95["P9-5 檢查金鑰<br/>commit、push"]
        p10["P10 收尾<br/>文件與程式對一遍"]
        p91 --> p92 --> p93 --> p94
        p94 -->|"有問題"| p93
        p94 -->|"OK"| p95 --> p10
    end

    prep ==> plan ==> stage1 ==> stage2

    classDef you fill:#fff3cd,stroke:#b8860b,color:#333
    classDef claude fill:#e1f5ff,stroke:#0288d1,color:#333
    classDef both fill:#e8f5e9,stroke:#2e7d32,color:#333
    class p0,p1,p2,p3,p43,p6t,p8,p92,p94 you
    class p42,p44,p6,p7,p91,p93,p95,p10 claude
    class p41,p5 both
```

## 6. 文件怎麼一路傳下去

每一步的產出都存成 repo 裡的檔案，下一步請 Claude Code 讀這些檔案。換電腦或重開 Claude Code，都可以從檔案接著做。

```mermaid
flowchart LR
    req["docs/requirements.md<br/>你寫的需求"]
    arch["docs/architecture.md<br/>架構文件"]
    claudemd["CLAUDE.md<br/>給 Claude Code 的守則"]
    design["docs/design.md<br/>視覺設計"]
    fe["前端程式<br/>階段一"]
    fe2["前端程式<br/>階段二"]

    req -->|"P4 grilling"| arch
    arch -->|"P4-4"| claudemd
    arch -->|"畫面清單"| design
    arch --> fe
    design --> fe
    fe -->|"P9 只換通訊"| fe2
    arch --> fe2
    fe2 -.->|"P10 對照"| arch

    classDef doc fill:#fff3cd,stroke:#b8860b,color:#333
    classDef code fill:#eef2f7,stroke:#6b7a94,color:#333
    class req,arch,claudemd,design doc
    class fe,fe2 code
```

## 開始動手

從 [prompts/README.md](prompts/README.md) 的 P0 開始。
