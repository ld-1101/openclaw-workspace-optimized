# TOOLS.md — Local Notes

## ⛔ Windows Service Management — SAFE Commands

> 2026-05-31: `taskkill /F /IM node.exe` kills the OpenClaw gateway. NEVER use it.

```bash
# SAFE: Kill process on specific port
for /f "tokens=5" %a in ('netstat -ano ^| findstr ":3456" ^| findstr "LISTENING"') do taskkill /PID %a /F

# SAFE: Stop via server.bat
C:\Users\zzz\.openclaw\workspace\douyin-bot-multi\packages\multi\server.bat stop

# SAFE: Kill by PID (look up first with netstat)
taskkill /PID <pid>

# DANGEROUS — NEVER USE:
# taskkill /F /IM node.exe   ← kills OpenClaw gateway!
```

## Active Services

| Service | Port | Path |
|---------|------|------|
| douyin-bot-multi | localhost:3456 | `douyin-bot-multi/` |
| OpenClaw Gateway | localhost:18789 | system service |

## Gateway

```bash
openclaw gateway status    # Check gateway status
openclaw gateway restart   # Restart gateway
```
