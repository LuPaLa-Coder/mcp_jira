# jira-cli (mcp_jira)

**Hermes Agent** • MIT License  
**Author**: Paolino Salamone

> Production-ready Jira CLI + MCP server for GitHub Copilot CLI and Claude Desktop.

## 📚 Quick Links

- **Installation**: [INSTALLATION.md](INSTALLATION.md)
- **Configuration**: [CONFIGURATION.md](CONFIGURATION.md)
- **MCP Setup**: [MCP-SETUP.md](MCP-SETUP.md)
- **MCP Services**: [SERVICES.md](SERVICES.md) — 30 core MCP tools documented
- **Troubleshooting**: [TROUBLESHOOTING.md](TROUBLESHOOTING.md)
- **Documentation Manifest**: [MANIFEST.md](MANIFEST.md)
- **Changelog**: [CHANGELOG.md](CHANGELOG.md)

---

## 🎯 What is jira-cli?

**jira-cli** is a dual-mode tool:

1. **CLI Mode** — run Jira operations directly from the terminal
2. **MCP Server Mode** — expose Jira tools to GitHub Copilot CLI and Claude Desktop

### Highlights

- ✅ Jira Cloud + Jira Server/Data Center support
- ✅ Self-contained binary per platform (`publish/mac/`, `publish/linux/`, `publish/windows/`)
- ✅ MCP tools for issue, project, attachment, sprint, user, field, filter, webhook operations
- ✅ Multi-site configuration and context switching
- ✅ Attachment downloads, project metadata, issue type and status helpers
- ✅ GitHub Copilot CLI + Claude Desktop integration

---

## 🚀 Quick Start

### 1. Install the binary

```bash
# Choose your platform:
chmod +x publish/mac/jira && sudo cp publish/mac/jira /usr/local/bin/    # macOS
chmod +x publish/linux/jira && sudo cp publish/linux/jira /usr/local/bin/  # Linux
# Windows: copy publish\windows\jira.exe C:\Windows\System32\

jira --version
```

See [INSTALLATION.md](INSTALLATION.md) for platform-specific steps.

### 2. Configure Jira

```bash
jira config add --site myco \
  --url https://myco.atlassian.net \
  --email me@myco.com \
  --token ATATT3xFfGF0...
```

For Jira Server / Data Center:

```bash
jira config add --site eng \
  --url https://jira.eng.com/jira \
  --token <PersonalAccessToken> \
  --auth-mode bearer
```

See [CONFIGURATION.md](CONFIGURATION.md) for full configuration details.

### 3. Run a first command

```bash
jira issue get PROJ-123
jira search --jql "assignee = currentUser()"
```

### 4. Start MCP mode

```bash
jira mcp serve
```

Then follow [MCP-SETUP.md](MCP-SETUP.md) to register the server with GitHub Copilot CLI or Claude Desktop.

---

## 📋 CLI Command Highlights

### Issue management

```bash
jira issue get PROJ-123
jira issue create --project PROJ --type Task --summary "My task"
jira issue update PROJ-123 --summary "New title"
jira issue transition PROJ-123 --to "In Progress"
jira issue comment PROJ-123 --body "Ready for testing"
jira issue worklog PROJ-123 --time "2h 30m"
jira issue attach PROJ-123 --file ./screenshot.png
```

### Attachments and search

```bash
jira issue attachments PROJ-123
jira issue download-attachment PROJ-123 --id 1003
jira issue download-all-attachments PROJ-123
jira search --jql "project = PROJ AND status = Open" --max-results 10
```

### Project, issue type, and status commands

```bash
jira project list
jira project get PROJ
jira project statuses PROJ
jira project types
jira project roles PROJ
jira project role PROJ 10002
jira project properties PROJ
jira project features PROJ
jira issuetype list
jira issuetype list --project 10000
jira issuetype get 10001
jira status list
jira status get "In Progress"
jira status categories
```

### Other useful commands

```bash
jira board list
jira sprint get 10
jira user me
jira field list
jira context
jira config list
```

---

## 🔌 MCP Tool Coverage

The bundled MCP server documents these core tool groups in [SERVICES.md](SERVICES.md):

- **10 issue management tools**
- **4 attachment tools**
- **7 project tools**
- **2 issue type tools**
- **3 status tools**
- **1 sprint tool**
- **1 user tool**
- **1 context tool**
- **1 field reference tool**

Use this mode to ask Copilot or Claude to search issues, transition work, inspect project metadata, and manage attachments without typing raw Jira REST requests.

---

## 🗂️ Documentation Map

| File | Purpose |
|------|---------|
| [INSTALLATION.md](INSTALLATION.md) | Binary installation and verification |
| [CONFIGURATION.md](CONFIGURATION.md) | Jira Cloud / Server / Data Center setup |
| [MCP-SETUP.md](MCP-SETUP.md) | GitHub Copilot CLI and Claude Desktop integration |
| [SERVICES.md](SERVICES.md) | 30 documented MCP tools with examples |
| [TROUBLESHOOTING.md](TROUBLESHOOTING.md) | Common failures and recovery steps |
| [MANIFEST.md](MANIFEST.md) | Documentation suite structure and publishing notes |
| [CHANGELOG.md](CHANGELOG.md) | Documentation release history |

---

## 👤 Author & Credits

**Hermes Agent** documentation suite by **Paolino Salamone**

- ✅ Hermes Agent branding maintained
- ✅ Author attribution preserved
- ✅ MIT licensed documentation and binary distribution

---

## 📄 License

MIT License — See [LICENSE](LICENSE)

---

**Hermes Agent** • MCP Orchestration Tier  
**Author**: Paolino Salamone  
Last updated: 2025-01-15
