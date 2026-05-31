# Reference Documents

Place large reference files here. They are **NOT** injected into the prompt automatically, but can be read on demand via the `read` tool.

## Why Here?

Files in the workspace root (`*.md`) are automatically injected into every prompt. Large files waste context window and break cache when modified. Moving them here keeps the prompt lean.

## Usage

```bash
# Move a large file here
mv my-large-doc.md docs/reference/

# The agent can still read it when needed
# Just ask: "Read docs/reference/my-large-doc.md"
```
