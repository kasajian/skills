---
name: Local File Search (Everything)
description: Use this to instantly search for files, folders, or specific file extensions across the local Windows filesystem. Use when the search can be constructed using the available filters (text, path, folder, file, ext). If globs are necessary, use usual glob patterns instead.  Requires voidtools Everything es.exe installed.
version: 1.0
---

## Capabilities
You can search the local Windows filesystem instantaneously using Voidtools Everything (es.exe).

## Step-by-Step Execution
When the user asks to find a file, folder, or filter by file type:
1. Execute the `es.exe` command in the terminal using the following format:
   `es.exe [options] "search query"`
2. Parse the standard output to find the exact file paths. Output format: one full path per line.

## Search Filters
Search string can be:
- text : locates files or folders containing text
- path:text : locates files or folders path containing text
- folder:text : locates folder names containing text
- file:text : locates file names containing text
- ext:<ext1>[;<ext2>...] : locates files or folders where extension is ext1 (or ext2, etc.)

## Common ES.exe Options
- `-case`: Match case.
- `-p`: Match full path and filename.
- `-w`: Match whole words.
- `-ad`: Limit scope to folders.
- `-a-d`: Limit scope to files.
- `/o-d` or `/od`: Sort by date modified descending (newest first). Useful to get the most recent files.
- `-r`: Search using Regular Expressions.  Examples: a) find both hello.c or hello.js files: `es -r "\\hello.(c|js)$"`

## Example Usage
- To find files or folders with path containing "budget":
  `es.exe budget`
- To find files containing "budget":
  `es.exe file:budget`
- To find folders containing "budget":
  `es.exe folder:budget`
- To find excel files:
  `es.exe -a-d ext:xls;xlsx`
- To find pptx files where path contains "AppData": 
  `es.exe -a-d path:appdata ext:pptx`
- To find files named AGENTS.md:
  `es.exe -a-d -r "\\AGENTS.md$"`
- To find all .git folders:
  `es.exe -ad -r "\\\.git$"`

Whenever possible, prefer non-regex filters because they're faster
