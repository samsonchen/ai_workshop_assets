# Agentic AI Engineering Course Supplementary Materials

## Install Software

Please install the following software on your laptop before the course. If you encounter any difficulties, you can install them on the day of the class.

### If you're using MacOS
- Microsoft Visual Studio Code
- SourceTree for MacOS
- Docker Desktop
- uv:
    curl -LsSf https://astral.sh/uv/install.sh | sh
- Claude Desktop
- Claude Code
- Pencil.dev Desktop
- [GitHub CLI via HomeBrew](https://github.com/cli/cli/blob/trunk/docs/install_macos.md#homebrew)
- [AWS CLI v2](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)

VS Code Extensions:
- pencil.dev extension
- Jupyter

### If you're using Windows

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
- Claude Code on Windows Powershell
- Pencil.dev Desktop

VS Code Extensions:
- pencil.dev extension (with WSL MCP)
- Jupyter

Install in WSL2:
- Claude Code
- SSH Agent
    sudo apt install keychain

     ```#add the following command to .profile```
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
- [GitHub CLI](https://github.com/cli/cli/blob/trunk/docs/install_linux.md)
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
    git config --global user.name "{your fullname}"

## Prepare Cloud Service Accounts

Please prepare accounts for at least the following three cloud services. If your project uses other cloud services, prepare those accounts as well:

 - GitHub 
 - AWS (requires credit card, but no charges if usage doesn't exceed free tier)
 - Pencil.dev

## Addtional Commands to check

### GitHub CLI

- gh auth login

### on WSL2

- Add ssh agent to .profile:
  eval "$(ssh-agent -s)" 
- Install GitHub CLI
  https://github.com/cli/cli/blob/trunk/docs/install_linux.md

## GitHub Tokens

Two types of security tokens are needed

### SSH and CPG keys

Settings -> SSH and GPG keys

#### create key

```script
ssh-keygen -t ed25519 -f {key_name}
```

- Put key pairs in ~/.ssh
- make sure the mode of .ssh is 700
- Paste the public key on GitHub

#### load key in OS

```script
ssh-add ~/.ssh/{key_name}
```

### Personal access tokens (PAT)

Settings -> Developer settings -> Personal access tokens -> Fine-grained tokens

Generate the token for the MCP to use

## MCP setting

```text
   "mcpServers": {
    "github": {
      "command": "docker",
      "args": [
        "run",
        "-i",
        "--rm",
        "-e",
        "GITHUB_PERSONAL_ACCESS_TOKEN={your_GitHub_PAT}",
        "ghcr.io/github/github-mcp-server"
      ]
    }
  },
```

## Continue Learning

[Anthropic Academy](https://anthropic.skilljar.com)

## claude.formosa communities

### claude.formosa Discord Server

[https://discord.gg/fWcPCyMBta](https://discord.gg/fWcPCyMBta)

![claude.formosa Discord](./claude-formosa-qr-discord.png)

### claude.formosa Threads

[https://www.threads.com/@claude.formosa](https://www.threads.com/@claude.formosa)

![claude.formosa Threads](./claude-formosa-qr-threads.png)
