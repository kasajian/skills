# kasajian/skills

This repository contains reusable "skills" that can be installed with the `skills` CLI.

Available skills

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

Global update

- To update all installed skills globally:

  ```bash
  npx skills update --global
  ```

Notes

- The install command above assumes the `skills` CLI is available via npx and that you want a global install.
- When a new skill is added to this repository, update this README.md with the skill name, a one-line description, and the install command shown above.
