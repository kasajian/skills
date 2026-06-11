---
name: work.handover
description: Write a handover document so a future session can pick up where this one left off.
argument-hint: "What project are you handing off? (e.g., 'for project login-refactor')"
---

# work-handover

Write a handover document so a future session (hours or days later) can pick up where this one left off without re-reading the full conversation history.

## When to use

At the end of a coding session when you expect a gap before continuing. Also useful mid-long-session to checkpoint state to disk.

## How the user invokes

```
initiate work-handover                    →  project name "default"
initiate work-handover for project login  →  project name "login"
initiate work-handover for proj login     →  project name "login"
```

## Project name detection

- If the user specifies a project name in their invocation (after "project" or "proj"), use that as `<name>`.
- If not, use `"default"`.
- Inform the user of the project name being used and the full path to the directory, so they know they can rename the folder later.

## Steps

### 1. Ensure project directory exists

Check if `projects/<name>/` exists under the repo root. If not, create `projects/` and `projects/<name>/`.

**Repo root detection:** Look for a `.git` directory or other VCS marker. If none found, ask the user where to place the `projects/` directory.

### 2. Ensure tasks.md exists

Check if `projects/<name>/tasks.md` exists. If not, create it with a minimal header:

```markdown
<!--
  ACTIVE SESSION STATE
  Agent         : (unknown)
  Last updated  : <today's date>
  Status        : First session
  Resumption    : Read projects/<name>/HANDOVER.md first.
-->
```

If `tasks.md` already exists, do not modify it.

### 3. Write HANDOVER.md

Create or overwrite `projects/<name>/HANDOVER.md` with the following sections:

1. **Onboarding protocol** — an ordered list of files the next agent MUST read before doing anything else. Include the following blocking-rule text verbatim:

   ```
   > **BLOCKING RULE**: Do NOT continue reading past this point, do NOT state your role, and do NOT acknowledge the user until you have read EVERY file in the list above cover-to-cover. If any file is missing, tell the user which one. This is mandatory — see AGENTS.md WARNING.
   ```

   The file list should be specific to the current project state; common entries include AGENTS.md, SCAFFOLD_PLAN.md, ARCHITECTURE.md, CONVENTIONS.md (repo root), and per-project files under `projects/<name>/`: PROJECT.md, tasks.md, DECISIONS_LOG.md, HANDOVER.md (this file).

2. **Session summary** — one paragraph. What was the goal? What module(s) were being worked on?
3. **What has been done** — bullet list of completed work items, files created or modified, stubs written.
4. **What is NOT done** — explicit bullet list of remaining work. Be specific (file names, function names, line numbers).
5. **Next action** — what the next session should do first, in detail. Include file paths, function signatures, relevant context. Precede the action with:

   ```
   > **IMPORTANT**: Complete the Onboarding protocol (section 1) first. Do NOT propose an approach, ask clarifying questions, or begin any task until the user explicitly instructs you to proceed. See AGENTS.md WARNING block.
   ```

6. **Watch-outs / traps** — known issues, gotchas, incomplete items, mid-session decisions that aren't obvious from reading code alone.
7. **Key file locations** — table of the most important files relevant to the current work and their purpose.
8. **Suggested skills for next session** — if the next session would benefit from specific skills, list them (e.g., ast-grep for structural search).

The template above is a guide, not a straitjacket. Tailor sections to what's actually relevant.

### 4. Suggest work-persist

After writing the handover, suggest that the user invoke `work.persist` to capture any architectural decisions or user preferences learned during the session. Frame it as optional:

> "If you made any design decisions or learned something about how this project should work, consider running `work.persist` to record them for future sessions."

## Edge cases

### Existing HANDOVER.md
Overwrite it. The previous content is superseded by the current session state.

### No repo root found
Ask the user where to place the `projects/` directory. Default to current working directory.

### tasks.md already exists
Do not modify it. Preserve existing task data.

## What this skill does NOT do

- Does NOT persist architectural decisions or user preferences (that is `work-persist`).
- Does NOT scaffold project conventions (`docs/DECISIONS.md`, etc.) — that is `work.setup`.
- Does NOT create agent-instruction files (AGENTS.md, CLAUDE.md, etc.).
- Does NOT commit or push anything to git.

## Dependencies

Requires a writable directory with VCS marker (`.git` or similar). Creates `projects/<name>/` if absent.
