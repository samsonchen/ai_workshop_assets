# Agentic AI Engineering Course Supplementary Materials

## Install Software

Please install the following software on your laptop before the course. If you encounter any difficulties, you can install them on the day of the class.

### If you're using MacOS
- Microsoft Visual Studio Code
- SourceTree for MacOS
- Docker Desktop
- uv:
    - curl -LsSf https://astral.sh/uv/install.sh | sh
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
    - Open PowerShell
    - wsl --install
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
- Update apt packages before any other installations

    ```
    sudo apt update
    ```

- Claude Code
- SSH Agent
    - sudo apt install keychain

    ```
    # add the following command to .profile
    eval "$(keychain --eval --quiet .ssh/id_ed25519)"
    ```
- uv:
    curl -LsSf https://astral.sh/uv/install.sh | sh
- npm:
    - sudo apt install nodejs npm -y
- unzip:
    - sudo apt install unzip
- pencil CLI:
    - npm install -g @pencil.dev/cli
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

## Additional Commands to check

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

### Using GitHub official Remote MCP (No Docker)

For Claude Code, this way is quicker but occupies more tokens to load all toolsets.

```text
  "mcpServers": {
    "github": {
      "type": "http",
      "url": "https://api.githubcopilot.com/mcp/",
      "headers": {
        "Authorization": "Bearer YOUR_GITHUB_PAT"
      }
    }
  },
```

For Claude Desktop, Since Claude Desktop supports only stdio, you need to bridge the command.

```text
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://api.githubcopilot.com/mcp/",
        "--header",
        "Authorization: Bearer ${GITHUB_PAT}"
      ],
      "env": {
        "GITHUB_PAT": "YOUR_GITHUB_PAT"
      }
    }
  },
```

### With Local Docker

More secured and you can control the toolsets you want.

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

### Bridge Docker Desktop

In the case you need to bridge your docker desktop, usually you don't, here are the procedures.

#### In Apple Silicon MacOS

- Start docker desktop
- ```sudo socat TCP-LISTEN:2375,reuseaddr,fork UNIX-CLIENT:/var/run/docker.sock```

#### WSL 1 / Windows 11 / UTM

- ```- export DOCKER_HOST=tcp://{MacOS_IP}:2375```
- Install Docker Client in WSL 1

```script
sudo apt update
sudo apt install -y ca-certificates curl gnupg lsb-release
sudo mkdir -p /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt install -y docker-ce-cli docker-buildx-plugin docker-compose-plugin
```

#### When to use Docker Bridge

If your environment is 100% macOS or 100% native Windows on an x86 CPU, then you don't need it. If you are running Windows in a virtual machine hosted by UTM on Apple Silicon macOS, you cannot use nested virtualization, which means you cannot run WSL 2 and Docker Desktop inside the Windows VM. You need to run Docker on macOS and bridge it to WSL 1 on Windows.


## Continue Learning

[Anthropic Academy](https://anthropic.skilljar.com)

## claude.formosa communities

### claude.formosa Discord Server

[https://discord.gg/fWcPCyMBta](https://discord.gg/fWcPCyMBta)

![claude.formosa Discord](./claude-formosa-qr-discord.png)

### claude.formosa Threads

[https://www.threads.com/@claude.formosa](https://www.threads.com/@claude.formosa)

![claude.formosa Threads](./claude-formosa-qr-threads.png)
