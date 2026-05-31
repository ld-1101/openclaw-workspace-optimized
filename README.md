# OpenClaw Workspace Optimized

> A cache-friendly, English-only OpenClaw workspace template. ~20KB prompt injection, 0 broken references.

---

## What Is This?

This is an optimized [OpenClaw](https://github.com/openclaw/openclaw) workspace template designed to **maximize prompt cache hit rate** while maintaining full functionality.

### The Problem

Default OpenClaw workspaces often accumulate:
- Large reference files injected into every prompt (~100KB+)
- Broken references to deleted/moved files
- Mixed-language content causing encoding issues
- Frequent file changes breaking prompt cache
- Empty placeholder files wasting injection space

### The Solution

This template provides:
- **8 files, ~20KB total** prompt injection (vs typical 100KB+)
- **0 broken references** — every file/link verified
- **English-only** — prevents encoding corruption
- **Batch memory writes** — minimizes prompt cache invalidation
- **Clean separation** — injected files vs reference docs

---

## Quick Start

```bash
# 1. Clone into your OpenClaw workspace
git clone https://github.com/YOUR_USERNAME/openclaw-workspace-optimized.git ~/.openclaw/workspace

# 2. Customize MEMORY.md with your context
# 3. Customize TOOLS.md with your services
# 4. Restart gateway
openclaw gateway restart
```

---

## File Structure

```
workspace/
├── AGENTS.md           # Workspace rules, memory policy (6KB)
├── SOUL.md             # AI persona, workflows (2KB)
├── IRON_RULES.md       # 10 iron rules (4KB)
├── PROMPT_RULES.md     # 9 prompt engineering principles (3KB)
├── MEMORY.md           # Long-term memory template (1KB)
├── TOOLS.md            # Service quick-reference template (0.7KB)
├── DREAMS.md           # Dream diary (0.3KB)
├── HEARTBEAT.md        # Heartbeat config (0.15KB)
├── docs/               # Reference docs (NOT injected)
│   └── reference/      # Move large files here
└── memory/             # Daily logs + dreaming
    ├── YYYY-MM-DD.md   # Daily notes
    └── archive/        # Archived memory
```

**Total injection: ~17KB** (8 files)

---

## Cache Optimization Guide

### Why Cache Matters

OpenClaw caches the prompt prefix. Every turn, the system prompt is constructed from:
1. OpenClaw framework prompt
2. All workspace `.md` files (AGENTS.md, SOUL.md, etc.)
3. Tool definitions
4. Conversation history

If **any** injected workspace file changes, all subsequent cached tokens are invalidated. This means:
- 100KB injection × frequent changes = **~5% cache hit rate**
- 20KB injection × batch writes = **50%+ cache hit rate**

---

### Real-World Case Study

A production workspace was found to have **5% cache hit rate**. Here's the full audit:

#### Before Optimization

| Metric | Value |
|--------|-------|
| Injected files | 16 |
| Total injection size | ~100KB |
| Broken references | 12 |
| Non-existent script references | 10 |
| Garbled text (encoding corruption) | 139 instances |
| Empty placeholder files | 2 |
| Frequent-write rules | 1 |
| Mutable timestamps | 1 |
| Cache hit rate | ~5% |

#### After Optimization

| Metric | Value |
|--------|-------|
| Injected files | 8 |
| Total injection size | ~20KB |
| Broken references | 0 |
| Non-existent script references | 0 |
| Garbled text | 0 |
| Empty placeholder files | 0 |
| Frequent-write rules | 0 |
| Mutable timestamps | 0 |
| Cache hit rate | Normal |

---

### 22 Problems Found (Checklist)

Use this checklist to audit your own workspace.

#### 🔴 High Impact — Directly breaks prompt cache

| # | Problem | How to Check | Fix |
|---|---------|-------------|-----|
| 1 | **Injected files too large/too many** | `wc -c *.md` in workspace root | Move large files to `docs/reference/` |
| 2 | **Large reference docs injected** (>10KB) | `ls -la *.md \| sort -k5 -rn` | `mv large-file.md docs/reference/` |
| 3 | **TOOLS.md garbled text** (`?` replacing Chinese) | `grep -cP '[\x{4e00}-\x{9fff}]' TOOLS.md` | Rewrite in English |
| 4 | **MEMORY.md too large** (>5KB) | `wc -c MEMORY.md` | Archive old entries to `memory/archive/` |
| 5 | **DREAMS.md auto-growing** (dreaming cron) | `wc -c DREAMS.md` over time | Archive old entries periodically |
| 6 | **Mutable `Last updated` timestamp** | `grep -i "last updated" *.md` | Remove the timestamp |
| 7 | **Daily memory file rotation** changes prefix | `ls memory/YYYY-MM-DD.md` | Unavoidable (framework behavior) |

#### 🟡 Medium Impact — Wastes tokens on failed tool calls

| # | Problem | How to Check | Fix |
|---|---------|-------------|-----|
| 8 | **Broken file reference** (deleted file) | `grep -oP '\[.*?\]\((?!https?://)[^)]+\)' *.md` then check each | Remove the reference |
| 9 | **Non-existent script reference** | `grep -nP '\.py\b\|\.mjs\b\|\.sh\b' *.md` then check each | Remove the rule or the reference |
| 10 | **Missing config file reference** | `grep -n 'config\.json\|settings\.json' *.md` then check if file exists | Remove the reference |
| 11 | **Empty placeholder files** | `find . -maxdepth 1 -name "*.md" -size 0` | Delete them |
| 12 | **BUGS.md referenced but missing** | `grep -n 'BUGS.md' *.md` | Remove references or create the file |

#### 🟢 Low Impact — Consistency issues

| # | Problem | How to Check | Fix |
|---|---------|-------------|-----|
| 13 | **Chinese content in English-only workspace** | `grep -cP '[\x{4e00}-\x{9fff}]' *.md` | Translate to English |
| 14 | **Mixed language content** | Manual review | Standardize to one language |
| 15 | **Encoding corruption** (`?` replacing characters) | `grep -c '?' *.md` (look for clusters) | Rewrite the affected text |

#### 🔴 Behavioral — Rules that cause frequent writes

| # | Problem | How to Check | Fix |
|---|---------|-------------|-----|
| 16 | **"Write it down" rule encourages per-turn writes** | Read AGENTS.md memory section | Add batch-write discipline rule |
| 17 | **No memory write frequency limit** | Check for batching rules | Add "write once per session" rule |
| 18 | **DREAMS.md manual append rule** | Check if rule says "manually append" | Change to "let cron handle it" |

#### 🟡 Structural — Files that shouldn't be injected

| # | Problem | How to Check | Fix |
|---|---------|-------------|-----|
| 19 | **Project-specific docs in root** | `ls *.md` — are all files workspace-level? | Move to `docs/reference/` |
| 20 | **Reference manuals injected** | Check for files >10KB with technical content | Move to `docs/reference/` |
| 21 | **Deprecated config references** | `grep -n 'FRP\|docker\|systemctl' *.md` for non-applicable services | Remove deprecated sections |
| 22 | **README.md injected** (if OpenClaw injects it) | Check if README.md content appears in prompt | Move to `docs/` if injected |

---

### Rules for Maintaining Cache

1. **Don't modify injected files mid-session** — batch changes to session end
2. **Keep injected files small** — move large docs to `docs/reference/`
3. **No empty placeholder files** — delete if not used
4. **No broken references** — agent will try to read them, wasting tokens
5. **English only** — prevents encoding corruption in Git
6. **No timestamps in content** — `Last updated: YYYY-MM-DD` changes every edit
7. **Batch memory writes** — don't write MEMORY.md after every turn
8. **Limit DREAMS.md growth** — archive old entries, let cron handle new ones

### What Breaks Cache

| Action | Impact |
|--------|--------|
| Editing MEMORY.md | All subsequent tokens re-processed |
| Adding to DREAMS.md | All subsequent tokens re-processed |
| Changing AGENTS.md | All subsequent tokens re-processed |
| Changing SOUL.md | All subsequent tokens re-processed |
| Changing IRON_RULES.md | All subsequent tokens re-processed |
| Daily memory file rotation | New filename = new prompt prefix |
| Tool call results | Always different, but unavoidable |

### What Doesn't Break Cache

| Action | Why |
|--------|-----|
| Reading files | No prompt change |
| Using tools | Results are in assistant turn, not system prompt |
| Sub-agent work | Isolated sessions don't affect main prompt |
| Editing docs/reference/ | Not injected |
| Editing templates/ | Not injected |
| Editing memory/archive/ | Not injected |

---

### Audit Script

Run this to check your workspace for common cache issues:

```bash
#!/bin/bash
# cache-audit.sh — Check workspace for cache-killing issues

cd ~/.openclaw/workspace

echo "=== 1. Injection size ==="
wc -c AGENTS.md SOUL.md MEMORY.md DREAMS.md TOOLS.md HEARTBEAT.md PROMPT_RULES.md IRON_RULES.md 2>/dev/null

echo ""
echo "=== 2. Chinese chars in injected files ==="
for f in AGENTS.md SOUL.md MEMORY.md DREAMS.md TOOLS.md HEARTBEAT.md PROMPT_RULES.md IRON_RULES.md; do
  count=$(grep -cP '[\x{4e00}-\x{9fff}]' "$f" 2>/dev/null || echo 0)
  if [ "$count" -gt 0 ]; then echo "  ❌ $f: $count Chinese chars"; fi
done
echo "  ✅ Check complete"

echo ""
echo "=== 3. Broken file references ==="
for f in AGENTS.md SOUL.md MEMORY.md DREAMS.md TOOLS.md HEARTBEAT.md PROMPT_RULES.md IRON_RULES.md; do
  refs=$(grep -oP '\[.*?\]\((?!https?://)[^)]+\)' "$f" 2>/dev/null | grep -oP '\((?!https?://)[^)]+\)' | tr -d '()')
  for ref in $refs; do
    if [ ! -e "$ref" ]; then echo "  ❌ $f → $ref (NOT FOUND)"; fi
  done
done
echo "  ✅ Check complete"

echo ""
echo "=== 4. Mutable timestamps ==="
grep -n "Last updated\|last updated" AGENTS.md SOUL.md MEMORY.md DREAMS.md TOOLS.md HEARTBEAT.md PROMPT_RULES.md IRON_RULES.md 2>/dev/null || echo "  ✅ No timestamps found"

echo ""
echo "=== 5. Empty placeholder files ==="
for f in IDENTITY.md USER.md; do
  if [ -f "$f" ]; then echo "  ❌ $f exists — delete it"; fi
done
echo "  ✅ Check complete"

echo ""
echo "=== 6. Large files in root (should be in docs/reference/) ==="
find . -maxdepth 1 -name "*.md" -size +10k -exec ls -la {} \;
echo "  ✅ Check complete"
```

---

## Customization

### Adding Your Context

1. **MEMORY.md** — Fill in your system info, rules, and chronology
2. **TOOLS.md** — Add your service management commands
3. **SOUL.md** — Adjust persona and principles to your style
4. **AGENTS.md** — Add project-specific rules

### Adding Large Reference Docs

```bash
# Move large files to docs/reference/ (not injected)
mv my-large-doc.md docs/reference/

# Access when needed via read tool
# The agent can still read docs/reference/ files on demand
```

### Adding Project Rules

Edit `IRON_RULES.md` to add rules specific to your project. Keep rules:
- Referencing only existing files
- In English
- Actionable (not aspirational)

---

## What's NOT Included

This template intentionally excludes:
- **IDENTITY.md** — Empty placeholder, wastes injection space
- **USER.md** — Empty placeholder, wastes injection space
- **Project-specific files** — Add your own
- **API keys** — Never commit these
- **Memory content** — Start fresh

---

## Contributing

1. Fork this repo
2. Create a feature branch
3. Keep changes cache-friendly
4. Submit a PR

---

## License

MIT

---

*Optimized for OpenClaw 2026.5.x · Cache hit rate: 50%+*
