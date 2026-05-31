# MEMORY.md — Long-Term Memory

Curated long-term memories, decisions, insights. English-only as of 2026-05-29.

---

## ⛔ Critical Incidents

### 2026-05-31 — Gateway Kill by `taskkill /F /IM node.exe`
- **What happened**: Ran `taskkill /F /IM node.exe` to restart douyin-bot API server, killed the OpenClaw gateway process alongside it
- **Root cause**: `taskkill /F /IM node.exe` terminates ALL node.exe processes indiscriminately
- **Rule established**: NEVER use `/IM node.exe`. Always target by PID or use the service's own stop script
- **Propagated to**: IRON_RULES.md Rule 1, AGENTS.md Red Lines, TOOLS.md

---

## System

- **Host**: Windows 11, Intel i5-14400, 32GB RAM, NVMe SSD
- **Gateway**: deepseek/deepseek-v4-pro (primary), volcengine models (secondary)
- **Security**: Huorong (primary AV), Windows Defender (disabled)
- **Performance**: Excluded `%LOCALAPPDATA%\OpenClaw` from AV scanning (45K node_modules files)
- **Linux Server**: Decommissioned 2026-05-29 — all projects migrated or archived

---

## Rules (2026-05-30)

1. **Single-account douyin-bot disabled** — `douyin-bot/` is decommissioned. Only `douyin-bot-multi/` is active.

---

## Recent Chronology

### 2026-05-29 — Cleanup
- MEMORY.md Chinese encoding irrecoverably garbled
- **Decision**: All workspace .md files switch to English-only
- `.gitattributes` created for UTF-8 enforcement

### 2026-05-30 — FRP Decommission & Docker Cleanup
- FRP service removed from Linux server
- Docker + Containerd uninstalled (~280MB disk + ~50MB memory freed)

### 2026-05-31 — Workspace Cache Optimization
- Identified cache hit rate ~5% due to large prompt injection (~100KB)
- Moved 5 large reference docs to `docs/reference/`
- Deleted empty IDENTITY.md and USER.md
- Slimmed TOOLS.md (8KB → 1KB), MEMORY.md (8KB → 3KB)
- Full archive: `memory/archive/MEMORY-2026-05-31-full.md`

---

## Active Projects

| Project | Status | Notes |
|---------|--------|-------|
| douyin-bot-multi | Active | Multi-account Douyin AI comment assistant |
| knowledge-base | Active | 222 docs, 8 modules (submodule) |
| legal-kb | Active | Separate repo: `ld-1101/legal-kb` |

---

*Full chronology archived to `memory/archive/MEMORY-2026-05-31-full.md`*
