# Troubleshooting Guide

**Hermes Agent** • MIT License

**Author**: Paolino Salamone

> Common issues and solutions for jira-cli installation, configuration, and usage.

---

## 🔍 Installation Issues

### Issue: `jira: command not found`

**Symptoms**: Running `jira` returns "command not found"

**Solutions**:

1. **Verify installation**:
   ```bash
   which jira
   ```
   If empty, binary not in PATH.

2. **Check if binary exists**:
   ```bash
   ls -la /usr/local/bin/jira
   ```

3. **Re-install binary**:
   ```bash
   chmod +x publish/jira
   sudo cp publish/jira /usr/local/bin/
   ```

4. **Add to PATH manually**:
   ```bash
   export PATH="/usr/local/bin:$PATH"
   # Add to ~/.bashrc or ~/.zshrc to persist
   ```

5. **Verify**:
   ```bash
   jira --version
   ```

---

### Issue: `Permission denied`

**Symptoms**: `bash: /usr/local/bin/jira: Permission denied`

**Solutions**:

```bash
# Make executable
chmod +x /usr/local/bin/jira

# Verify permissions
ls -la /usr/local/bin/jira
# Should show: -rwxr-xr-x

# Try again
jira --version
```

---

### Issue: `Cannot execute binary file` (wrong architecture)

**Symptoms**: `cannot execute binary file: Exec format error`

**Solutions**:

1. **Check your CPU architecture**:
   ```bash
   uname -m
   ```
   - `x86_64` → The provided binary is compatible ✅
   - `aarch64` → The provided binary may not be compatible
   - `arm7l` → The provided binary is not compatible

2. **Verify binary architecture**:
   ```bash
   file publish/jira
   # Should show: ELF 64-bit LSB executable, x86-64
   ```

3. **Use binary as-is for Linux x86-64**:
   ```bash
   chmod +x publish/jira
   sudo cp publish/jira /usr/local/bin/
   jira --version
   ```

**Note**: The binary is precompiled for Linux x86-64. Other architectures are not currently supported.

---

### Issue: `libc.so.6: version not found` (Linux)

**Symptoms**: `version 'GLIBC_2.34' not found`

**Solutions**:

1. **Check glibc version**:
   ```bash
   ldd --version
   ```

2. **Use WSL2 on Windows** (recommended):
    ```bash
    wsl --install
    cd /tmp && cp ~/publish/jira . && chmod +x jira
    ```

---

### Issue: `xattr: No such file` (macOS)

**Symptoms**: File cannot execute due to quarantine

**Solutions**:

```bash
# Remove quarantine attribute
xattr -d com.apple.quarantine /usr/local/bin/jira

# Verify
xattr -l /usr/local/bin/jira
# Should return nothing

# Try again
jira --version
```

---

## 🔐 Configuration Issues

### Issue: `Non-interactive mode: register site`

**Symptoms**: Running a jira command fails with "register site" message

**Solutions**:

```bash
# Add a site interactively
jira config add --site myco \
  --url https://myco.atlassian.net \
  --email me@myco.com \
  --token ATATT3xFfGF0...
```

Or set default:
```bash
jira config set-default --site myco
```

---

### Issue: `Cannot determine Jira site`

**Symptoms**: Command runs but can't find which site to use

**Solutions**:

1. **Use `--site` flag**:
   ```bash
   jira issue get PROJ-123 --site myco
   ```

2. **Set env var**:
   ```bash
   export JIRA_SITE=myco
   jira issue get PROJ-123
   ```

3. **Set default site**:
   ```bash
   jira config set-default --site myco
   jira issue get PROJ-123  # now works
   ```

4. **Verify which site is selected**:
   ```bash
   jira context
   ```

---

### Issue: Config file not found / auto-created

**Symptoms**: Config changes don't persist

**Solutions**:

1. **Check config path**:
   ```bash
   jira config path
   ```

2. **Create directory manually**:
   ```bash
   mkdir -p ~/.config/jira-cli
   ```

3. **Check permissions**:
   ```bash
   ls -ld ~/.config/jira-cli
   # Should be: drwx------
   chmod 700 ~/.config/jira-cli
   ```

4. **Add a site** (this creates config.json):
   ```bash
   jira config add --site myco --url ... --email ... --token ...
   ```

---

## 🔑 Authentication Issues

### Issue: `HTTP 401 Unauthorized` (Jira Cloud)

**Symptoms**: Requests fail with 401 error

**Solutions**:

