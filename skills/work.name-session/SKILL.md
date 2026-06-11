---
name: work.name-session
description: Generate a short title (<=10 words) summarizing the current session's accomplishments for use as a session name.
argument-hint: none
---

# work-name-session

Generate a concise session title from the conversation history. Max ~10 words, abbreviated where obvious. Focus on what was actually built/designed/audited — not process.

## When to use

At the end of a session, when you want to name it for notes or handover. Invoke via `/name session`.

## How the user invokes

```
/name session
```

## Behavior

Review the **entire** conversation history from start to finish (not just recent messages). Identify every module designed/implemented, file created, structural change, or cross-cutting task. Condense into a comma-separated list of key modules/tasks, dropping the project name.

You MUST scroll back through the full conversation — do not rely on recency.

### Rules

1. Max ~10 words, fewer is better.
2. Abbreviate obvious terms (e.g., "ringbuf" for ring buffer, "checksum" for CRC32/SHA-1).
3. List the significant modules/tasks built. Omit minor documentation updates.
4. Do not say "and more" or "etc" — if it doesn't fit, drop the least significant item.
5. No full stops at the end.

### Examples

- `Arc: url,http,random,checksum,base64,path,ringbuf`
- `Arc: dir,sleep,mmap,temp,watch stubs`
- `Forge: meson.build, meson.options, all .c impls`
- `.h audit: metadata,agent blocks,xrefs`
