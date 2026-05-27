# MCP Server Setup Guide

**Hermes Agent** • MIT License

**Author**: Paolino Salamone

> Configure jira-cli as an MCP (Model Context Protocol) server for GitHub Copilot CLI and Claude Desktop.

---

## 📡 What is MCP?

**MCP (Model Context Protocol)** allows AI assistants to use jira-cli as a tool. Instead of typing CLI commands, you can ask GitHub Copilot or Claude Desktop to manage Jira issues directly.

### Example

```
You: "Create a task in Jira for the PROJ project called 'Fix login bug'"

Claude: (uses create_issue tool)
✓ Created PROJ-456
```

---

## 🚀 Start MCP Server Manually

```bash
jira mcp serve
```

Output:
```
[MCP] Server listening on stdio (JSON-RPC 2.0)
Ready for connections...
```

Press `Ctrl+C` to stop.

---

## 🔌 Register with GitHub Copilot CLI

### Step 1: Find Config File

GitHub Copilot CLI MCP servers config is located at:

```bash
~/.config/github-copilot/mcp-servers.json
```

Create the file if it doesn't exist.

### Step 2: Add jira-cli Configuration

```json
{
  "mcpServers": {
    "jira": {
      "command": "jira",
      "args": ["mcp", "serve"]
    }
  }
}
```

### Step 3: Restart GitHub Copilot CLI

```bash
# Kill any running Copilot processes
pkill -f "copilot"

# Start GitHub Copilot CLI again
copilot
```

### Step 4: Verify Registration

In GitHub Copilot CLI:
```
/context

Should show:
✓ jira (MCP) — ready
```

### Step 5: Test

In GitHub Copilot CLI:
```
Search issues in PROJ project assigned to me
```

GitHub Copilot will use `search_issues_jql` tool automatically.

---

## 🍎 Register with Claude Desktop (macOS)

### Step 1: Find Config File

Claude Desktop MCP servers config:

```bash
~/Library/Application\ Support/Claude/claude_desktop_config.json
```

### Step 2: Add jira-cli Configuration

```json
{
  "mcpServers": {
    "jira": {
      "command": "jira",
      "args": ["mcp", "serve"]
    }
  }
}
```

### Step 3: Restart Claude Desktop

- Close Claude Desktop completely
- Reopen it
- Claude will auto-connect to the MCP server

### Step 4: Verify

Look for a **⚙️** icon in Claude's interface (indicates MCP servers are loaded).

### Step 5: Test

```
In Claude: "Show me issues in the PROJ project"

Claude will use get_all_projects + search_issues_jql tools
```

---

## 🪟 Register with Claude Desktop (Windows/WSL2)

### Step 1: Find Config File

On Windows:
```
%APPDATA%\Claude\claude_desktop_config.json
```

On WSL2:
```bash
/mnt/c/Users/<Username>/AppData/Roaming/Claude/claude_desktop_config.json
```

### Step 2: Add Configuration

Same as macOS — use the JSON structure above.

### Step 3: Restart Claude Desktop

Restart the application and verify connection.

---

## 🐧 Register with Claude Desktop (Linux)

### Step 1: Find Config File

```bash
~/.config/Claude/claude_desktop_config.json
```

Create the directory if needed:
```bash
mkdir -p ~/.config/Claude
```

### Step 2: Add Configuration

```json
{
  "mcpServers": {
    "jira": {
      "command": "jira",
      "args": ["mcp", "serve"]
    }
  }
}
```

### Step 3: Restart Claude Desktop

Close and reopen the application.

---

## 🛠️ Advanced Configuration

### Custom Environment Variables

If jira-cli needs environment variables (e.g., custom config path):

```json
{
  "mcpServers": {
    "jira": {
      "command": "jira",
      "args": ["mcp", "serve"],
      "env": {
        "XDG_CONFIG_HOME": "/custom/path",
        "JIRA_SITE": "myco"
      }
    }
  }
}
```

### Default Site for MCP

Set MCP to always use a specific site:

```json
{
  "mcpServers": {
    "jira": {
      "command": "jira",
      "args": ["mcp", "serve"],
      "env": {
        "JIRA_SITE": "myco"
      }
    }
  }
}
```

---

## 📋 Available MCP Tools

When using jira-cli as an MCP server, these tools are exposed:

### Issues

- `get_issue` — Retrieve a single issue
- `create_issue` — Create a new issue
- `update_issue` — Update issue fields
- `delete_issue` — Delete an issue
- `assign_issue` — Assign to a user
- `transition_issue` — Change issue status
- `add_comment` — Add a comment
- `add_worklog` — Log time spent
- `add_attachment` — Attach a file

### Attachments

- `list_attachments` — List files on an issue
- `download_attachment` — Download a single file
- `download_all_attachments` — Download all files from an issue + sub-tasks

### Search

- `search_issues_jql` — Search using JQL (GET)
- `search_issues_jql_post` — Search using JQL (POST, for complex queries)

### Projects & Boards

- `get_all_projects` — List all projects
- `get_project` — Get project details
- `get_all_boards` — List boards
- `get_sprint` — Get sprint details

### Configuration

- `jira_context` — Show current site configuration
- `jira_config_list` — List all configured sites

---

## 🔍 Debugging MCP

### Check Logs

MCP server logs to stderr (safe for JSON-RPC on stdout):

```bash
jira mcp serve 2> /tmp/jira-mcp.log &
tail -f /tmp/jira-mcp.log
```

### Test MCP with Smoke Test

```bash
jira mcp serve < integration/smoke-test.jsonl
```

### Enable Debug Mode

Add debug env var:

```bash
JIRA_DEBUG=1 jira mcp serve
```

---

## 🆘 MCP Troubleshooting

| Issue | Fix |
|-------|-----|
| `MCP server not found` | Verify config file path and restart Claude/Copilot |
| `stdout corrupted` (MCP fails) | Ensure no `Console.WriteLine` before MCP starts; use stderr for logs |
| `OptionsValidationException on mcp serve` | Already fixed in current version — Polly timeout is 10s ✅ |
| `Connection refused` | Verify jira-cli is installed and in PATH: `which jira` |
| `Tools not showing in Claude` | Restart Claude Desktop after updating config.json |
| `Tool call fails silently` | Enable MCP logs (see "Check Logs" above) |

---

## 📚 Integration Flow

```
Your Command in Claude/Copilot
         ↓
Claude/Copilot selects MCP tool (e.g., search_issues_jql)
         ↓
JSON-RPC message sent to jira mcp serve (stdio)
         ↓
jira-cli executes tool (via Jira API)
         ↓
Result returned as JSON-RPC response
         ↓
Claude/Copilot displays result to you
```

---

## ✅ Verification Checklist

- [ ] `jira mcp serve` starts without errors
- [ ] MCP config file created at correct path
- [ ] jira-cli is in system PATH (`which jira`)
- [ ] Jira site configured (`jira config list`)
- [ ] Claude Desktop / GitHub Copilot CLI restarted
- [ ] MCP indicator visible in Claude/Copilot UI
- [ ] Test command: Ask AI to "list all Jira projects"

---

## 📚 Next Steps

- **Troubleshoot**: [TROUBLESHOOTING.md](TROUBLESHOOTING.md)
- **Full CLI Reference**: [README.md](README.md)
- **Configuration Guide**: [CONFIGURATION.md](CONFIGURATION.md)

---

**License**: MIT  
**Hermes Agent** • Integration Tier

**Author**: Paolino Salamone