1. **Regenerate API token**:
   - Go to [id.atlassian.com/manage-profile/security/api-tokens](https://id.atlassian.com/manage-profile/security/api-tokens)
   - Delete old token
   - Create new token
   - Update config: `jira config add --site myco ... --token NEW_TOKEN`

2. **Verify email is correct**:
   ```bash
   jira config list
   # Check "email" field matches your Atlassian account
   ```

3. **Check token format**:
   - Token should start with `ATATT` (Atlassian API token)
   - Ensure no extra spaces/newlines

---

### Issue: `HTTP 401 Unauthorized` (Jira Server/DC)

**Symptoms**: Bearer auth fails with 401

**Solutions**:

1. **Verify `--auth-mode bearer`**:
   ```bash
   jira context --site eng
   # Should show: Auth: bearer
   ```

2. **Regenerate PAT**:
   - Log in to Jira Server
   - Profile → Personal Access Tokens
   - Create new token
   - Update: `jira config add --site eng ... --token NEW_PAT --auth-mode bearer`

3. **Verify API version is 2**:
   ```bash
   # Edit ~/.config/jira-cli/config.json
   "eng": {
     "apiVersion": "2"  # must be "2" for bearer auth
   }
   ```

---

### Issue: `Error: '<' is an invalid start of a value`

**Symptoms**: Parsing error when making requests

**Solutions**:

This indicates **API version mismatch**:

1. **For bearer (Server/DC) auth, force API v2**:
   ```bash
   jira config add --site eng \
     --url https://jira.eng.com/jira \
     --token <PAT> \
     --auth-mode bearer
   
   # Manually edit ~/.config/jira-cli/config.json
   "apiVersion": "2"
   ```

2. **Verify context shows correct auth**:
   ```bash
   jira context --site eng
   # Should show: Auth: bearer, API: 2
   ```

3. **Try request again**:
   ```bash
   jira issue get PROJ-123 --site eng
   ```

---

### Issue: `HTTP 403 Forbidden`

**Symptoms**: Authentication works but insufficient permissions

**Solutions**:

1. **Check user permissions in Jira**:
   ```bash
   jira user me
   # Returns your account details
   ```

2. **Verify project permissions**:
   - Jira Cloud: Project → Project settings → Members
   - Jira Server: Project admin → Permissions

3. **Request elevated access** or ask your Jira admin

---

## 📥 Attachment Download Issues

### Issue: Attachment download returns `HTTP 401`

**Symptoms**: `jira issue download-attachment` fails with 401

**Solutions**:

This usually happens with **Jira Server + 2FA**:

1. **Get JSESSIONID**:
   - Open Jira Server in browser
   - Log in normally
   - Open DevTools (F12) → Storage or Application
   - Find `JSESSIONID` cookie
   - Copy the value

2. **Store in config**:
   ```bash
   jira config set-session-cookie --site eng --cookie ABC123XYZ456
   ```

3. **Try download again**:
   ```bash
   jira issue download-attachment PROJ-123 --id 1003046
   ```

---

### Issue: `All attachment download attempts failed`

**Symptoms**: Session cookie expired

**Solutions**:

1. **Session expired** — get fresh cookie:
   ```bash
   # In browser, re-login to Jira Server
   # Copy new JSESSIONID
   jira config set-session-cookie --site eng --cookie NEW_COOKIE
   ```

2. **Try again**:
   ```bash
   jira issue download-attachments PROJ-123 --output ~/Downloads
   ```

---

## 🚀 MCP Server Issues

### Issue: `OptionsValidationException on mcp serve`

**Symptoms**: MCP server fails to start with validation error

**Solutions**:

This is **already fixed** in the current version of jira-cli.

1. **Verify binary is current**:
   ```bash
   jira --version
   ```

2. **Reinstall from publish/jira**:
   ```bash
   chmod +x publish/jira
   sudo cp publish/jira /usr/local/bin/jira
   ```

3. **Test MCP server**:
   ```bash
   jira mcp serve
   ```

---

### Issue: MCP stdout corrupted

**Symptoms**: JSON-RPC messages appear corrupted

**Solutions**:

Ensure **no debug output** before MCP server starts:

1. **Check Program.cs** — no `Console.WriteLine` before server init
2. **Redirect logs to stderr**:
   ```csharp
   var loggerFactory = LoggerFactory.Create(builder =>
       builder.AddConsole(opts => opts.FormatterName = "simple")
              .SetMinimumLevel(LogLevel.Debug)
   );
   ```

3. **Test MCP manually**:
   ```bash
   jira mcp serve < integration/smoke-test.jsonl
   ```

---

### Issue: MCP tools not available in Claude Desktop

**Symptoms**: Claude doesn't see jira tools

**Solutions**:

1. **Verify config file exists**:
   ```bash
   # macOS
   cat ~/Library/Application\ Support/Claude/claude_desktop_config.json
   
   # Linux
   cat ~/.config/Claude/claude_desktop_config.json
   ```

2. **Check JSON syntax** (must be valid):
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

3. **Restart Claude Desktop** completely (not just quit)

4. **Check logs**:
   ```bash
   jira mcp serve 2>&1 | tee /tmp/mcp.log &
   # Make a request in Claude
   tail -f /tmp/mcp.log
   ```

---

## 🔍 General Debugging

### Enable Debug Mode (CLI only)

```bash
jira --debug issue get PROJ-123
```

Shows:
```
[DBG] → GET https://jira.myco.com/rest/api/3/issues/PROJ-123
[DBG] ← 200 OK (156 ms)
[DBG]   Headers: {...}
[DBG]   Body: {...}
```

---

### Check Jira Connection

```bash
# Basic connectivity test
jira context
# Shows site URL, auth mode, API version

# Test auth
jira user me
# Should return your Jira account details
```

---

### View Config File

```bash
jira config path
# Then view it
cat ~/.config/jira-cli/config.json
```

---

## 📞 Getting Help

If issues persist:

1. **Gather debug info**:
   ```bash
   jira --version
   jira config list
   jira --debug issue get PROJ-123 2>&1 | tee ~/debug.log
   ```

2. **Check logs**: Review `/tmp/jira-mcp.log` for MCP errors

3. **Read full docs**: [README.md](README.md)

4. **Search issues**: Check GitHub repo for similar problems

---

## ✅ Diagnostics Checklist

- [ ] `jira --version` works
- [ ] `jira config list` shows sites
- [ ] `jira context` shows correct site
- [ ] `jira user me` returns user details
- [ ] `jira issue get PROJ-123` retrieves an issue
- [ ] `jira mcp serve` starts without errors
- [ ] Claude Desktop recognizes jira MCP server

---

**License**: MIT  
**Hermes Agent** • Support Tier

**Author**: Paolino Salamone
