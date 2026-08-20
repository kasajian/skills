---
name: smart-commit
description: Evaluates whether staged git changes exist. If staged changes exist, it builds a commit message exclusively from them. If no staged changes exist, it falls back to parsing all unstaged changes and untracked files in the working directory.
---

# Skill: Smart Commit Message Generator

## Purpose
Enforces intelligent context isolation during Git commit message drafting. It acts as a fallback router: targeting strictly staged changes (`git diff --cached`) when they exist, but gracefully broadening context to include unstaged and untracked files (`git diff` and `git status`) if the staging area is completely empty.

## When to Use
- When the user issues commands like `/smart-commit`, "write a commit message", or "draft a commit".
- When you need to prevent unrelated, unstaged working-tree changes from polluting a staged-only commit description.

## Inputs
- **Git Context**: The dynamic file modification, deletion, and addition maps pulled from the underlying operating system terminal environment via git sub-processes.

## Instructions
Execute the following pipeline sequentially when this skill is invoked:

1. **Verify Git Repository**: Confirm the active directory is an initialized git repository. If not, exit with an error.
2. **Scan Staging Area**: Execute `git diff --cached --quiet` to silently check if any modifications have been explicitly staged.
3. **Route Context Logic**:
   - **Scenario A (Staged Changes Exist)**: If the previous command yields a non-zero exit status, extract the context using exclusively `git diff --cached`. Do **NOT** scan or include unstaged files or untracked elements in the final summary.
   - **Scenario B (Staging Area is Empty)**: If the previous command yields an exit status of 0, extract context across the entire active working tree. Execute `git diff HEAD` for tracked modifications, and execute `git status --porcelain` to identify any untracked or newly introduced files.
4. **Draft the Commit Message**:
   - Write a clear, descriptive message strictly matching the [Conventional Commits](https://conventionalcommits.org) specification using the active context.
   - Structure format: `<type>[optional scope]: <short description>` followed by a concise body and footer(s) if change complexity or breaking changes warrant it.
   - **Allowed Types**: Use only `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`.
   - **Scope Derivation**: Derive scope from the primary domain, module, or component name (e.g., `auth`, `api`, `cli`, `config`). Omit the scope if changes span multiple unrelated areas or are repository-wide. Do not use file paths or generic terms (e.g., `code`, `files`).
   - **Breaking Changes**:
     - Append `!` after the type/scope (e.g., `feat(auth)!: replace session tokens with jwt` or `refactor!: overhaul config structure`).
     - Include a `BREAKING CHANGE: <explanation>` footer describing what broke and migration requirements.
   - **Secret Sanitization**: Never echo credentials, tokens, private keys, or raw secrets detected in the diff into the commit message; describe such changes abstractly.
   - Ensure the description is in the imperative mood, present tense, and limits the summary line length (<72 characters).

## Output Format
- Print the final formatted commit message directly to the standard output buffer.
- **Strict Rule**: Do not wrap the message in chat block preambles, introductory text (e.g., "Here is your message:"), or trailing operational chatter. 

## Examples

### Example 1: Staged Context Only
**Input Situation**: Running `git status` displays `index.js` as staged and `styles.css` as unstaged.
**Calculated Context**: Evaluates the diff of `index.js` only.
**Output**:
```text
feat(auth): integrate jwt verification middleware into base router

- Add jsonwebtoken validation block to incoming request pipeline
- Secure downstream user endpoints from unauthenticated access
```

### Example 2: Empty Staging Area Fallback
**Input Situation**: Running `git status` displays an empty index, but contains unstaged modifications to `utils.js` and an untracked file named `logger.js`.
**Calculated Context**: Evaluates the collective diff and file creation paths of both `utils.js` and `logger.js`.
**Output**:
```text
refactor(diagnostics): abstract log processing to separate utility file

- Move base console overrides out of primary core utils configuration
- Introduce logger.js template for structured runtime file streams
```
