# Setup — MacOS

> Software 與 Cloud Service 清單見 [Software_and_Cloud_Service_List.md](Software_and_Cloud_Service_List.md)
> Windows 版見 [Setup_Windows.md](Setup_Windows.md)

這份文件涵蓋總表上**與作業系統有關**的安裝步驟。帳號申請、訂閱、Claude GitHub App 等與作業系統無關的項目，見 [Software_and_Cloud_Service_List.md](Software_and_Cloud_Service_List.md)，請先做完那一份再開始這裡。

## 1. 基本工具
Xcode CLI
```bash
xcode-select --install
```

Homebrew
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Git、Node.js、GitHub CLI
```bash
brew install git
```
```bash
brew install node
```
```bash
brew install gh
```

確認：
```bash
git --version
```
```bash
node --version
```
```bash
npm --version
```

## 2. 應用程式
已經手動裝過其中幾個的人不用先移除，也不用挑著跳過，整段照跑就好。`--adopt` 會把現有的 App 接管過來，不覆蓋你的設定。
```bash
brew install --cask --adopt claude
```
```bash
brew install --cask --adopt claude-code
```
```bash
brew install --cask --adopt visual-studio-code
```
```bash
brew install --cask --adopt google-chrome
```
```bash
brew install --cask --adopt obsidian
```
```bash
brew install --cask --adopt discord
```
```bash
brew install --cask --adopt sourcetree
```

確認 Claude Code：
```bash
claude --version
```
## 3. Chrome Extensions
裝好 Chrome 後手動加：
- [ ] [Claude in Chrome](https://claude.com/claude-in-chrome)
- [ ] [Obsidian Web Clipper](https://chromewebstore.google.com/detail/obsidian-web-clipper/cnjifjpddelmedmihgijeibhnjfabmlf)

## 4. Obsidian Plugins
Obsidian Plugin 要在有 Vault 下才能安裝，這裡先跳過，待課堂上再安裝。

## 5. Cloud Service CLI
Wrangler CLI
```bash
npm i -g wrangler
```
```bash
wrangler --version
```

Supabase CLI
```bash
brew install supabase/tap/supabase
```
```bash
supabase --version
```

## 6. GitHub Shell Login
先將 GitHub CLI、Git 與 SourceTree 安裝好才進行。
```bash
gh auth login
```
```bash
gh auth setup-git
```
`gh auth login` 選項建議：GitHub.com → HTTPS → 用 gh 認證 Git → 瀏覽器登入。

## 7. GitHub Claude Application
到 [GitHub Claude Application](https://github.com/apps/claude)，點 Configure 就可以了。

## 8. Deactivate Conda base
有用 conda 的人跑這行，沒用的跳過。
```bash
conda config --set auto_activate_base false
```

## 9. Python
1. uv（裝 Python，之後裝套件也用它）
```bash
brew install uv
```
```bash
uv --version
```
```bash
uvx --version
```
兩個都要印得出版本。

2. 裝 Python
```bash
uv python install --preview-features python-install-default --default 3.12
```
`--default` 不能省，沒有它只會裝出 `python3.12`，不會有 `python` 和 `python3`。

3. 把 uv 的指令放到 PATH 最前面
```bash
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc
```
```bash
exec zsh
```

4. 確認
```bash
python --version
```
```bash
python3 --version
```
兩行都要印出 `Python 3.12.x`。

如果印出來的不是 3.12，先看它指到哪：
```bash
which python3
```
要是 `/Users/<你的帳號>/.local/bin/python3`。不是的話，檢查 `~/.zshrc` 最後一行有沒有第 3 步那段 `export PATH`，然後重開終端機。

## 10. Notebooklm-py
裝完 Python 才可以裝這個。
```bash
uv tool install "notebooklm-py[browser]"
```
再下載 Chromium 瀏覽器本體，這步不能跳過：
```bash
uv tool run --from "notebooklm-py[browser]" playwright install chromium
```
確認：
```bash
notebooklm --version
```
印得出版本才算成功。
