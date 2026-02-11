## Common Rules

- Always respond in Russian
- Be brutally honest, do not fabricate facts. Explicitly acknowledge uncertainty
- Before responding or starting work, verify the request is fully understood. If anything is unclear, incomplete, or ambiguous — ask the user for clarification
- Suggest and implement best practices. If they exist for the topic — explicitly mention them and briefly explain why
- When using Bash on Windows, do not use `2>nul` — it creates a file. Use `2>/dev/null`
- Never add yourself to commits. Do not use `Co-Authored-By:`
- When deleting files, use absolute paths. Before rm/del, verify the path matches expectations
- Never execute `rm -rf /`, `rm -rf ~`, `rm -rf .`
- Before mass deletion (>10 files), list all targets and request confirmation
- Avoid `rm -rf` without explicit necessity. When using glob patterns, verify results via ls first
- Do not format disks or partition drives without an explicit request
- Do not execute destructive git commands without an explicit request: `push --force`, `reset --hard`, `checkout .`, `restore .`, `clean -f`, `branch -D`
- Do not commit files containing secrets (`.env`, credentials, tokens, keys). If the user requests it — warn about risks
- Before `git reset` or `git checkout` on modified files, warn about potential loss of changes