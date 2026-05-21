# Interactive Prompts to make a website of any public commercial place

## Prompts

1. Prompt 1 is for Claude Code Desktop
2. Prompt 2 is for Claude Design
3. Prompt 3 is for Claude Code Desktop again

## Notes

- All images files shall be uploaded to GitHub repo directly. Do not let Claude Code to upload the images.
- Make sure images are small enough to be stored on GitHub

## 網站產生器流程

```mermaid
graph TD
    %% ===== 外部資料 =====
    GM["🗺️ Google Maps URL"]
    UP["📸 使用者上傳照片\n店面 / 環境 / 品項 / 菜單"]

    %% ===== PHASE 1 =====
    subgraph P1["Prompt 1 ── Claude Code Desktop"]
        P1A["Web Fetch 抓取\nGoogle Maps 公開資料"]
        P1B{"資料完整？"}
        P1C["互動式向 User\n補齊缺漏欄位"]
        P1D["照片分類存入\nimages/ 資料夾"]
        P1E["菜單照片 OCR\n輸出 menu.json"]
        P1F["產生 restaurant.json\n+ DESIGN_BRIEF.md"]
    end

    %% ===== GITHUB =====
    GH[("☁️ GitHub Repo\nmain branch")]

    %% ===== PHASE 2 =====
    subgraph P2["Prompt 2 ── Claude Design"]
        P2A["讀取 Repo 資料\n與照片"]
        P2B["設計八個頁面區塊\nHero / 菜單 / 地圖 等"]
        P2C["產生 index.html\nCSS / JS"]
        P2D["Handoff to Claude Code"]
    end

    %% ===== PHASE 3 =====
    subgraph P3["Prompt 3 ── Claude Code Desktop"]
        P3A["接收 Handoff\n整合設計稿與 Repo"]
        P3B["補完動態功能\nfetch JSON / 地圖 embed / 輪播"]
        P3C["響應式檢查\n390px / 768px / 1280px"]
        P3D["建立 GitHub Actions\ndeploy.yml"]
        P3E["Push 所有檔案"]
        P3F["引導 User 開啟\nGitHub Pages"]
    end

    %% ===== 最終成果 =====
    OUT["🌐 網站上線\nGitHub Pages\n支援電腦 + iPhone 13+"]

    %% ===== 連線 =====
    GM --> P1A
    UP --> P1D

    P1A --> P1B
    P1B -->|"有缺漏"| P1C
    P1C --> P1B
    P1B -->|"完整"| P1D
    P1D --> P1E
    P1E --> P1F

    P1F -->|"git push"| GH

    GH -->|"讀取 restaurant.json\nimages/ DESIGN_BRIEF.md"| P2A
    P2A --> P2B
    P2B --> P2C
    P2C --> P2D

    P2D --> P3A
    P3A --> P3B
    P3B --> P3C
    P3C --> P3D
    P3D --> P3E
    P3E -->|"git push"| GH
    P3F -->|"設定 GitHub Pages\nSource: GitHub Actions"| GH

    P3E --> P3F
    GH -->|"GitHub Actions 自動部署"| OUT

    %% ===== 樣式 =====
    classDef external fill:#fff8e1,stroke:#f9a825,stroke-width:2px
    classDef github fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px
    classDef output fill:#e3f2fd,stroke:#1565c0,stroke-width:2px

    class GM,UP external
    class GH github
    class OUT output
```

## 節點說明

| 顏色 | 代表 |
|------|------|
| 黃色 | 外部輸入（Google Maps URL、使用者上傳照片） |
| 綠色 | GitHub Repo（三個階段的唯一交接媒介） |
| 藍色 | 最終成果（GitHub Pages 網站） |
| 白色 | 各階段的處理步驟 |

## 階段說明

| 階段 | 工具 | 主要任務 |
|------|------|---------|
| Prompt 1 | Claude Code Desktop | 資料收集、照片處理、菜單 OCR、push 到 GitHub |
| Prompt 2 | Claude Design | 讀取 Repo、設計網站、Handoff to Claude Code |
| Prompt 3 | Claude Code Desktop | 整合設計稿、補完功能、響應式檢查、部署上線 |
