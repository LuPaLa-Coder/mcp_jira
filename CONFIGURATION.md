# jira Configuration Guide

**Hermes Agent** • MIT License

**Author**: Paolino Salamone

> Step-by-step configuration for Jira Cloud, Jira Server, and Data Center instances.

---

## 📍 Configuration File Location

```
~/.config/jira-cli/config.json
```

Or set custom location:
```bash
export XDG_CONFIG_HOME=/custom/path
# Then config lives at: /custom/path/jira-cli/config.json
```

Show current path:
```bash
jira config path
```

---

## 🔐 Authentication Methods

### Auth Mode: `basic` (Jira Cloud)

**Header**: `Basic Base64(email:token)`  
**API Version**: 3 (auto)

```json
{
  "sites": {
    "myco": {
      "siteUrl": "https://myco.atlassian.net",
      "email": "me@myco.com",
      "apiToken": "ATATT3xFfGF0...",
      "authMode": "basic",
      "defaultProjectKey": "MYCO"
    }
  }
}
```

### Auth Mode: `bearer` (Jira Server / Data Center)

**Header**: `Bearer <PersonalAccessToken>`  
**API Version**: 2 (auto)

```json
{
  "sites": {
    "eng": {
      "siteUrl": "https://jira.eng.com/jira",
      "apiToken": "<PersonalAccessToken>",
      "authMode": "bearer",
      "apiVersion": "2"
    }
  }
}
```

---

## 🚀 Setup Jira Cloud (Recommended)

### Step 1: Generate API Token

