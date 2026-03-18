# rmBug Agent

The rmBug agent is a CLI that gives you instant, identity-based access to any database your team has connected to rmBug — no shared credentials, no VPN.

## Install

### macOS (Homebrew)

```bash
brew install rmbug/tap/rmbug-agent
```

### Linux (curl)

```bash
curl -fsSL https://rmbug.com/install.sh | sh
```

### Linux (apt)

```bash
curl -fsSL https://dl.rmbug.com/agent/apt/gpg.asc | sudo gpg --dearmor -o /usr/share/keyrings/rmbug.gpg
echo "deb [signed-by=/usr/share/keyrings/rmbug.gpg] https://dl.rmbug.com/agent/apt stable main" | sudo tee /etc/apt/sources.list.d/rmbug.list
sudo apt-get update && sudo apt-get install rmbug-agent
```

### Linux (yum / dnf)

```bash
sudo rpm --import https://dl.rmbug.com/agent/gpg.asc
sudo tee /etc/yum.repos.d/rmbug.repo << 'EOF'
[rmbug]
name=rmBug Agent
baseurl=https://dl.rmbug.com/agent/yum
enabled=1
gpgcheck=1
gpgkey=https://dl.rmbug.com/agent/gpg.asc
EOF
sudo dnf install rmbug-agent
```

### Windows (Winget)

```powershell
winget install rmBug.Agent
```

### Windows (MSI)

Download `rmbug-agent_<version>_windows_amd64.msi` from the [latest release](https://github.com/rmbug/agent/releases/latest).

### Direct download

Binaries and packages for all platforms are attached to each [GitHub release](https://github.com/rmbug/agent/releases).

## Quick start

```bash
# Log in to your rmBug account
rmbug login

# List databases you have access to
rmbug resources ls

# Connect to a database
rmbug connect my-postgres-db
```

`rmbug connect` starts a local proxy and prints the connection string. Paste it into psql, mysql, or any GUI tool — it just works.

## What happens when you connect

When you run `rmbug connect`:

1. The agent starts a local proxy on a random port (e.g. `127.0.0.1:15432`)
2. You get a connection string like `postgres://localhost:15432/mydb`
3. Connect with any native client — psql, Sequel Pro, DataGrip, whatever you use
4. The agent authenticates you with the rmBug Gateway on your company's infrastructure
5. The gateway connects to the actual database using the stored credentials

Your database credentials never touch your machine. Access is governed by your org's policies and every connection is logged in the audit trail.

## Usage

```bash
rmbug login                     # authenticate (opens browser)
rmbug logout                    # revoke the current session

rmbug resources ls              # list all databases you can access
rmbug resources ls --org myorg  # filter by organization

rmbug connect <resource>        # start a local proxy (interactive)
rmbug connect <resource> -d     # run proxy in background (detached)

rmbug status                    # show active proxy connections
rmbug disconnect <resource>     # stop a running proxy
```

## Configuration

The agent stores config in `~/.config/rmbug/` on Linux/macOS and `%APPDATA%\rmBug\` on Windows. `rmbug login` sets everything up automatically.

| Environment variable | Description |
|---------------------|-------------|
| `RMBUG_DEBUG` | Enable debug logging |
| `RMBUG_FORMAT` | Output format (`text` or `json`) |
| `RMBUG_QUIET` | Suppress non-essential output |
| `RMBUG_NO_COLOR` | Disable colour output |

## Full documentation

→ [rmbug.com/docs/agent](https://rmbug.com/docs/agent)
