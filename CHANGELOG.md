# 📝 Hermes Agent - Changelog

**Author**: Paolino Salamone  
**Project**: Hermes Agent • Documentation Suite

All notable changes to the documentation suite are recorded here.

---

## [1.1.0] - 2025-01-15

### Added
- ✨ **Author Attribution**: Added "Paolino Salamone" as official author across all documentation files
- ✨ **Author & Credits Section**: New section in public README.md with author information and documentation policy
- ✨ **Author Retention Rule**: Updated HermesAgent.md sanitization rules to preserve author attribution during publishing workflow
- ✨ **MANIFEST.md Author Policy**: Documented author attribution policy and signature requirements
- ✨ **CHANGELOG.md**: Version tracking for documentation suite

### Updated
- 📝 **HermesAgent.md**: Enhanced with author attribution signature in header, sanitization rules, validation check, workflow diagram, and footer
- 📝 **MANIFEST.md**: Added author signature and updated tagging section to include author retention policy
- 📝 **README.md (Public)**: Added Author & Credits section, cleaned up duplicate sections
- 📝 **All .md files**: Added `**Author**: Paolino Salamone` to headers (all files)

### Policy Changes
- **Author Attribution**: All generated documentation now carries author signature: `**Author**: Paolino Salamone`
- **Sanitization Rule Update**: HermesAgent.md now preserves author name during publishing transformation
- **Signature Verification**: Phase 6 validation now checks for author attribution in all files

### Validation
- ✅ Documentation suite: author attribution present across source files
- ✅ Published docs: author attribution present across public files  
- ✅ Zero internal references in public docs (maintained)
- ✅ Hermes Agent branding: Maintained across all files
- ✅ Binary integrity: Preserved (70 MB)

---

## [1.0.0] - 2025-01-10

### Initial Release
- 🎯 **HermesAgent.md**: 7-phase orchestration prompt for automated publishing workflow
- 📦 **MANIFEST.md**: Internal documentation structure and publishing workflow reference
- 📚 **SERVICES.md**: Complete reference of 17 MCP tools (issue, attachment, project, field operations)
- 📖 **INSTALLATION.md**: Binary installation guide (source and public versions)
- 🔧 **CONFIGURATION.md**: Jira Cloud/Server authentication setup (source and public versions)
- 🔌 **MCP-SETUP.md**: Claude Desktop & GitHub Copilot CLI integration (source and public versions)
- 🆘 **TROUBLESHOOTING.md**: 50+ issue resolution scenarios (source and public versions)
- 🏗️ **Dual-doc model**: Source (development) + Public (distribution) with automated sanitization

### Features
- Automated sanitization workflow (7 phases)
- Internal reference removal (paths, internal mentions)
- Brand preservation (Hermes Agent tag)
- Binary immutability (70 MB executable preserved)
- Publication verification and reporting

---

## 📌 Documentation Signature

All changes in this changelog are attributed to:

**Hermes Agent** • MIT License  
**Author**: Paolino Salamone  
**Version**: 1.1.0  
**Last Updated**: 2025-01-15

---

## 🔄 Publishing Workflow

Every change follows the HermesAgent orchestration:

```
Source Doc Edit
    ↓
esegui HermesAgent.md (7-phase automation)
    ↓
Public Repo Update (sanitized + authored)
    ↓
Manual git commit/push
```

For details, see [HermesAgent.md](HermesAgent.md)