1. Go to [id.atlassian.com/manage-profile/security/api-tokens](https://id.atlassian.com/manage-profile/security/api-tokens)
2. Click **Create API token**
3. Name it: `jira-cli`
4. Copy the token (you'll only see it once!)

### Step 2: Add Configuration

```bash
jira config add \
  --site myco \
  --url https://myco.atlassian.net \
  --email me@myco.com \
  --token ATATT3xFfGF0...
```

### Step 3: Verify

```bash
jira config list
jira context
```

Expected output:
```
Site: myco
URL: https://myco.atlassian.net
Auth: basic (Jira Cloud)
```

---

## 🖥️ Setup Jira Server / Data Center

### Step 1: Generate Personal Access Token

1. Log in to your Jira Server/DC instance
2. Click your profile icon → **Profile**
3. Go to **Personal Access Tokens** (left sidebar)
4. Click **Create token**
5. Name: `jira-cli`
6. Expiry: Choose an appropriate duration
7. Copy the token

### Step 2: Add Configuration

```bash
jira config add \
  --site eng \
  --url https://jira.eng.com/jira \
  --token <PersonalAccessToken> \
  --auth-mode bearer
```

### Step 3: Verify

```bash
jira config list
jira context --site eng
```

Expected output:
```
Site: eng
URL: https://jira.eng.com/jira
Auth: bearer (Jira Server/DC)
API Version: 2
```

---

## ⚙️ Full Configuration Schema

```jsonc
{
  "defaultSite": "myco",                    // optional — used when --site not specified
  "defaultApiVersion": "3",                 // fallback (auto-selected per auth mode)
  "defaultAgileApiVersion": "1.0",          // for board/sprint operations
  "timeoutSeconds": 30,                     // HTTP timeout
  "sites": {
    "myco": {
      "siteUrl": "https://myco.atlassian.net",  // no trailing slash
      "email": "me@myco.com",                   // Jira Cloud only
      "apiToken": "ATATT3xFfGF0...",            // API token
      "authMode": "basic",                      // basic (Cloud) or bearer (Server/DC)
      "defaultProjectKey": "MYCO",              // optional
      "apiVersion": "3",                        // optional (auto-set per auth mode)
      "sessionCookie": "<JSESSIONID>",          // optional (for 2FA attachment downloads)
      "lastUpdated": "2025-01-15T10:00:00Z"     // auto-updated by CLI
    },
    "eng": {
      "siteUrl": "https://jira.eng.com/jira",
      "apiToken": "<PersonalAccessToken>",
      "authMode": "bearer",
      "apiVersion": "2",                        // usually "2" for Server/DC
      "sessionCookie": null,                    // if 2FA enabled
      "lastUpdated": "2025-01-15T10:00:00Z"
    }
  }
}
```

---

## 🔄 Site Selection Priority

When running a command without `--site`:

1. `--site <name>` flag (highest priority)
2. `JIRA_SITE=<name>` environment variable
3. `defaultSite` in config.json
4. The only configured site (if exactly one exists)

### Examples

```bash
# Use default site
jira issue get PROJ-123

# Use env var
JIRA_SITE=eng jira issue get PROJ-123

# Use explicit flag
jira issue get PROJ-123 --site myco

# Set default site
jira config set-default --site myco
```

---

## 📋 Common Commands

### List All Sites

```bash
jira config list
```

Output:
```
myco  (default)
  URL: https://myco.atlassian.net
  Auth: basic

eng
  URL: https://jira.eng.com/jira
  Auth: bearer
```

### Add a New Site

```bash
# Jira Cloud
jira config add --site prod \
  --url https://prod.atlassian.net \
  --email devops@company.com \
  --token ATATT_...

# Jira Server/DC
jira config add --site legacy \
  --url https://jira.legacy.com \
  --token <PAT> \
  --auth-mode bearer
```

### Remove a Site

```bash
jira config remove --site old-site
```

### Set Project Default

```bash
jira config add --site myco \
  --url https://myco.atlassian.net \
  --email me@myco.com \
  --token ATATT_... \
  --project PROJ
```

Now you can omit `--project`:
```bash
jira issue create --type Task --summary "My task"  # uses PROJ automatically
```

### Change Default Site

```bash
jira config set-default --site eng
```

---

## 🔐 2FA Setup (Jira Server/DC Attachment Downloads)

If your Jira Server has 2FA enabled and you need to download attachments:

### Step 1: Get Session Cookie

1. Open your Jira Server in a browser
2. Log in normally
3. Open **Developer Tools** (F12) → **Storage** or **Application**
4. Find cookie named `JSESSIONID`
5. Copy its value

### Step 2: Store in Config

```bash
jira config set-session-cookie --site eng --cookie <JSESSIONID>
```

Or manually edit `~/.config/jira-cli/config.json`:
```json
{
  "sites": {
    "eng": {
      "sessionCookie": "ABC123XYZ456..."
    }
  }
}
```

### Step 3: Test

```bash
jira issue download-attachment PROJ-123 --id 1003046 --output ~/Downloads
```

---

## 🆘 Configuration Troubleshooting

| Issue | Fix |
|-------|-----|
| `Cannot determine Jira site` | Set `JIRA_SITE` or use `--site`, or run `jira config set-default --site NAME` |
| `Config file not found` | File auto-created on first `jira config add`. Verify path: `jira config path` |
| `HTTP 401 (Unauthorized)` | Token expired or invalid. Regenerate at https://id.atlassian.com or Jira profile |
| `HTTP 401 with bearer auth` | Verify `--auth-mode bearer` and that PAT is still valid. Check `jira context` |
| `Error: '<' is an invalid start of a value` | API version mismatch. Ensure bearer auth uses API v2. Set `"apiVersion": "2"` explicitly |
| `Cannot update config.json` | Check file permissions: `ls -l ~/.config/jira-cli/config.json` |
| `Site name contains invalid characters` | Use alphanumeric + hyphens only (no spaces or special chars) |

---

## 📚 Next Steps

- **Get MCP running**: [MCP-SETUP.md](MCP-SETUP.md)
- **Troubleshoot issues**: [TROUBLESHOOTING.md](TROUBLESHOOTING.md)
- **Run commands**: See [README.md](README.md) for full CLI reference

---

**License**: MIT  
**Hermes Agent** • Configuration Tier

**Author**: Paolino Salamone
