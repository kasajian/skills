AGENTS: Updating skills and documentation

Purpose

This file documents the expected behavior for automated agents (or humans acting as agents) working on this repository.

When adding or updating a skill

1. Add the skill code/files in the appropriate location.
2. Update README.md BEFORE creating any commit that introduces the new skill. The README entry should include:
   - skill name (folder/name)
   - one-line description (what it does)
   - install command exactly:

     ```bash
     npx skills add -g kasajian/skills <skill-name>
     ```
   - optional per-skill update command:

     ```bash
     npx skills update <skill-name> --global
     ```

3. If any tests or linters exist, run them locally and fix failures.
4. Prepare a clear commit message describing the new skill and the README update.
5. Do NOT commit or push changes without explicit user approval. Instead, prepare a PR or a patch and ask for the user's confirmation.

Entry template (copy/paste into README.md):

- <skill-name>
  - Description: <one-line description>
  - Install:

    ```bash
    npx skills add -g kasajian/skills <skill-name>
    ```
  - Update (per-skill):

    ```bash
    npx skills update <skill-name> --global
    ```

Agent responsibilities

- Keep README.md accurate and minimal. One line per skill plus install instructions is sufficient.
- When uncertain about phrasing, prefer brevity and clarity. Use imperative install instructions exactly as shown above.
- If adding multiple skills in one change, add entries for all new skills in README.md.
- When asked to make commits or push changes, always request explicit user permission first.

Based on this conversation (2026-05-23):
- The repository currently contains one skill: `locate-files`.
- When future skills are added, agents must update README.md before committing the skill code.
