---
name: work-setup
description: Create a project workspace for organizing work across sessions.
argument-hint: "What are you working on? (e.g., 'for project login-refactor')"
---

# work-setup

Create a project workspace so you and future agents have a place to track tasks and organize work across sessions. Sets up the minimal structure needed by `work-handover` and `work-persist`.

## When to use

At the start of a new unit of work — a feature, bug, user story, refactoring, or any multi-step task. Run once. If the project already exists, this is a no-op.

## How the user invokes

```
initiate work-setup                     →  project name "default"
initiate work-setup for project login   →  project name "login"
initiate work-setup for proj login      →  project name "login"
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
  Status        : New project
  Resumption    : Read projects/<name>/HANDOVER.md first.
-->
```

If `tasks.md` already exists, do not modify it.

### 3. Optional: suggest additional docs

If the user asked for a fuller setup (e.g., said "full setup" or "with docs"), offer to create:

- `projects/<name>/README.md` — what this project is about, goals, completion criteria
- `docs/DECISIONS.md` — record of design decisions made

But only if the user explicitly wants them. Default is just the directory + tasks.md.

## What this skill does NOT do

- Does NOT write a handover document (that is `work-handover`).
- Does NOT persist architectural decisions or user preferences (that is `work-persist`).
- Does NOT create agent-instruction files (AGENTS.md, CLAUDE.md, etc.).
- Does NOT commit or push anything to git.

## Dependencies

Requires a writable directory with VCS marker (`.git` or similar). Creates `projects/<name>/` if absent.
