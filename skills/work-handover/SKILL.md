---
name: work-handover
description: Write a handover document so a future session can pick up where this one left off — either an ephemeral chat code block for an immediate fresh-context handoff, or a durable in-repo file to resume days later. Use when hitting context limits, switching focus, ending a work session, checkpointing mid-session, or handing a project to another session. Invoke with 'handoff' for the ephemeral chat version, or 'for project NAME' to persist under projects/.
argument-hint: "handoff for an ephemeral chat code block, or 'for project NAME' to persist in-repo (e.g., 'for project login-refactor')"
---

# work-handover

One skill, two modes. Write a handover document so a future session can pick up where this one left off without re-reading the full conversation history. The document is context for the next agent, not a command queue.

- **Ephemeral mode** ("handoff") — output the document as a single chat code block, copy-pasteable into a fresh agent session. No repo required. Use for context-limit hops, switching focus, or partitioning a task across fresh contexts.
- **Durable mode** ("for project NAME") — persist the document at `projects/<name>/HANDOVER.md` in the repo. Use for resuming hours or days later, checkpointing mid-session, or handing a project to another session.

## How the user invokes

```
initiate work-handover handoff                    →  ephemeral mode (chat code block)
initiate work-handover for project login          →  durable mode, project "login"
initiate work-handover for proj login             →  durable mode, project "login"
initiate work-handover                            →  durable mode, project "default"
```

**Mode decision:**
- Argument mentions "handoff", "fresh session", or "new context" → **ephemeral**.
- Argument says "for project NAME" or "for proj NAME" → **durable**, project NAME.
- No argument → **durable**, project "default".

Inform the user of the mode being used; in durable mode, also the project name and full path to the directory (they can rename the folder later).

## Core principles (both modes)

1. **State, not instructions.** Describe what *is true*, not what the next agent *should do*. Write "Login flow is implemented; logout is not started" — never "Implement logout next." The next agent decides actions; you give it ground truth.
2. **Reference, don't duplicate.** Before writing, read AGENTS.md / CLAUDE.md and any prior handover. Do **not** restate anything already covered there — the document is session-specific only. Point to files, ADRs, issues, and commits by path instead of re-embedding their content.
3. **Traps & Dead Ends.** Approaches that already FAILED, and things the next agent will be tempted to do wrong. This is the least recoverable information — code shows *what*; only this session remembers *what failed and why*.
4. **Redact secrets.** Strip API keys, tokens, passwords, and PII. Reference where credentials live (e.g. ".env.local, not committed") — never their values.
5. **Be ruthless.** Every line must be something the next agent cannot trivially get by reading the code or project config. Cut anything obvious, redundant, or explanatory.

## Document template (both modes)

Fill in every section. Omit a section only if it is genuinely empty — mark it `None`. Phrase everything as status, not actions:

1. **Goal & session summary** — What are we ultimately trying to accomplish (1–3 sentences)? What did this session work on?
2. **Why this matters / background** — motivation and constraints. Skip anything already covered in repo docs (principle 2).
3. **Current state** — what is DONE, PARTIAL, NOT STARTED:
   - DONE: OAuth login flow, tests passing locally
   - PARTIAL: Session persistence — store wired up, refresh logic missing
   - NOT STARTED: Logout endpoint
4. **Key decisions (and why)** — choices made and the reasoning, including rejected approaches.
5. **Traps & Dead Ends** — failed approaches and temptations, e.g. "Tried mocking the DB in integration tests — flaky, abandoned for a test container" or "Do NOT bump the SDK to v3 — it breaks the streaming API".
6. **Relevant files & onboarding list** — ordered list of files the next agent must read before acting. Durable mode: repo-root files (AGENTS.md, conventions), per-project files under `projects/<name>/` (PROJECT.md, DECISIONS_LOG.md if they exist), with this HANDOVER.md last. Ephemeral mode: the code files that matter, with line ranges and what is *specifically* there (e.g. `src/auth/oauth.ts:L40-L88 — provider config + token exchange`). Be specific to the current state, not generic boilerplate.
7. **Open work** — what remains, described as state and ordering — NOT a command list:
   - Logout endpoint is not yet implemented
   - Session persistence depends on the logout endpoint existing first

End the document with exactly:

> Read every file listed in section 6. If any is missing, say so. Then wait for my instructions before taking any action. Treat every claim in this document as context to verify against the code, not facts to trust blindly.

## Ephemeral mode

1. If a project config file exists (AGENTS.md / CLAUDE.md / equivalent), read it first. Do **not** restate anything already covered there.
2. If the user references an existing handoff, read and update it rather than starting from scratch.
3. Fill the template, then output the ENTIRE document as a **single fenced code block** in the chat so the user can copy it in one click.
4. Also save a copy to the OS temp directory — `$TMPDIR/handoff-<random-8-chars>.md` (or the system temp dir equivalent) — never into the repo working tree.
5. Tell the user the absolute path. A fresh session can start with:

   ```
   Read the file <absolute-path> to get the context, then wait for instructions.
   ```

No repo is required for this mode.

## Durable mode

1. **Read project config and any prior handover.** Read AGENTS.md / CLAUDE.md / equivalent first. If `projects/<name>/HANDOVER.md` exists, read it before writing — carry forward anything still true (done work, decisions, traps) instead of starting from a blank page.
2. **Ensure the project directory exists.** Check `projects/<name>/` under the repo root; create `projects/` and `projects/<name>/` if absent. **Repo root detection:** look for a `.git` directory or other VCS marker. If none found, ask the user where to place the `projects/` directory (default: current working directory).
3. **Write `projects/<name>/HANDOVER.md`** using the shared template (section 6 becomes the onboarding list, HANDOVER.md last). Overwrite the previous file — it is superseded by the current session state, minus anything still true that you carried forward.
4. **Suggest work-persist (optional):** "If you made any design decisions or learned something about how this project should work, consider running `work-persist` to record them for future sessions."

## Edge cases

- **Existing HANDOVER.md** — read it first, then overwrite with carry-forward (durable mode).
- **No repo root found** — ask the user where to place the `projects/` directory; default to cwd (durable mode).

## What this skill does NOT do

- Does NOT persist architectural decisions or user preferences (that is `work-persist`).
- Does NOT scaffold project conventions (that is `work-setup`).
- Does NOT commit or push anything to git.

## Dependencies

- Ephemeral mode: none — writes only to the OS temp directory.
- Durable mode: a writable directory with a VCS marker (`.git` or similar); writes `projects/<name>/HANDOVER.md`, creating the directory if absent.
