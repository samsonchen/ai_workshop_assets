# Setup — MacOS

> Software 與 Cloud Service 清單見 [Software_and_Cloud_Service_List.md](Software_and_Cloud_Service_List.md)
> Windows 版見 [Setup_Windows.md](Setup_Windows.md)

這份文件涵蓋總表上**與作業系統有關**的安裝步驟。帳號申請、訂閱、Claude GitHub App 等與作業系統無關的項目，見 [Software_and_Cloud_Service_List.md](Software_and_Cloud_Service_List.md)，請先做完那一份再開始這裡。

## 1. 基本工具
Xcode CLI
```
xcode-select --install
```
Homebrew
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```
Git、Node.js、GitHub CLI
```
brew install git
brew install node
brew install gh
```
確認：
```
git --version
node --version
npm --version
```

## 2. 應用程式
已經手動裝過其中幾個的人不用先移除，也不用挑著跳過，整段照跑就好。
```
brew install --cask --adopt claude
brew install --cask --adopt claude-code
brew install --cask --adopt visual-studio-code
brew install --cask --adopt google-chrome
brew install --cask --adopt obsidian
brew install --cask --adopt discord
brew install --cask --adopt sourcetree
```

### `--adopt` 在做什麼
`/Applications` 裡已經有同名 App 時，不加這個參數 brew 會直接停下來報 `It seems there is already an App at ...`。加了它，brew 會把現有那份接管成自己管理的，不覆蓋、不動你的設定與擴充套件。沒裝過的人加了也沒有副作用。

**即使是 adopt，brew 還是會先完整下載一次安裝檔**，所以你會看到幾百 MB 的下載進度。那是 brew 的固定流程，不代表它在覆蓋你的 App。看輸出裡有沒有這一行就知道：
```
==> Adopting existing App at '/Applications/Visual Studio Code.app'
```
有這行就是接管，不是重裝。

想確認的話：
```
brew list --cask
ls -la /Applications/Visual\ Studio\ Code.app
```
前者要列得出該 App，後者的目錄時間應該還是你當初安裝的日期，不是今天。

adopt 過程中 brew 有時會順手把 CLI 指令連到 PATH（例如 VS Code 的 `code`、`code-tunnel`），這是附帶的好處，原本要在 App 裡手動設定。

### 之後的更新
這些 App 大多有內建自動更新，會跟 brew 各管各的：App 自己升級後 brew 記錄的版本會落後，`brew upgrade` 可能報錯或想把它降回去。不想處理的話，讓 App 用自己的自動更新就好，不要對這些 cask 跑 `brew upgrade`。

確認 Claude Code：
```
claude --version
```
不想用 brew 的可以到各自官網下載安裝檔，見 [Software_and_Cloud_Service_List.md](Software_and_Cloud_Service_List.md) 的連結。

## 3. Chrome Extensions
裝好 Chrome 後手動加：
- [ ] [Claude in Chrome](https://claude.com/claude-in-chrome)
- [ ] [Obsidian Web Clipper](https://chromewebstore.google.com/detail/obsidian-web-clipper/cnjifjpddelmedmihgijeibhnjfabmlf)

## 4. Obsidian Plugins
(Obsidian Plugin 要在有 Vault 下才能安裝，如果還沒有可先跳過。)
Obsidian → 設定 → 第三方外掛 → 瀏覽，搜尋安裝：
- [ ] Claudian
- [ ] Mermaid Flow
- [ ] Mermaid Zoom
- [ ] Spaced Repetition

## 5. Cloud Service CLI
Wrangler CLI
```
npm i -g wrangler
wrangler --version
```
Supabase CLI
```
brew install supabase/tap/supabase
supabase --version
```

## 6. GitHub Shell Login
先將 GitHub CLI、Git 與 SourceTree 安裝好才進行。
```
gh auth login
gh auth setup-git
```
`gh auth login` 選項建議：GitHub.com → HTTPS → 用 gh 認證 Git → 瀏覽器登入。

## 7. Claude Desktop Connectors
Claude Desktop → Customize → Connectors，確認以下已整合：
- [ ] GitHub Integration
- [ ] Claude in Chrome

## 8. Deactivate Conda base
如果你有使用 conda，請不要自動啟動 base。如果你沒有使用 conda，請跳過此段。
```
conda config --set auto_activate_base false
```

## 9. Python
```
# 1. pyenv (Python version manager)
brew install pyenv

# 2. Shell integration — add to ~/.zshrc (or ~/.bashrc)
echo 'eval "$(pyenv init -)"' >> ~/.zshrc
source ~/.zshrc

# 3. Install a Python version
pyenv install 3.12.7
pyenv global 3.12.7

# 4. uv (package/env manager)
curl -LsSf https://astral.sh/uv/install.sh | sh
```

## 10. Notebooklm-py
裝完 Python 才可以裝這個。
```bash
pip install "notebooklm-py[browser]"
```
如果遇到 `externally-managed-environment` 錯誤（macOS Homebrew / Linux 常見），改用：
```bash
uv tool install "notebooklm-py[browser]"
# 或
pipx install "notebooklm-py[browser]"
```
確認指令跑得起來：
```
notebooklm --version
```
印得出版本才算成功。
