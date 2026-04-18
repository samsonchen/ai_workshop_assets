# AI Agentic Engineering Workshop

AI Agentic Engineering Workshop

我們相信 AI 不只是工具更是一場需要所有人參與的對話 

## Abstract

從零開始用 Claude Code 打造 AI Agentic 工作流。一天之內，帶你從 Prompt 到 Sub-Agent，從想法到能動的 Project。帶著筆電跟你的點子來就好。

## Speaker

Samson Chen / 陳紹俊
cloud.formosa 社群召集人
Care Active (Canada) CTO

## 課程大綱

- 快速上手 Claude Code
- Claude Code 與 Workflow
- MCP
- Sub-Agent and Skill
- AI Fluency
- AI 治理與安全使用

## 課前預備

### 想做的 Project

請準備 60 秒的介紹，介紹你想要做什麼 Project，這個 Project 需要具備 UI 及某種雲端功能，請不要事先動手做，課程當日才開始做。
一組至少準備一個 Project，或者如果你想做 One-Man Soldier 自己一人也歡迎。
介紹時間只有 60 秒，超過會被切斷，所以請在課程前專注準備這個 Project 的說明。

### Claude Pro Subscription

當日各組至少有一個 Claude Pro 的 Subscription Plan.

### 安裝軟體

請同學在當日要使用的筆電安裝下列軟體，有困難的部份，可以上課當日尋求協助

#### 如果你使用 MacOS
- Microsoft Visual Studio Code
- SourceTree for macOS
- Docker Desktop
- uv:
    curl -LsSf https://astral.sh/uv/install.sh | sh
- Claude Desktop
- Claude Code
- Pencil.dev Desktop
- [AWS CLI v2](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

VS Code Extensions:
- pencil.dev extension
- Jupyter

#### 如果你使用 Windows
- WSL2:
    Open PowerShell
    wsl --install
- Microsoft Visual Studio Code
- SourceTree for Windows
- Docker Desktop
- VS Code Extension: code -install-extension ms-vscode-remote.remote.wsl
- [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)
- [Windows Git](https://git-scm.com/install/windows)
- Claude Desktop
- Claude Code on Windows PowerShell
- Pencil.dev Desktop

VS Code Extensions:
- pencil.dev extension
- Jupyter

Install in WSL2:
- Claude Code
- SSH Agent
    sudo apt install keychain

    # add the following command to .profile
    eval "$(keychain --eval --quiet .ssh/id_ed25519)"
- uv:
    curl -LsSf https://astral.sh/uv/install.sh | sh
- npm:
    sudo apt update
    sudo apt install nodejs npm -y
- unzip:
    sudo apt install unzip
- pencil CLI:
    npm install -g @pencil.dev/cli
- [AWS CLI v2](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

### Generate SSH Key
    # make sure at user home
    mkdir .ssh
    chmod 700 .ssh
    cd .ssh
    ssh-keygen -t ed25519 -C "your_email@example.com"

### AWS Configuration
    # Use this command to configure AWS access key
    aws configure

    Default region name: us-west-2
    Default output format: json

### Complete git settings
    git config --global user.email "{your_email}"
    git config --global user.name "{your_fullname}"

### 請準備好下面服務的雲端帳號

請至少準備下面三種的雲端帳號，如果你的 Project 使用到其它的雲端帳號，那也請先準備好:

 - GitHub 
 - AWS (需要先提供信用卡, 但流量沒用超過不會收費)
 - Pencil.dev
