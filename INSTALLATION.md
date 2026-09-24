# jira Installation Guide

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

### Step 1: Download Binary

The precompiled binaries are attached to each [GitHub Release](https://github.com/LuPaLa-Coder/mcp_jira/releases/latest):

| Platform | Asset | Size |
|----------|-------|------|
| macOS (arm64) | `jira-macos-arm64` | ~74 MB |
| Linux (x64) | `jira-linux-x64` | ~68 MB |
| Windows (x64) | `jira-windows-x64.exe` | ~69 MB |

```bash
# macOS
curl -L -o jira https://github.com/LuPaLa-Coder/mcp_jira/releases/latest/download/jira-macos-arm64

# Linux
curl -L -o jira https://github.com/LuPaLa-Coder/mcp_jira/releases/latest/download/jira-linux-x64
```

```powershell
# Windows (PowerShell)
Invoke-WebRequest -OutFile jira.exe https://github.com/LuPaLa-Coder/mcp_jira/releases/latest/download/jira-windows-x64.exe
```

**Verify integrity** (optional): compare with `SHA256SUMS` from the same release.

```bash
shasum -a 256 jira
```

### Step 2: Copy to System PATH

```bash
# macOS / Linux
chmod +x jira
sudo cp jira /usr/local/bin/

# Windows (PowerShell, admin)
copy jira.exe C:\Windows\System32\
```

**Verify installation**:
```bash
jira --version
```

Expected:
```
jira v1.2.0
```

---

## 🖥️ Platform-Specific Steps

### Linux (x64)

```bash
curl -L -o jira https://github.com/LuPaLa-Coder/mcp_jira/releases/latest/download/jira-linux-x64
chmod +x jira
sudo cp jira /usr/local/bin/

# Verify
jira --version
```

### macOS (Apple Silicon / Intel)

```bash
curl -L -o jira https://github.com/LuPaLa-Coder/mcp_jira/releases/latest/download/jira-macos-arm64
chmod +x jira
sudo cp jira /usr/local/bin/

# Remove quarantine attribute (macOS security)
xattr -d com.apple.quarantine /usr/local/bin/jira

# Verify
jira --version
```

### Windows (x64)

```powershell
# From PowerShell (admin)
Invoke-WebRequest -OutFile jira.exe https://github.com/LuPaLa-Coder/mcp_jira/releases/latest/download/jira-windows-x64.exe
copy jira.exe C:\Windows\System32\

# Verify
jira --version
```

Or via WSL2:

```bash
curl -L -o jira https://github.com/LuPaLa-Coder/mcp_jira/releases/latest/download/jira-linux-x64
chmod +x jira
sudo cp jira /usr/local/bin/
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

- [ ] Binary downloaded from the [latest release](https://github.com/LuPaLa-Coder/mcp_jira/releases/latest)
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
| `Binary not found` | Download the asset for your platform from the [latest release](https://github.com/LuPaLa-Coder/mcp_jira/releases/latest) |
| `Cannot copy: Permission denied` | Use `sudo cp` or copy to home: `cp jira ~/bin/` |

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
