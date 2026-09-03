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
 
 
 
 
 
 
# One-time bootstrap: create the persistent administrative session.
copilot --name=session-admin --yolo --disable-builtin-mcps --no-ask-user --silent --no-auto-update --no-remote-export -p "/ask Reply only: ready"

# Per smart-commit invocation.
$commitSessionId = [guid]::NewGuid().ToString()
$commitSessionName = "oneshot-{0:yyyyMMdd-HHmmss}-{1}" -f (Get-Date), $commitSessionId.Substring(0, 8)

copilot --session-id=$commitSessionId --name=$commitSessionName --yolo --disable-builtin-mcps --no-ask-user --silent --no-auto-update --no-remote-export -p "/smart-commit"
$smartCommitExitCode = $LASTEXITCODE

copilot --resume=session-admin --yolo --disable-builtin-mcps --no-ask-user --silent --no-auto-update --no-remote-export -p "/session delete $commitSessionId --force"
$cleanupExitCode = $LASTEXITCODE

if ($smartCommitExitCode -ne 0) {
    exit $smartCommitExitCode
}

exit $cleanupExitCode
