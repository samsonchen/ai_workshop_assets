# Setup — Windows

> Software 與 Cloud Service 清單見 [[Software_and_Cloud_Service_List.md]]
> MacOS 版見 [[Setup_MacOS.md]]

這份文件涵蓋總表上**與作業系統有關**的安裝步驟。帳號申請、訂閱、Claude GitHub App 等與作業系統無關的項目，見 [[Software_and_Cloud_Service_List.md]]，請先做完那一份再開始這裡。

Windows 沒有 Homebrew，用 **winget** 裝 GUI 程式與大部分 CLI。以下指令都在 **PowerShell** 執行。先裝 [Windows Terminal](https://aka.ms/terminal)。

> 每次安裝完會改到 PATH 的工具（Claude Code、pyenv、gh、node），**要關掉 PowerShell 再開一個新的**，指令才找得到。不重開就會看到 `xxx is not recognized`。

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
winget install --id OpenJS.NodeJS.LTS
winget install --id GitHub.cli
```
裝完重開 PowerShell，確認：
```powershell
git --version
node --version
npm --version
gh --version
```

## 3. 應用程式
```powershell
winget install --id Anthropic.Claude
winget install --id Microsoft.VisualStudioCode
winget install --id Google.Chrome
winget install --id Obsidian.Obsidian
winget install --id Discord.Discord
winget install --id Atlassian.Sourcetree
```
不想用 winget 的可以到各自官網下載安裝檔，見 [[Software_and_Cloud_Service_List.md]] 的連結。

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
CMD：
```
curl -fsSL https://claude.ai/install.cmd -o install.cmd && install.cmd && del install.cmd
```
裝完**重開終端機**再執行：
```powershell
claude --version
```
不需要系統管理員權限，也不需要 WSL。Git for Windows 建議先裝好，Claude Code 會用 Git Bash 當 Bash tool；沒裝的話會改用 PowerShell 當 shell。

VS Code 內建終端機若遇到 Claude Code 啟動後沒反應，改用獨立的 Windows Terminal 或 PowerShell。

## 5. Chrome Extensions
裝好 Chrome 後手動加：
- [ ] [Claude in Chrome](https://claude.com/claude-in-chrome)
- [ ] [Obsidian Web Clipper](https://chromewebstore.google.com/detail/obsidian-web-clipper/cnjifjpddelmedmihgijeibhnjfabmlf)

## 6. Obsidian Plugins
Obsidian → 設定 → 第三方外掛 → 瀏覽，搜尋安裝：
- [ ] Claudian
- [ ] Mermaid Flow

## 7. Cloud Service CLI
Wrangler CLI
```powershell
npm i -g wrangler
wrangler --version
```

Supabase CLI — Windows 不支援 `npm install -g supabase`。裝在專案資料夾裡，用 `npx` 執行：
```powershell
npm i supabase --save-dev
npx supabase --version
```
之後所有 supabase 指令前面都要加 `npx`，而且要在這個專案資料夾裡跑：
```powershell
npx supabase init
npx supabase start
```
不加 `npx` 直接打 `supabase` 會找不到。

## 8. GitHub Shell Login
先將 GitHub CLI、Git 與 SourceTree 安裝好才進行。
```powershell
gh auth login
gh auth setup-git
```
`gh auth login` 選項建議：GitHub.com → HTTPS → 用 gh 認證 Git → 瀏覽器登入。

## 9. Claude Desktop Connectors
Claude Desktop → Customize → Connectors，確認以下已整合：
- [ ] GitHub Integration
- [ ] Claude in Chrome

## 10. Python
```powershell
# 1. pyenv-win (PowerShell)
Invoke-WebRequest -UseBasicParsing -Uri "https://raw.githubusercontent.com/pyenv-win/pyenv-win/master/pyenv-win/install-pyenv-win.ps1" -OutFile "./install-pyenv-win.ps1"; &"./install-pyenv-win.ps1"

# 2. 關掉 PowerShell，重開一個新的，然後確認
pyenv --version

# 3. Install a Python version
pyenv install 3.12.7
pyenv global 3.12.7
python --version

# 4. uv (package/env manager)
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"

# 5. 再重開一次 PowerShell
uv --version
```
若第 1 步被擋下，先執行 `Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser`。

Windows 商店版的 Python（打 `python` 會跳出 Microsoft Store 那個）會跟 pyenv-win 打架。到「設定 → 應用程式 → 應用程式執行別名」把 python.exe / python3.exe 的別名關掉。

## 11. Notebooklm-py
裝完 Python 才可以裝這個。notebooklm-py 是純 Python + Playwright，Windows 可以正常使用。
```powershell
py -m pip install "notebooklm-py[browser]"
playwright install chromium
```
或用 uv：
```powershell
uv tool install "notebooklm-py[browser]"
```
與 macOS 的差別：
- 用 `py -m pip install`，不要用 `pip install`
- Windows 沒有 `externally-managed-environment` 問題，不需要 `--break-system-packages`
- 設定檔位置為 `%USERPROFILE%\.notebooklm\`
