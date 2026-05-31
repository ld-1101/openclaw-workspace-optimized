# PROMPT_RULES.md — Core Prompt Engineering Rules

> Auto-injected into every request. Derived from knowledge-base/prompt-engineering (56 docs).
> Last updated: 2026-05-29

## Universal Principles

### 1. Context Engineering over Prompt Writing
LLM is the CPU; context window is RAM; your job is the OS — load precise working memory for each task.
- Focus on: what documents, what history, what tools — not just phrasing.
- Most failures are context failures, not model failures.

### 2. CO-STAR Framework
Every complex prompt must define:
| Element | Meaning |
|---------|---------|
| **C**ontext | Background / situation |
| **O**bjective | What to achieve |
| **S**tyle | Writing style |
| **T**one | Emotional tone |
| **A**udience | Who will read it |
| **R**esponse | Output format |

### 3. Role Prompting — Be Specific
- ❌ "You are a programmer"
- ✅ "You are a full-stack Node.js developer with 5 years experience, specializing in automation tools. Write production-quality code with error handling."

### 4. Chain-of-Thought — Graded
Not every task needs CoT. Match effort to complexity:
- L0 (None): Simple classification, translation → 0 token CoT
- L1 (Micro): ~15 tokens → one-line reasoning
- L2 (Light): ~30 tokens → short reasoning
- L3 (Moderate): ~50 tokens → step-by-step
- L4 (Full): ~80 tokens → complete reasoning chain

**Rule**: Simple tasks with CoT REDUCE accuracy. Only use CoT for genuinely complex reasoning.

### 5. Few-Shot — Quality > Quantity
- 1-3 representative examples beats 10 generic ones
- Show edge cases and error handling
- Match the exact output format you want

### 6. Attention U-Curve
LLMs attend best to the BEGINNING and END of context. Middle content gets less attention.
- Put critical instructions at start or end
- Long context: re-emphasize key constraints near the end

### 7. Output Format — Be Explicit
- Specify exact output structure (JSON schema, markdown table, code block language)
- For code: specify language, style, error handling, edge cases
- "Return ONLY valid JSON, no markdown wrapping, no explanation"

### 8. Multi-Turn Memory
- Each turn should carry forward key decisions from previous turns
- Summarize long history instead of carrying all of it
- Reference prior decisions explicitly: "As decided earlier, we will..."

### 9. Error Recovery
- When uncertain, list assumptions and ask for confirmation
- Don't guess — if info is missing, state what's needed
- Provide fallback paths when a primary approach fails

## Channel-Specific Rules

### Code / Development
- Show reasoning before code
- Include error handling, edge cases, tests
- Use English for code, comments, commit messages

### Content / Writing
- Match the specified tone and audience exactly
- Provide alternatives when asked ("give me 3 options")
- Keep it concise unless length is requested

### Research / Analysis
- Cite sources explicitly
- Distinguish between fact, inference, and opinion
- Note confidence level for uncertain claims

## Anti-Patterns

- ❌ Over-explaining simple tasks
- ❌ Using CoT for classification
- ❌ Generic role definitions
- ❌ Ignoring context window limits
- ❌ Omitting output format specification
- ❌ Mixing unrelated tasks in one prompt
