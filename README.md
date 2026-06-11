# skills

This repository contains reusable "skills" that can be installed with the `skills` CLI.

Available skills

To install all skills at once:

```bash
npx skills add kasajian/skills
```

To update all installed skills globally:

```bash
npx skills update --global
```

Or install individual skills:

- work.persist
  - Description: Summarize the current session and persist technical/architectural reasoning to the project's documentation.
  - Install:

    ```bash
    npx skills add -g kasajian/skills work.persist
    ```
  - Update (per-skill):

    ```bash
    npx skills update work.persist --global
    ```

- work.handover
  - Description: Write a handover document so a future session can pick up where this one left off.
  - Install:

    ```bash
    npx skills add -g kasajian/skills work.handover
    ```
  - Update (per-skill):

    ```bash
    npx skills update work.handover --global
    ```

- work.name-session
  - Description: Generate a short title (<=10 words) summarizing the current session's accomplishments for use as a session name.
  - Install:

    ```bash
    npx skills add -g kasajian/skills work.name-session
    ```
  - Update (per-skill):

    ```bash
    npx skills update work.name-session --global
    ```

- locate-files
  - Description: Fast file/folder name search on Windows using Voidtools Everything (es.exe). Use this to quickly find files or folders by name.
  - Install:

    ```bash
    npx skills add -g kasajian/skills locate-files
    ```
  - Update (per-skill):

    ```bash
    npx skills update locate-files --global
    ```

Notes

- The install command above assumes the `skills` CLI is available via npx and that you want a global install.
- When a new skill is added to this repository, update this README.md with the skill name, a one-line description, and the install command shown above.
