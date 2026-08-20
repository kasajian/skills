# All platforms

## Github Copilot CLI
copilot --yolo --disable-builtin-mcps --no-ask-user --silent --no-auto-update -p "/smart-commit"
copilot --yolo --disable-builtin-mcps --no-ask-user --silent --no-auto-update -p "Draft a commit message using the /smart-commit skill, and then execute the git commit command to commit the changes using that message."

## Claude Code
claude --dangerously-skip-permissions -p "/smart-commit" 
claude --dangerously-skip-permissions -p "Draft a commit message using the /smart-commit skill, and then execute the git commit command to commit the changes using that message."

## Using agy (Google Antigravity CLI)
### Bash
agy --dangerously-skip-permissions --add-dir "$PWD" -p "/smart-commit"
agy --dangerously-skip-permissions --add-dir "$PWD" -p "Draft a commit message using the /smart-commit skill, and then execute the git commit command to commit the changes using that message."

### Windows
agy --dangerously-skip-permissions --add-dir "%CD%" -p "/smart-commit"
agy --dangerously-skip-permissions --add-dir "%CD%" -p "Draft a commit message using the /smart-commit skill, and then execute the git commit command to commit the changes using that message."

# Show last commit comment
git show -s --format=%B HEAD
  