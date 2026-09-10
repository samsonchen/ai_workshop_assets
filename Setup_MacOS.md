# Setup — MacOS

> Software 與 Cloud Service 清單見 [[Software_and_Cloud_Service_List.md]]
> Windows 版見 [[Setup_Windows.md]]

這份文件涵蓋總表上**與作業系統有關**的安裝步驟。帳號申請、訂閱、Claude GitHub App 等與作業系統無關的項目，見 [[Software_and_Cloud_Service_List.md]]，請先做完那一份再開始這裡。

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
(如果已經安裝過部份套件，下列動作不會再裝第二次，也不需要改用此種方法安裝)
```
brew install --cask --adopt claude
brew install --cask --adopt claude-code
brew install --cask --adopt visual-studio-code
brew install --cask --adopt google-chrome
brew install --cask --adopt obsidian
brew install --cask --adopt discord
brew install --cask --adopt sourcetree
```
`--adopt` 是給已經手動裝過的人用的。`/Applications` 裡已經有同名 App 時，不加這個參數 brew 會直接停下來報 `It seems there is already an App at ...`；加了它會把現有那份接管成 brew 管理，不覆蓋、不動你的設定。沒裝過的人加了也沒有副作用。

裝完之後 App 內建的自動更新會跟 brew 各管各的：App 自己升級後 brew 記錄的版本會落後，`brew upgrade` 可能報錯或想把它降回去。不想處理的話，讓 App 用自己的自動更新就好，不要對這些 cask 跑 `brew upgrade`。

確認 Claude Code：
```
claude --version
```
不想用 brew 的可以到各自官網下載安裝檔，見 [[Software_and_Cloud_Service_List.md]] 的連結。

## 3. Chrome Extensions
裝好 Chrome 後手動加：
- [ ] [Claude in Chrome](https://claude.com/claude-in-chrome)
- [ ] [Obsidian Web Clipper](https://chromewebstore.google.com/detail/obsidian-web-clipper/cnjifjpddelmedmihgijeibhnjfabmlf)

## 4. Obsidian Plugins
Obsidian → 設定 → 第三方外掛 → 瀏覽，搜尋安裝：
- [ ] Claudian
- [ ] Mermaid Flow

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
如果遇到 `externally-managed-environment` 錯誤（macOS Homebrew / Linux 常見），官方 skill 不建議用 `--break-system-packages`，改用：
```bash
uv tool install "notebooklm-py[browser]"
# 或
pipx install "notebooklm-py[browser]"
```
需安裝 Chromium 瀏覽器
```
# pip 安裝者
playwright install chromium
```
