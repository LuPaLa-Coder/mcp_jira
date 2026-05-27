# JiraCli Installation Guide

**Hermes Agent** • MIT License

**Author**: Paolino Salamone

> Fast and simple installation of **jira-cli** from the precompiled binary.

---

## 🚀 Quick Start

### Supported Platforms

- **Linux** (x86-64) — Primary support
- **macOS** (Intel, Apple Silicon) — Tested
- **Windows** (via WSL2) — Supported

### System Requirements

- **Disk space**: 70 MB
- **Runtime**: No external dependencies (self-contained)
- **.NET Runtime**: Not required

---

## 📦 Installation

### Step 1: Locate Binary

The precompiled binary is in the `publish/` directory:

```bash
cd mcp_jira
ls -lh publish/jira
```

Expected:
```
-rwxr-xr-x  jira  (70M)
```

### Step 2: Copy to System PATH

```bash
chmod +x publish/jira
sudo cp publish/jira /usr/local/bin/
```

**Verify installation**:
```bash
jira --version
```

Expected:
```
jira version 1.3.0 (.NET 8.0.0)
```

---

## 🖥️ Platform-Specific Steps

### Linux (x86-64)

```bash
# From mcp_jira directory
chmod +x publish/jira
sudo cp publish/jira /usr/local/bin/

# Verify
jira --version
```

### macOS (Intel & Silicon)

```bash
# From mcp_jira directory
chmod +x publish/jira
sudo cp publish/jira /usr/local/bin/

# Remove quarantine attribute (macOS security)
xattr -d com.apple.quarantine /usr/local/bin/jira

# Verify
jira --version
```

### Windows (WSL2)

```bash
# From mcp_jira directory in WSL2
chmod +x publish/jira

# Copy to home bin or system path
mkdir -p ~/bin
cp publish/jira ~/bin/
chmod +x ~/bin/jira

# Add to PATH (add to ~/.bashrc or ~/.zshrc)
echo 'export PATH="$HOME/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc

# Verify
jira --version
```

---

## ✅ Installation Verification

```bash
# Test 1: Version command
jira --version

# Test 2: Help output
jira --help | head -20

# Test 3: Config path (should exist after first config)
jira config path
# Expected: /home/user/.config/jira-cli/config.json
```

---

## 📋 Installation Checklist

- [ ] Binary located in `publish/jira`
- [ ] Binary is executable (`chmod +x`)
- [ ] Binary copied to `/usr/local/bin/` (Linux/macOS) or `~/bin/` (Windows)
- [ ] `jira --version` outputs version number
- [ ] `jira --help` shows command help
- [ ] Next: Follow [CONFIGURATION.md](CONFIGURATION.md)

---

## 🆘 Troubleshooting

| Issue | Solution |
|-------|----------|
| `jira: command not found` | Binary not in PATH. Use absolute path: `/usr/local/bin/jira --version` |
| `Permission denied` | Run `chmod +x /usr/local/bin/jira` |
| `Cannot execute binary` (macOS) | Run `xattr -d com.apple.quarantine /usr/local/bin/jira` |
| `Binary not found` | Verify in `mcp_jira/publish/jira` exists (70 MB size) |
| `Cannot copy: Permission denied` | Use `sudo cp` or copy to home: `cp publish/jira ~/bin/` |

---

## 🔗 Next Steps

1. **Configure Jira credentials**: [CONFIGURATION.md](CONFIGURATION.md)
2. **Set up MCP server**: [MCP-SETUP.md](MCP-SETUP.md)
3. **Troubleshoot issues**: [TROUBLESHOOTING.md](TROUBLESHOOTING.md)

---

**License**: MIT  
**Hermes Agent** • Installation Tier  

**Author**: Paolino Salamone
**Last Updated**: 2025-01-15
