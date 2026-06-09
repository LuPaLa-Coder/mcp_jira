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

### Step 1: Locate Binary

The precompiled binaries are in the `publish/` directory:

```bash
# Platform-specific binaries:
ls -lh publish/mac/jira      # macOS (arm64)
ls -lh publish/linux/jira    # Linux (x64)
ls -lh publish/windows/jira.exe  # Windows (x64)
```

Expected:
```
-rwxr-xr-x  jira  (74M)  # macOS
-rwxr-xr-x  jira  (68M)  # Linux
-rwxr-xr-x  jira  (69M)  # Windows
```

### Step 2: Copy to System PATH

```bash
# macOS
chmod +x publish/mac/jira
sudo cp publish/mac/jira /usr/local/bin/

# Linux
chmod +x publish/linux/jira
sudo cp publish/linux/jira /usr/local/bin/

# Windows (in WSL2 or terminal)
copy publish\windows\jira.exe C:\Windows\System32\
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
chmod +x publish/linux/jira
sudo cp publish/linux/jira /usr/local/bin/

# Verify
jira --version
```

### macOS (Apple Silicon / Intel)

```bash
chmod +x publish/mac/jira
sudo cp publish/mac/jira /usr/local/bin/

# Remove quarantine attribute (macOS security)
xattr -d com.apple.quarantine /usr/local/bin/jira

# Verify
jira --version
```

### Windows (x64)

```powershell
# From PowerShell (admin)
copy publish\windows\jira.exe C:\Windows\System32\

# Verify
jira --version
```

Or via WSL2:

```bash
cp publish/linux/jira /usr/local/bin/jira
chmod +x /usr/local/bin/jira
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
| `Binary not found` | Check platform dir: `ls -lh publish/mac/jira` or `publish/linux/jira` |
| `Cannot copy: Permission denied` | Use `sudo cp` or copy to home: `cp publish/linux/jira ~/bin/` |

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
