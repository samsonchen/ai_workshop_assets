# Setup — Windows

> Software 與 Cloud Service 清單見 [Software_and_Cloud_Service_List.md](Software_and_Cloud_Service_List.md)
> MacOS 版見 [Setup_MacOS.md](Setup_MacOS.md)

這份文件涵蓋總表上**與作業系統有關**的安裝步驟。帳號申請、訂閱、Claude GitHub App 等與作業系統無關的項目，見 [Software_and_Cloud_Service_List.md](Software_and_Cloud_Service_List.md)，請先做完那一份再開始這裡。

Windows 沒有 Homebrew，用 **winget** 裝 GUI 程式與大部分 CLI。以下指令都在 **PowerShell** 執行。先裝 [Windows Terminal](https://aka.ms/terminal)。

> 每次安裝完會改到 PATH 的工具（Claude Code、uv、gh、node），**要關掉 PowerShell 再開一個新的**，指令才找得到。不重開就會看到 `xxx is not recognized`。

整份流程不需要系統管理員權限。

## 1. winget
Windows 10 (1809 以上) 與 Windows 11 內建。確認：
```powershell
winget --version
```
沒有的話從 Microsoft Store 安裝「應用程式安裝程式 / App Installer」。

## 2. 基本工具
```powershell
winget install --id Git.Git
```
```powershell
winget install --id OpenJS.NodeJS.LTS
```
```powershell
winget install --id GitHub.cli
```

裝完重開 PowerShell，確認：
```powershell
git --version
```
```powershell
node --version
```
```powershell
npm --version
```
```powershell
gh --version
```

如果 npm 無法執行，需設定權限：
```powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

## 3. 應用程式
```powershell
winget install --id Anthropic.Claude
```
```powershell
winget install --id Microsoft.VisualStudioCode
```
```powershell
winget install --id Google.Chrome
```
```powershell
winget install --id Obsidian.Obsidian
```
```powershell
winget install --id Discord.Discord
```
```powershell
winget install --id Atlassian.Sourcetree
```
不想用 winget 的可以到各自官網下載安裝檔，見 [Software_and_Cloud_Service_List.md](Software_and_Cloud_Service_List.md) 的連結。

已經手動裝過的不用先移除。winget 靠「新增/移除程式」的記錄判斷，認得出來就會跳過（顯示 `Found an existing package already installed`），不會覆蓋也不會報錯。

有兩種情況會變成裝了兩份：手動裝的 installer 類型跟 winget package 不同（例如 MSIX 對 exe），或是一份裝在使用者層、一份在系統層。要指定層級可以加 `--scope user` 或 `--scope machine`。

全部裝完掃一次有沒有重複：
```powershell
winget list
```
真的重複了就從「新增/移除程式」移掉不要的那份，或 `winget uninstall --id <ID>`。

## 4. Claude Code
PowerShell：
```powershell
irm https://claude.ai/install.ps1 | iex
```

請注意看安裝過程的訊息，有可能需要編輯系統設定的環境變數 (environment variables) 裡面的 Path 項目。裝完**重開終端機**再執行：
```powershell
claude --version
```

## 5. Chrome Extensions
裝好 Chrome 後手動加：
- [ ] [Claude in Chrome](https://claude.com/claude-in-chrome)
- [ ] [Obsidian Web Clipper](https://chromewebstore.google.com/detail/obsidian-web-clipper/cnjifjpddelmedmihgijeibhnjfabmlf)

## 6. Obsidian Plugins
Obsidian Plugin 要在有 Vault 下才能安裝，這裡先跳過，待課堂上再安裝。

## 7. Cloud Service CLI
Wrangler CLI
```powershell
npm i -g wrangler
```
```powershell
wrangler --version
```

Supabase CLI — Windows 不支援 `npm install -g supabase`，要裝在專案資料夾裡：
```powershell
npm i supabase --save-dev
```
這一步等課堂上建好專案再做。之後所有 supabase 指令前面都要加 `npx`，例如 `npx supabase --version`。

## 8. GitHub Shell Login
先將 GitHub CLI、Git 與 SourceTree 安裝好才進行。
```powershell
gh auth login
```
```powershell
gh auth setup-git
```
`gh auth login` 選項建議：GitHub.com → HTTPS → 用 gh 認證 Git → 瀏覽器登入。

## 9. GitHub Claude Application
到 [GitHub Claude Application](https://github.com/apps/claude)，點 Configure 就可以了。

## 10. Python
1. uv（裝 Python，之後裝套件也用它）
```powershell
winget install --id astral-sh.uv
```
裝完重開 PowerShell。

2. 裝 Python
```powershell
uv python install 3.12
```

3. 讓 `python` 指到剛裝的那一版
```powershell
uv python update-shell
```
再重開一次 PowerShell。

4. 確認
```powershell
python --version
```
印得出 `Python 3.12.x` 就可以了。

如果印出來的不是 3.12，代表 PATH 上有別的 Python 排在前面。最常見的是 Windows 商店版：到「設定 → 應用程式 → 應用程式執行別名」，把 python.exe 和 python3.exe 的別名關掉，重開 PowerShell 再試一次。

## 11. Notebooklm-py
裝完 Python 才可以裝這個。
```powershell
uv tool install "notebooklm-py[browser]"
```
```powershell
uv tool update-shell
```
重開 PowerShell，再下載 Chromium 瀏覽器本體，這步不能跳過：
```powershell
uv tool run --from "notebooklm-py[browser]" playwright install chromium
```
確認：
```powershell
notebooklm --version
```
印得出版本才算成功。

設定檔位置為 `$HOME\.notebooklm\`。
