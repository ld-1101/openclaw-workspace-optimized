# IRON_RULES.md - Workspace Iron Rules

> Companion to SOUL.md + AGENTS.md. Mandatory reading.

---

## Core References

| File | Purpose |
|------|---------|
| [SOUL.md](SOUL.md) | Persona, tone, 5-step task workflow, 8-step bug fix workflow |
| [AGENTS.md](AGENTS.md) | Workspace rules, memory policy, red lines |
| [PROMPT_RULES.md](PROMPT_RULES.md) | Prompt engineering standards |

---

## 10 Iron Rules

### Rule 1: Service Management - Check First
```
check → restart → curl test → journalctl verify → declare "done"
```
- Check status before restarting anything
- Verify with curl/HTTP check after restart
- Check logs for errors before claiming success

**⛔ NEVER use `taskkill /F /IM node.exe` — it kills the OpenClaw gateway process.**
- Target by PID: `taskkill /PID <pid>` (from `netstat -ano | findstr ":<port>"`)
- Target by port: stop the service via its own stop command (server.bat stop, etc.)
- If you must kill all node processes, warn the user first and get explicit approval

### Rule 2: Agent Communication
```
sessions_spawn    # Start new agent (isolated work)
sessions_send     # Send message to existing agent
subagents         # List/kill/steer agents
context:"fork"    # Only when child needs current transcript
```
- Tool-heavy work → spawn sub-agent, don't run inline

### Rule 3: Code Quality - File Separation
- **JS/HTML/CSS MUST be separate files**, not embedded in Python strings
- JS → `.js` file, CSS → `.css` file
- HTML → include via `<script src="...">`, `<link rel="stylesheet">`
- No inline code over 5 lines
- No template variable injection (`%%DATA%%`) in JS - use API calls instead

### Rule 4: Agent/Sub-agent Workspace
**Agent file boundaries:**
- `sessions_spawn` → isolated sub-agent for independent work
- `sessions_send` → cross-agent communication
- `subagents list/kill` → lifecycle management
- `context:"fork"` → only when child needs parent transcript

### Rule 5: Change Validation
Every change → verify → document:
```
1. State what changed and why
2. Verify: console logs, curl tests, status checks
3. Document: update relevant .md files
4. If bug introduced: revert and document
```
- Maximum 3 changes per batch before verification
- After 2 failed fix attempts: stop and escalate

### Rule 6: Context Budget — No Process Logs in Context

⛔ **Never leave full process logs, build output, or raw API responses in the conversation.**

```
❌ Bad: npm start full output (2-3k token) in context
❌ Bad: vite build full output (1k token) in context
❌ Bad: API test full JSON response in context
✅ Good: Only key summary (e.g. "3 Workers running, 116 endpoints")
✅ Good: >5 rounds troubleshooting → spawn isolated sub-session
```

**Rules:**
1. **Sub-session isolation** — troubleshooting/startup/testing >5 rounds must use sessions_spawn (context:isolated, cleanup:delete)
2. **Tool output truncation** — exec results: extract key lines only, don't keep full output
3. **Compact discipline** — context >80k token or >40 rounds → run /compact immediately
4. **Cost warning** — cost >$2.00 → warn and suggest compact

### Rule 7: Environment & Security
- API keys → `settings.json` or `.env`, NEVER in code
- Tokens/passwords → config files, NOT in logs or error messages
- When config looks wrong: verify with `curl` + logs before editing

### Rule 8: Memory Write Discipline
- Do NOT update MEMORY.md or daily files after every single turn
- Batch writes: collect items during session, write once at session end
- Exception: user explicitly says "remember this now" → write immediately

### Rule 9: Chat Behavior — When to Respond
**Respond when:**
- Directly mentioned or asked a question
- Can add genuine value
- Correcting important misinformation
- Summarizing when asked

**Stay silent when:**
- Late night (23:00-08:00) unless urgent
- User is clearly busy
- Nothing new since last check (<30 min)
- Conversation is flowing without you

### Rule 10: Reactions
- Max 1 emoji reaction per message
- Use as lightweight acknowledgment

---

> Last cleaned: 2026-05-31 — removed all references to non-existent scripts and files.
