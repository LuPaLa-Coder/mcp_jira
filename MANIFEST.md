# 📦 Hermes Agent Documentation Manifest

**Hermes Agent** • MIT License  
**Author**: Paolino Salamone

> Public documentation map for `jira-cli` and its publishing workflow.

---

## 📁 Repository Structure

```text
mcp_jira/
├── README.md
├── INSTALLATION.md
├── CONFIGURATION.md
├── MCP-SETUP.md
├── TROUBLESHOOTING.md
├── SERVICES.md
├── MANIFEST.md
├── CHANGELOG.md
├── LICENSE
└── publish/
    └── jira
```

**Note**: `publish/jira` is the distributed binary and must remain unchanged during documentation publishing.

---

## 🗂️ Documentation Index

| File | Purpose |
|------|---------|
| `README.md` | Hub page with quick links, install steps, and command highlights |
| `INSTALLATION.md` | Binary installation and verification guide |
| `CONFIGURATION.md` | Jira Cloud / Server / Data Center authentication and config |
| `MCP-SETUP.md` | GitHub Copilot CLI and Claude Desktop MCP setup |
| `TROUBLESHOOTING.md` | Installation, auth, MCP, and runtime troubleshooting |
| `SERVICES.md` | 30 core MCP tools documented, with examples and workflows |
| `CHANGELOG.md` | Documentation suite history |
| `LICENSE` | MIT license text |

---

## 🔄 Publishing Rules

The Hermes Agent publishing workflow follows these principles:

1. Source documentation is sanitized before publication.
2. Local workstation paths and internal source references are removed.
3. `Hermes Agent`, `#HermesAgent`, author attribution, and technical content are preserved.
4. The public README stays aligned with the current CLI and MCP service surface.
5. The `publish/jira` binary is never modified by the documentation workflow.

---

## 🔐 Sanitization Checklist

Before publishing, verify that the generated docs do **not** contain:

- Absolute local filesystem paths
- Internal source folder references
- Internal-only labels or markers
- Broken links to unpublished internal material

Always keep:

- `Hermes Agent`
- `#HermesAgent`
- `Paolino Salamone`
- Author attributions
- Technical instructions and examples

---

## 📊 Current Documentation Scope

- **7 published docs** plus the main `README.md`
- **30 core MCP tools documented** in `SERVICES.md`
- **68 tools implemented overall** in the server codebase
- **193 WireMock command integration tests** in the validation suite
- **1 distributed binary** in `publish/jira`

---

## ✅ Publication Validation

After publishing, verify:

- No internal source references remain in `*.md`
- No absolute local workstation paths remain in `*.md`
- `Hermes Agent` branding remains visible
- `Paolino Salamone` attribution remains visible
- `publish/jira` is still present and unchanged

---

## 👤 Author Attribution Policy

All public documentation in this repository keeps the Hermes Agent signature and the original author attribution:

```markdown
**Hermes Agent** • MIT License
**Author**: Paolino Salamone
```

---

## 📄 License

MIT License — See [LICENSE](LICENSE)

---

**Hermes Agent** • Documentation Suite Reference  
**Author**: Paolino Salamone  
Last updated: 2026-05-27
